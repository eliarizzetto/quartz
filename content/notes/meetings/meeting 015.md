---
title: "Meeting 015"
tags:
- timeline
- meeting
date: 2022-12-13 02:58
---
<span 
		class="ob-timelines"
		data-date="2022-12-13-00">
</span>
📑 [**Reference session: 015**](

🔙 [**Previous meeting: 014**](notes/meetings/meeting%20014.md)

## IdentifierManager
Ho fatto Wikipedia, Wikidata e Viaf, ma solo i metodi che uso per validare (manca `get_extra_info()`). Il metodo `normalise` l'ho lasciato uguale o molto simile a quello per i doi.

Ora mancano ror e pmcid.

## Page
* Ho integrato nella funzione `wellformedness_page` i pattern alfanumerici e quelli misti (numeri romani e arabi)
* Ho creato una funzione `check_interval_page` che ritorna False se l'intervallo è invalido (integer della start page è maggiore del'integers della end page) OR se non è stato possibile convertire le pagine dell'intervallo (una sola o entrambe) in integers (ad esempio perché si tratta di una notazione alfanumerica o perché il numero romano non viene convertito dalla libreria che uso, [roman](https://pypi.org/project/roman/)). Se il valore non passa questa funzione viene sollevato un **warning**. 


