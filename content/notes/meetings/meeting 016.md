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

|type               |doi|isbn|issn|pmid|pmcid|wikidata|wikipedia|url|
|-------------------|---|----|----|----|-----|--------|---------|---|
|book               |t  |t   |    |t   |     |t       |t        |t  |
|dataset/data file  |t  |    |    |t   |     |t       |t        |t  |
|dissertation       |t  |t   |    |t   |t    |t       |t        |t  |
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

**Dal punto di vista semantico (compliance con il data model), sono necessari altri controlli oltre ai seguenti?**
* type e id della risorsa
* id e presenza nei campi dei RA (alcuni ID sono solo per organizzazioni, altri solo per people), che poi in realtà è già controllata nel primo livello di validazione

## Prossime mosse e scadenze?
* Consigli/indicazioni sulla struttura:
	* I capitolo: stato dell'arte -> cosa dire? stato dell'arte sui validatori o solo su OpenCitation?
	* II capitolo:
	* III capitolo:
* Fonti da cui partire
* Dovrò finire di scrivere il software mentre sono in fase di scrittura per i primi capitoli
* scadenze?


1. toc esteso per prossimo incontro (titolo sezione con paragrafo di descrizioni)
2. tesi è implementativa:
	1. introduzione (vedi articolo su framework optimeta https://arxiv.org/abs/2301.01502) e importanza del validatore. qui devi difendere l'utilitità e lo scopo del validatore
	2. literature review (sui validatori, non su OpenCitations !!!! pyschacl, shex, pydantic, cerberus e le altre cose che abbiamo visto all'inizio vanno tutte bene per questa parte !!! Nel menzionare lo stato dell'arte, giustifica l'esigenza di un nuovo validatore: esigenza legata alla specificità dei dati)
	3. metodologia (come funziona senza entrare nel merito del codice, deve spiegare il processo ad alto livello); è abbastanza importante inserire diagramma UML delle classi e come interagiscono tra loro
	5. descrizione del codice a basso livello, nel dettaglio
	6. capitolo di ""evaluation"" con la valutazione del costo computazionale: devi creare un dataset pulito e uno con problemi per testare la performance e vedere quanto ci mette il software per andare, sia con i dati "corretti" che con i dati contenenti errori. Idealmente tutti i possibili errori devono essere rappresentati. La misura della performance, in questa evaluation, è data dai tempi che hai computato
	7. Conclusioni citando future works e futuri sviluppi del validatore



* file di configurazione esterno dove la chiave è il type e il valore è una lista di ID (un json va bene)
* per capire quali tipi sono associabili agli ID puoi partire dai dati esistenti (tabelle di Meta -- o crossref? -- che scarichi da Zenodo, c'è il link sul sito di OC), per poi analizzarli e vedere che cosa viene fuori di inaspettato (rispetto a quello che c'è scritto nelle singole documentazioni degli identificativi) e poco frequente. Così puoi orientarti meglio nel definire l'allineamento. Devi scrivere uno scriptino per creare un dizionario a partire dai csv del dump, che abbia per ogni identificativo tutti i tipi che trova in Meta. In linea generale, è meglio essere più inclusivi che restrittivi nel definire i tipi ammissibili per ogni identificativo
* pagina wikipedia rappresenta se stessa (type solo reference entry e other)
* organizza tutto in classi
* scrivi i test per gli ID manager