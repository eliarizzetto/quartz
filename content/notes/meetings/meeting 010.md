---
title: "Meeting 010"
tags:
- timeline
- meeting
date: 2022-10-31 15:47
---
<span 
		class="ob-timelines"
		data-date="2022-10-31-00">
</span>
📑 [**Reference session: 010**](notes/sessions/session%20010.md)

🔙 [**Previous meeting: 000**](notes/meetings/meeting%20009.md)


### Report degli errori: aggiornato il dizionario per la posizione
Ho aggiornato i JSON schema[^1] secondo le indicazioni di [meeting 009](notes/meetings/meeting%20009.md), modificando il dizionario `position` all'interno del report in modo che abbia:
* una voce `located_in` che fa riferimento al tipo di posizione (se l'errore nasce in un confronto tra rows, `located_in` avrà valore `row`; se nasce da un confronto tra field di una stessa row, `located_in` avrà valore `field`; se l'errore è relativo a un item, `located_in` avrà valore `item`)
* una voce `table`, che è un *albero* rappresentato come dizionario: le leaves sono sempre liste contenenti gli indici degli item interessati dall'errore[^2]; il genitore (chiave) di una lista di indici di items è un field; il genitore di un field è una row. 
* Esempio di report per la validazione di **un singolo item**:
	```json
	{'validation_level': 'csv_wellformedness',
	  'error_type': 'error',
	  'message': 'The value in this field is not expressed in compliance with the '
	             "syntax of OpenCitations CITS-CSV. The content of 'citing_id' and "
	             "'cited_id' must be either a single ID or a sequence of IDs, each "
	             'separated by one single space character (Unicode Character '
	             '“SPACE”, U+0020). Each ID must not have spaces within  itself, '
	             'and must be of the following form: <ID abbreviation> + “:” + <ID '
	             'value>.\n',
	  'position': {'located_in': 'item', 'table': {0: {'citing_id': [1]}}}}
	```


### Modifiche alla struttura principale per CITS-CSV

^21392f

