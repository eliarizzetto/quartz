---
title: "Session 016"
tags:
- timeline
- session
date: 2023-01-05 19:16
---
<span 
		class='ob-timelines'
		data-date="2023-01-05-00">
</span>

👥 [**Reference meeting: 016**](notes/meetings/meeting%20016.md)

🔙 [**Previous work session: 015**](notes/sessions/session%20015.md)


## IDManager

### PMCID
* è un'identificativo associato a ciascuno degli articoli presenti in PubMed Central
* può essere cercato tramite almeno due API:
	* **Entrez Programming Utilities** (aka E-utilities o e-util), la API tramite cui è possibile accedere a ***tutti*** i server, database e serivizi del National Center for Biotechnology Information (NCBI) (a sua volta parte del NIH), <u>non</u> solo a PubMed Central . 
	* **ID Converter di PMC**, il tool per la conversione ID, specifico per records che alloggiano in PubMed Central (pmc), che consente di risalire dall'identificativo di un lavoro agli altri suoi identificativi, se questo lavoro è presente in pmc.

#### Il limite di 3 richieste al secondo
Il problema con Entrez è che ammette fino a 3 richieste al secondo: se si sfora questo limite, dopo un po' ti bloccano (è possibile che ti contattino prima di bloccarti, se nella richiesta http hai scritto l'indirizzo e-mail). Si può richiedere via mail una API key per prevenire il problema dell'essere bloccati. Per quanto riguarda la API dell'ID Converter, non trovo scritto da nessuna parte di limiti al numero di richieste, che è verosimile visto che viene usata per un servizio molto più specifico e rischia di meno di essere sovraccaricata di richieste. 

#### L'uso dell'API in base alla funzione
Se devo solo verificare che un PMCID sia registrato, posso usare anche ID Converter: non è pensato per questo, ma funziona; mi dà gli altri identificativi associati, come il doi o il pmid; soprattutto non ha limiti al numero di richieste.
Invece, se voglio ulteriori informazioni, come titolo, autore, data di pubblicazione, ecc., allora senz'altro devo usare Entrez, con azione e parametri appropriati, in base a quante/quali extra info voglio ottenere. 
Per tutte le API legate PubMed Central, vedi https://www.ncbi.nlm.nih.gov/pmc/tools/developers/.


**Documentazione Entrez**: https://www.ncbi.nlm.nih.gov/books/NBK25500/

**Documentazione ID Converter**: https://www.ncbi.nlm.nih.gov/pmc/tools/id-converter-api/

Video spiegazione Entrez + altre API, tra cui ID Converter: https://youtu.be/0pRvEDS6Afw

**Ho deciso di usare l'API di ID Converter** per non avere il problema del limite alle richieste. In `oc_idmanager.pmcid.py`, sia in `syntax_ok` che in `exists`, ammetto la possibilità di avere il PMCID in due formati, visto che la documentazione non è chiara:
* con "PMC" a precedere il numero reale di 7(?) cifre
* solo il numero, senza "PMC"
Praticamente, "PMC" è un prefisso interno che serve a specificare che quell'ID non è un PMID (o altro?), ma un PMCID. Comunque, quando si trova citato un PMCID nella maggior parte dei casi lo si trova citato *con* "PMC" incluso.
Nell'API di ID Converter, purché sia specificato il parametro `idtype=pmcid`, questo identificativo si può scrivere con o senza "PMC"; se non si specifica il parametro `idtype` e l'id viene scritto senza "PMC", allora verrà interpretato come un PMID!! --> ho specificato  `idtype=pmcid` tra i parametri della richiesta, in modo che un pmcid possa essere cercato sia con il prefisso "PMC" sia senza.

Se si vogliono ottenere più informazioni, conviene usare E-Utilities, ma bisogna chiaramente cambiare come viene formulata la richiesta è il modo in cui viene interpretata dal metodo `esists` la risposta.[1] 

[1]: Una cosa da tenere presente è che E-Utilities, se ho capito bene , richiede che venga specificato il servizio a cui si vuole mandare la richiesta con il parametro `db`, quindi non credo sia possibile scrivere un PMCID con il prefisso "PMC" incluso (perché lo abbiamo già specificato in `db=pmc`). In ogni caso sarebbero tutte cose da studiare con più calma se si decidesse di usare E-Utilities perché consente di avere più informazioni aggiuntive.

