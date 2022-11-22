---
title: "Meeting 012"
tags:
- timeline
- meeting
date: 2022-11-16 14:53
---
<span 
		class="ob-timelines"
		data-date="2022-11-16-00">
</span>
📑 [**Reference session: 012**](notes/sessions/session%20012.md)

🔙 [**Previous meeting: 011**](notes/meetings/meeting%20011.md)


## META-CSV: domande
versione: https://github.com/eliarizzetto/thesis_resources/blob/f5d0a5e189b17df71173e1542857e9ae630d4998/CITS/validate_meta.py




### 1. Come suddividere in items i valori dei field dei r.a.?
* ~~author e editor vogliono obbligatoriamente la virgola~~ {la virgola può anche non esserci, come nel caso in cui author non sia una persona ma un azienda, ad esempio}.
* ~~publisher non ammette la virgola (???)~~ {il publisher ammette la virgola, vedi sotto}
* le quadre ci sono solo se contengono ID all'interno (mai `[]` vuote!)
* presto Arcangelo gestirà anche la possibilità di avere solo gli identificativi all'interno delle quadre, senza che siano associati alla stringa di un nome


Ho bisogno di un modo che suddivida questi field in item garantendo che:
* <u>funzioni sempre nello stesso modo</u>, indipendentemente dalla stringa in input (per poter localizzare un errore nel documento in input in maniera coerente)
* la <u>localizzazione dell'errore sia quanto più possibile precisa</u>, ovvero tutto ciò che analizzo come input di una funzione di validazione deve avere la sua posizione in `table`. Idealmente, devono poter essere identificabili come item tutti i seguenti "tipi" di stringa:
	* spazi → per trovare gli spazi extra
	* singoli identificativi → per controllarne il formato OC, la sintassi, l'esistenza e la coerenza con il tipo specificato
	* in generale tutto quello che non può essere considerato valido come item all'interno del field (caratteri illegali come `;`, `[`, ecc. 
* allo stesso tempo siano inclusi nella lista degli item anche quelli validi, naturalmente

Sono esempi di errori:
* spazi, quadre, punti-e-virgola *extra*
* id non ben formati (formato OC o sintassi specifica)
* sequenze di items errate, come nel caso di virgole mancanti tra un cognome e una `[`, o punti-e-virgola mancanti tra due gruppi `cognome, (nome)`...

#### Esempio
```
Tostivint, Hervé; Trabucchi, Michele [orcid:0000-0001-6885-5628 doi:10/1234]; Conlon,; Lihrmann, Isabelle
```
Una stringa come questa qui sopra, è possibile/sensato rappresentarla con la lista di items qui sotto? 

```python
[
'Tostivint,',
'Hervé',
';',
'Trabucchi,',
'Michele',
'[',
'orcid:0000-0001-6885-5628',
'doi:10/1234',
']',
';',
'Conlon',
';',
'Lihrmann,',
'Isabelle'
]

```

Ho dato un'occhiata alle regex di Arcangelo: https://github.com/opencitations/oc_meta/blob/master/oc_meta/lib/master_of_regex.py

##### Il field publisher non ammette virgole nel nome?
Sì, anche il field publisher può contenere virgole, perché può tranquillamente essere incluso nel nome della casa editrice. La virgola nel campo publisher viene trattata semplicemente come qualsiasi altro carattere all'interno della stringa, mentre nel caso di author e editor, se c'è, viene trattata come separatore (tra nome e cognome). 

### 2. url e wikipedia sono ID possibili per tutti i responsible agents (authors, publisher, editor)?

Risposta: **<u>No, né url né wikipedia sono ammissibili come ID di responsible agents, li teniamo solo per le br!</u>**


# Incontro

### Risposta 1

Gli item sono quelli separati dal punto-e-virgola + spazio: `; `. [^1]Non si può localizzare più precisamente di così, quindi la localizzazione di un errore in `table` sarà sempre relativa alla posizione dell'item in questa lista, anche se l'errore sta in una parte specifica all'interno di quell'item; quello che cambia è il messaggio human-readable, che deve essere più specifico possibile, e chiaramente un'indicazione è data anche dal `validation_level` e dall'`error_label`. 

[^1]: Il criterio generale con cui un field viene diviso in items deve essere fisso. Il criterio in questione è che un field che ammette più elementi ha un separatore specifico (gli spazi nel caso del field `id`, ad esempio; il punto e virgola nel caso di `author`); su questa base mi sembra che non abbia molto senso dividere in item anche quei field che per sintassi non contengono più elementi (ovvero non hanno separatori): ad esempio, il field `pub_date` non andrebbe splittato, (nonostante il modo in cui esprimo la posizione di un errore in table), in quanto non ha un separatore. 

Ad esempio, nel caso della stringa:
```
Tostivint, Hervé; Trabucchi, Michele [orcid:0000-0001-6885-5628 doi:10/1234]; Conlon,; Lihrmann, Isabelle
```
La lista di items sarebbe:
```python
'Tostivint, Hervé',
'Trabucchi, Michele [orcid:0000-0001-6885-5628 doi:10/1234]',
'Conlon,',
'Lihrmann, Isabelle'
```
Nel caso in cui volessi rappresentare il fatto che un doi non può essere associato a un author, il report sarebbe localizzato in `position` in questo modo: `table: {0:{'author':[1]}}`, ma avrebbe `'validation_level': 'semantic'`, magari una `error_label` specifica, e conseguentemente un `message` preciso/dettagliato.


Forse, si può anche pensare di usare le formatted strings per localizzare la posizione dell'ID, ad esempio, nel messaggio all'utente, dando qualche indicazione **non** machine readable.


Inoltre, tieni presente che non solo non è necessario, ma non sarebbe nemmeno corretto, mappare le stringhe dei nomi a gruppi di identificativi (le stringhe dei nomi, infatti, possono variare, ad esempio con abbreviazioni, secondi nomi, ecc.). --> per META-CSV non bisogna mappare niente. 