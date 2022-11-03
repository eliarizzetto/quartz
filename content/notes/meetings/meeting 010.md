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


### Modifiche alla [struttura principale per CITS-CSV](https://github.com/eliarizzetto/thesis_resources/blob/3e540fd7a9a3b8d8fe741d146608a8e1d90d2566/CITS/validate_cits.py)
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

Vedi [duplicates examples](notes/duplicates%20examples.md).


[^1]: [Branch](https://github.com/eliarizzetto/thesis_resources/tree/1-semantically-richer-error-dictionaries-meeting-008) dedicato. Vedi: JSON Schema [per singolo errore](https://github.com/eliarizzetto/thesis_resources/blob/3e540fd7a9a3b8d8fe741d146608a8e1d90d2566/check_output/single_validation_output_schema.json) e [per tutto il report](https://github.com/eliarizzetto/thesis_resources/blob/3e540fd7a9a3b8d8fe741d146608a8e1d90d2566/check_output/error_report_schema.json); funzione per creare i dizionari in [`create_report.py`](https://github.com/eliarizzetto/thesis_resources/blob/3e540fd7a9a3b8d8fe741d146608a8e1d90d2566/CITS/create_report.py). 
[^2]: Nel caso in cui un field sia una stringa vuota è opportuno rappresentare questa posizione con None? Se sì, all'interno di una lista? Esempio 1: `'table': {0: {'citing_id': [None]}`; esempio 2: `'table': {0: {'citing_id': None}`.