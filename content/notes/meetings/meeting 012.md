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
* author e editor vogliono obbligatoriamente la virgola
* publisher non ammette la virgola (???)
* le quadre ci sono solo se contengono ID all'interno (mai `[]` vuote!)


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
Una stringa come questa qui sopra (che è corretta), è possibile/sensato rappresentarla con la lista di items qui sotto? 

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


### 2. url e wikipedia sono ID possibili per tutti i responsible agents (authors, publisher, editor)?