#### Nessuna documentazione sulla sintassi dell'identificativo
Non c'è una pagina che spieghi qualcosa sul formato/sintassi dei PMCID, se non appunto che PMC può esserci o no (almeno per quanto riguarda le richieste alle API). Dagli esempi trovati vediamo che:
* un PMCID è tendenzialmente citato con il prefisso PMC, anche nella *risposta* dell'API
* non ho mai trovato più di 7 cifre (dopo l'eventuale prefisso) e ho trovato solo numeri reali (alla sequenza di cifre anche le fonti ufficiali si riferiscono con "number")
* non ho mai trovato lettere
* un PMCID può essere esteso con un punto e delle cifre che indicano il numero della versione del lavoro a cui è associato il PMCID. Es. PMC1234324.1
* la pagina wikidata dà questa regular expression (nota che è senza prefisso "PMC"): `[1-9]\d{0,6}` . Vedi [proprietà](https://www.wikidata.org/wiki/Property:P932) e [Q entity](https://www.wikidata.org/wiki/Q11801904).

In mancanza di una documentazione specifica, ufficiale e dettagliata, mi sono tenuto largo nel definire cosa è sintatticamente corretto: il prefisso "PMC" è ammesso ma non obbligatorio; deve esserci almeno una cifra, ma senza un limite (anche più di sette); può esserci un punto seguito da massimo due cifre (perché si suppone che nessun lavoro abbia più di 99 versioni registrate). Questa tolleranza è mantenuta solo per principio, appunto perché non ho trovato una fonte ufficiale da poter citare, però nella realtà dei fatti forse sarebbe meglio essere più restrittivi ed usare ad esempio la regex di wikidata (magari con il solo adattamento del prefisso).



### ROR
#### Che cos'è ROR

Da https://ror.org/about/#what-is-ror:

>The Research Organization Registry (ROR) is a global, community-led registry of open persistent identifiers for research organizations. ROR makes it easy for anyone or any system to disambiguate institution names and connect research organizations to researchers and research outputs.

>Organizations are not static entities. They change their names, merge, split, shut down, and re-emerge, and this makes it difficult to connect research organizations to research outputs and researchers. A persistent identifier for research organizations makes this easier.

>ROR is the first and only organization identifier that is openly available (CC0 data available via an open REST API and public data dump), specifically focused on identifying affiliations in scholarly metadata, developed as a community initiative to meet community use cases, and designed to be integrated into open scholarly infrastructure. It is the default identifier supported in Crossref DOI metadata, DataCite DOI metadata, and ORCID.

>ROR is operated as a collaborative initiative by [California Digital Library](https://cdlib.org/), [Crossref](https://crossref.org/), and [DataCite](https://datacite.org/).


"ROR is more than just an identifier: each record associated with a ROR ID contains useful information about the organization’s name in multiple languages, acronyms, aliases, location, website, Wikipedia page, and other persistent identifiers". <u>Queste informazioni, comunque, non sono codificate nell'identificativo, ma soltanto contenute nel record a cui l'identificativo è associato</u>. 

#### Sintassi

Da https://ror.org/about/faqs/:

>ROR identifiers have a [predictable pattern](https://ror.readme.io/docs/ror-identifier-pattern) and can be validated with the regular expression ﻿﻿`^https://ror.org/^0[a-z|0-9]{6}[0-9]{2}$`. The canonical form of a ROR identifier value is the entire URL.

La regex che uso è: `^ror:((https:\/\/)?ror\.org\/)?0[a-hj-km-np-tv-z|0-9]{6}[0-9]{2}$`. Include sia i casi in cui si usi l'URL completo (come sarebbe giusto), sia quelli in cui si usi l'URL senza "https" e "www", sia quelli in cui si usi solo l'identificativo, senza l'URL. **Forse bisognerebbe essere più restrittivi, ammettendo solo l'URL completo come forma valida?**

#### API

Documentazione: https://ror.readme.io/docs/rest-api
* rate limit is a maximum of 2000 requests in a 5-minute period, and API traffic can be quite heavy
* per cercare un singolo identificativo (come nel nostro caso), è sufficiente aggiungerlo a questo base URL: `https://api.ror.org/organizations/`. Vengono accettati tutti i tre formati dell'identificativo che sono ammessi anche dalla mia regex, cioè al base URL può essere attaccato un ID in uno qualsiasi di questi tre formati:
	* **Full ROR ID URL:** `https://ror.org/015w2mp89`
	* **Domain and ID:** only `ror.org/015w2mp89`
	* **ID only :** `015w2mp89`
* la risposta viene data direttamente in json, senza bisogno di specificare alcun parametro. Per la data structure della risposta vedi https://ror.readme.io/docs/ror-data-structure, anche se è abbastanza human-readable.



## Funzioni per livello 2 e 3 (solo `exists`)

Le funzioni sono scritte, ma devi ancora integrarle nella funzione principale (+ fare messaggi e aggiungere in errors_map)