- [permalink alla versione del file](https://github.com/eliarizzetto/thesis_resources/blob/3e540fd7a9a3b8d8fe741d146608a8e1d90d2566/CITS/validate_cits.py)
* messaggi all'utente in file di configurazione esterno (→ da chiavi con nomi più espliciti nel file yaml)
* adattato struttura a nuova forma di `position` (ad es. se il field di una row è `citing_publication_date` o `citing_publication_date` → allora → `table = {row: field: [0]}`, dal momento che questi field non vengono "scomposti" in items).
* **È importante poter distinguere quale specifico errore (cioè quale funzione di validazione non è stata passata) a partire soltanto dai dati in `position` e nel resto del report?** → Il problema è che ad un solo report possono corrispondere diversi messaggi e diverse funzione di validazione fallite!
	![ambiguous_report](images/ambiguos_report.jpg)


### Disperati tentativi di gestire i duplicati delle citazioni

Quando delle citazioni si ripetono nel documento, si tratta di un errore.
* Quante e quali casistiche di duplicazione sono possibili, e quindi da individuare?
* Le citazioni devono essere considerate tra *identificativi*  o tra *entità bibliografiche*? 
* È necessario mappare a dei meta-identificativi ciascun ID tra quelli che ci sono nel documento (sulla base della co-occorrenza di alcuni di essi in uno stesso field)? 
* Nel caso di una citazione duplicata, la posizione di quali elementi dovrebbe essere presente nel report dell'errore?

Vedi [Duplicates examples](notes/Duplicates%20examples.md).

[^1]: [Branch](https://github.com/eliarizzetto/thesis_resources/tree/1-semantically-richer-error-dictionaries-meeting-008) dedicato. Vedi: JSON Schema [per singolo errore](https://github.com/eliarizzetto/thesis_resources/blob/3e540fd7a9a3b8d8fe741d146608a8e1d90d2566/check_output/single_validation_output_schema.json) e [per tutto il report](https://github.com/eliarizzetto/thesis_resources/blob/3e540fd7a9a3b8d8fe741d146608a8e1d90d2566/check_output/error_report_schema.json); funzione per creare i dizionari in [`create_report.py`](https://github.com/eliarizzetto/thesis_resources/blob/3e540fd7a9a3b8d8fe741d146608a8e1d90d2566/CITS/create_report.py). 
[^2]: Nel caso in cui un field sia una stringa vuota è opportuno rappresentare questa posizione con None? Se sì, all'interno di una lista? Esempio 1: `'table': {0: {'citing_id': [None]}`; esempio 2: `'table': {0: {'citing_id': None}`.


## Meeting 

1. Per rappresentare la posizione di un errore relativo a un field vuoto (o che coinvolge un field vuoto), il valore di `table[row][field]` deve essere semplicemente `None`, (**non** una lista contenente `None`!). Ad esempio, nel caso in cui l'errore sia il fatto che un field obbligatorio, per un determinato type di pubblicazione non è specificato, il report potrebbe avere una struttura come questa:

```
{
	'validation_level': 'csv_wellformedness',
	  'error_type': 'error',
	  'message': 'The value in this field is not expressed in compliance with the '
				 "syntax of OpenCitations CITS-CSV. The content of 'citing_id' and "
				 "'cited_id' must be either a single ID or a sequence of IDs, each "
				 'separated by one single space character (Unicode Character '
				 '“SPACE”, U+0020). Each ID must not have spaces within  itself, '
				 'and must be of the following form: <ID abbreviation> + “:” + <ID '
				 'value>.\n',
	  'position': {
		  'located_in': 'field', 
		  'table': {
			  0: {'title': None,
				  'publication_type': [0]
			}
		  }
	  }
}
```
La lista vuota come valore del field (es. `'table': {0: {'citing_id': []}` ) tienila a mente come possibilità futura per rappresentare alcune casistiche, anche se per ora non ci viene in mente nessun caso in cui possa essere utile. 

2. Per risolvere il problema sollevato in [Modifiche alla struttura principale per CITS-CSV](#Modifiche%20alla%20struttura%20principale%20per%20CITS-CSV), ovvero che a funzioni di validazione diverse corrispondono error report uguali, rendendo impossibile distinguere la tipologia di errore specifica in modo machine-readable, bisogna **aggiungere una label ulteriore nel report, che specifichi, oltre al validation_level, anche di quale errore specifico si tratta**, all'intero della tassonomia. Ad esempio, per poter distinguere il fatto che un item non è valido <u>perché è uno spazio</u> dal fatto che non è valido <u>perché non rispetta la regex  del formato degli identificativi</u>, si metterà qualcosa come:
   
```
[
{'validation_level': 'csv_wellformedness',
 'error_label': 'csv_wellformedness-space',
 'error_type': 'error', ⬅️⬅️⬅️
  'message': 'The value in this field is not expressed in compliance with the '
             "syntax of OpenCitations CITS-CSV. The content of 'citing_id' and "
             "'cited_id' must be either a single ID or a sequence of IDs, each "
             'separated by one single space character (Unicode Character '
             '“SPACE”, U+0020). Each ID must not have spaces within  itself, '
             'and must be of the following form: <ID abbreviation> + “:” + <ID '
             'value>.\n',
  'position': {'located_in': 'item', 'table': {0: {'citing_id': [1]}}}},
 {'validation_level': 'csv_wellformedness',
  'error_label': 'id_format', ⬅️⬅️⬅️
  'error_type': 'error', 
  'message': 'The value in this field is not expressed in compliance with the '
             'syntax of OpenCitations CITS-CSV.  Each identifier in '
             "'citing_id' and/or 'cited_id' must have the following form: ID "
             'abbreviation + “:” + ID value. No spaces are admitted within the '
             'ID. Example: doi:10.48550/arXiv.2206.03971. The accepted '
             "prefixes  (ID abbreviations) are the following: 'doi', 'issn', "
             "'isbn', 'pmid', 'pmcid', 'url', 'wikidata', 'wikipedia'.\n",
  'position': {'located_in': 'item', 'table': {0: {'citing_id': [1]}}}}
]

```

Ogni errore (così come rilevato da una specifica funzione di validazione) avrà quindi la sua label, in modo che sia distinguibile del tutto dagli altri errori, non soltanto attraverso il messaggio all'utente (che può quindi anche essere ambiguo, nonostante non abbia troppo senso, visto che così si può spiegare l'errore con un buon grado di specificità).

Inoltre, **rivedi i messaggi all'utente** anche in relazione a questo. 