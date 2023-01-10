---
title: "Meeting 016"
tags:
- timeline
- meeting
date: 2023-01-05 19:16
---
<span 
		class="ob-timelines"
		data-date="2023-01-05-00">
</span>
📑 [**Reference session: 016**](notes/sessions/session%20016.md)

🔙 [**Previous meeting: 015**](notes/meetings/meeting%20015.md)

## Identifier Manager
* finiti tutti gli identificativi, aggiunti quindi pmcid e ror (di quelli che ho fatto io ho integrato solo `syntax_ok` e `exists`, rimane fuori `get_extra_info`)
* **va bene l'API per pmcid (poche informazioni extra ma senza il limite di richieste)?**

## Allineamento `id` e `type`
Per verificare la compliance con il data model, devo verificare che id e tipo assegnati ad una stessa risorsa siano compatibili/coerenti.

Nella tabella sotto è riassunta la proposta per le BR:
* doi, pmid, wikidata, wikipedia e url → tutti i tipi
* issn → book series, book set, journal, proceedings series, report series, standard series, series
* isbn → book, dissertation, edited book, monograph, report, reference book, standard
* **pmcid** → dissertation, journal article, peer review, proceedings article, report

In particolare pmcid è poco documentato, sappiamo solo che:
* PubMed Central is an index of full-text papers, while PubMed is an index of abstracts. The PMCID links to full-text papers in PubMed Central, while the PMID links to abstracts in PubMed. https://publicaccess.nih.gov/include-pmcid-citations.htm#Difference
* Your paper may be included in PubMed Central (PMC) if (https://www.ncbi.nlm.nih.gov/pmc/about/submission-methods/#Determining%20Eligibility):
	-   You published in a journal that is fully archived in PMC;
	-   You made open access arrangements with a PMC selective deposit journal or publisher program; or
	-   Your article was
	    -   supported by the National Institutes of Health (NIH), another PMC designated funder, or a member of the Europe PMC funder group;
	    -   peer reviewed; and
	    -   accepted for publication in a journal.
* dal 2020 anche preprint sono ammissibili per pmcid, se espongono risultati di ricerche finanziate dal NIH

|id                 |doi|isbn|issn|pmid|pmcid|wikidata|wikipedia|url|
|-------------------|---|----|----|----|-----|--------|---------|---|
|book               |t  |t   |    |t   |     |t       |t        |t  |
|dataset/data file  |t  |    |    |t   |     |t       |t        |t  |
|disseration        |t  |t   |    |t   |t    |t       |t        |t  |
|edited book        |t  |t   |    |t   |     |t       |t        |t  |
|journal article    |t  |    |    |t   |t    |t       |t        |t  |
|monograph          |t  |t   |    |t   |     |t       |t        |t  |
|other              |t  |    |    |t   |     |t       |t        |t  |
|peer review        |t  |    |    |t   |t    |t       |t        |t  |
|posted content     |t  |    |    |t   |     |t       |t        |t  |
|proceedings article|t  |    |    |t   |t    |t       |t        |t  |
|report             |t  |t   |    |t   |t    |t       |t        |t  |
|reference book     |t  |t   |    |t   |     |t       |t        |t  |
|reference entry    |t  |    |    |t   |     |t       |t        |t  |
|book chapter       |t  |    |    |t   |     |t       |t        |t  |
|book part          |t  |    |    |t   |     |t       |t        |t  |
|book section       |t  |    |    |t   |     |t       |t        |t  |
|book track         |t  |    |    |t   |     |t       |t        |t  |
|component          |t  |    |    |t   |     |t       |t        |t  |
|book series        |t  |    |t   |t   |     |t       |t        |t  |
|book set           |t  |    |t   |t   |     |t       |t        |t  |
|journal            |t  |    |t   |t   |     |t       |t        |t  |
|proceedings        |t  |    |    |t   |     |t       |t        |t  |
|proceedings series |t  |    |t   |t   |     |t       |t        |t  |
|report series      |t  |    |t   |t   |     |t       |t        |t  |
|series             |t  |    |t   |t   |     |t       |t        |t  |
|standard           |t  |t   |    |t   |     |t       |t        |t  |
|standard series    |t  |    |t   |t   |     |t       |t        |t  |
|journal issue      |t  |    |    |t   |     |t       |t        |t  |
|journal volume     |t  |    |    |t   |     |t       |t        |t  |


## Prossime mosse e scadenze?






