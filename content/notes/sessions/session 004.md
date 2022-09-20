---
title: "Session 004"
tags:
- timeline
- session
- wikipedia
- wikidata
date: 2022-09-17 18:58
---
<span 
		class='ob-timelines'
		data-date="2022-09-17-00">
</span>

👥 [**Reference meeting: 004**](notes/meetings/meeting%20004.md)

🔙 [**Previous work session: 003**](notes/sessions/session%20003.md)

## Identifiers: wikidata e wikipedia

<u>**Per info complete vedi [Managing Wiki Identifiers](notes/Managing%20Wiki%20Identifiers.md)**</u>

### Accesso ai dati: MediaWiki Action API e Linked Data Interface
I servizi di Wikimedia sono consultabili attraverso un'unica API, con un URL specifico per ogni servizio: la [**MediaWiki Action API**](https://www.mediawiki.org/wiki/API:Main_page). 

###### Wikidata
Seguendo i consigli della pagina di aiuto per l'accesso a Wikidata, per questo identificativo ho però deciso di ricorrere a un altro tool (disponibile solo per Wikidata), la cosiddetta [**Linked Data Interface**](https://www.wikidata.org/wiki/Wikidata:Data_access/en#Linked_Data_Interface_(URI)); funziona come un'API, nel senso che posso specificare i parametri che mi interessano, come ad esempio l'output format in json, ma è più veloce, oltre che più indicata per l'accesso a risorse precise e già note (come quelle inviate dall'utente).

###### Wikipedia
Per Wikipedia, invece, è necessario usare la [MediaWiki Action API]https://www.mediawiki.org/wiki/API:Main_page), perché non c'è nessun altro tool in cui si possano specificare i parametri nella richiesta. Per altro, bisogna per forza specificare un URL per la lingua di riferimento, quindi l'endpoint dell'API sarebbe quello di Wikipedia in inglese.

### Quali identificativi considerare?

#### Page ID
<u>Tutte le pagine dei servizi che sfruttano il software MediaWiki[^1] hanno un identificativo ***della pagina*** (!!)</u>: il **page id**. Tramite questo identificativo si può accedere a una pagina specifica, sia che si utilizzi l'API sia senza, usando specifici parametri a seconda della soluzione scelta per accedere ai dati.

> [!examples]- Esempi
> 
> * **Con API** (parametro `pageid` o `pageids`, a seconda del valore del parametro `action`, rispettivamente `query` e `parse`):
> 
> https://en.wikipedia.org/w/api.php?action=parse&format=json&pageid=8091
>
> https://en.wikipedia.org/w/api.php?action=query&format=json&pageids=8091
> 
> * **Senza API** (parametro `curid`)
>
> https://en.wikipedia.org/w/index.php?curid=8091


1. Il Page ID è univoco, e lo è relativamente a **tutte** le pagine create con MediaWiki prese nella loro totalità (il Page ID assegnato a una pagina Wikipedia non solo non potrà essere assegnato ad un'altra pagina Wikipedia, ma nemmeno a una pagina Wikidata).

2. <u>Il Page ID è una proprietà **della pagina**</u> appunto, non dell'entità che essa descrive. Ovvero, se l'entità Douglas Adams ha un identificativo, quello è "Q42", *non* "138", che è il Page ID della sua pagina all'interno di Wiki*data*.

Per la ragione menzionata al punto 2), <u>non mi sembra appropriato usare il Page ID per rappresentare dei concetti o delle entità, come si cercherebbe di fare nel rappresentare delle risorse provenienti da Wikidata</u> --> per gli id prefissati da `wikidata:` posso usare il **Q-ID** (vedi [Managing Wiki Identifiers](notes/Managing%20Wiki%20Identifiers.md)).
**<u>Ma per le pagine di Wikipedia?</u>** (vedi sotto).

#### Page title
Il Page title è un identificativo univoco che rappresenta ciò che la pagina descrive, o meglio il suo contenuto. È relativo a ciascuno dei due servizi, Wikipedia e Wikidata. **Nel caso di Wikidata**, consiste nel Q-ID - ed ha quindi una sintassi precisa -  e rappresenta l'entità che la pagina descrive (o almeno così l'ho inteso). **Nel caso di Wikipedia**, è comunque univoco (due pagine, entrambe di Wikipedia, non avranno mai lo stesso titolo), ma è un semplice literal ("teoricamente" senza spazi, da quanto capisco). È visibile nell'ultima parte dell'URL, formattato con degli underscore al posto degli spazi.

#### Altri identificativi
Esiste (un) altro/altri identificativo/i, con una sintassi uguale a quella del Page ID, ma associato/i a versioni specifiche della pagina. Non è quindi distinguibile automaticamente dal Page ID ma ha bisogno di un suo parametro specifico nella richiesta.

#### Wikipedia

[^1]: **MediaWiki** is a particular wiki engine software developed for and used by Wikipedia and the other Wikimedia projects. MediaWiki is freely available for others to use (and improve), and it is in use by all sorts of projects and organizations around the world. This site, mediawiki.org, is intended for information about MediaWiki and related software. *from [https://www.mediawiki.org/wiki/Differences_between_Wikipedia,_Wikimedia,_MediaWiki,_and_wiki](https://www.mediawiki.org/wiki/Differences_between_Wikipedia,_Wikimedia,_MediaWiki,_and_wiki)*.

## WikidataManager
Ho aggiunto agli IdentifierManager la classe `WikidataManager`, in [**wikidata.py**](https://github.com/opencitations/identifier_manager/blob/main/oc_idmanager/wikidata.py). È stata costruita sul modello di `DOIManager`; così com'è funziona e **passa i test, già caricati su Github**. Non dovrebbero esserci correzioni da fare, ma **se si vogliono aggiungere le funzionalità di get_extra_info** (vedi sotto) **è ovviamente da estendere**.


## get_extra_info: un parametro (o un metodo) per ritornare informazioni ulteriori ottenute dall'API
Con Arianna si pensava di aggiungere un metodo, che chiameremo `.get_extra_info`, che con un chiamata all'API non solo verifica che l'id esista e sia registrato (facendo cioè quello che già fa `.exist`) ma in più estrae dal json in risposta altre informazioni utili (es. publisher, tipo di pubblicazione, ecc.). Questo metodo va aggiunto tra i parametri di `.is_valid` ed è in default **False**, lasciando che `.is_valid` chiami e ritorni `.exist`, esattamente come ora; se però   `.get_extra_info` è specificato come True, viene chiamato e, di conseguenza, `.is_valid` ritorna sia il valore booleano del controllo di esistenza, sia un dizionario con le informazioni extra estratte dalla risposta dell'API. Vedi codice sotto. Ovviamente il metodo andrà aggiunto anche su Su Base Manager.


```python {title="Proposta per modifiche di .is_valid; es. da wikidata.py"}


def is_valid(self, wikidata_id, get_extra_info=False): #default False tra i params 
  
    wikidata_id = self.normalise(wikidata_id, include_prefix=True)
    if wikidata_id is None or not self.syntax_ok(wikidata_id):  
        return False  
    elif get_extra_info=False:  # se get_extra_info==False chiama e ritorna exist oppure il valore all'interno di data
        if not wikidata_id in self._data or self._data[wikidata_id] is None:  
            return self.exists(wikidata_id)  
        return self._data[wikidata_id].get("valid")
    else: #se get_extra_info==True chiama e ritorna get_extra_info
	    return self.get_extra_data(wikidata_id)


...

def get_extra_info(self, wikidata_id_full):
	... #richiesta all'API (vedi .exist())
	... #estrazione delle info extra (se possibile, cioè dipendentemente dalla risposta che dà l'API)
	
	return bool, dict # se la chiamata va a buon fine e la risorsa esiste, ritorna True e un dizionario con le info aggiuntive

```


**Update**: vedi [notebook di Arianna](https://github.com/ariannamorettj/OC_notebook_n/blob/main/OC_AM_notebook.ipynb), sezione 13/09 - 20/09:

> "Confronto con Elia: **Proposta di aggiungere un parametro tipo get_extra_info=False a is_valid**, che viene **poi passato a exist**. Il vantaggio di mettere questo parametro inizializzato a False già in is_valid (e non solo in exist) permette, su richiesta, di ottenere informazioni aggiuntive (es. publisher, tipo di pubblicazione, ecc.). senza dover fare una seconda chiamata alle API, e di ottenere, insieme al booleano relativo alla validazione, anche un dizionario con i dati aggiuntivi recuperati. In alternativa, il parametro get_extra_info=False si potrebbe aggiungere solo ad exists, ma il vantaggio sarebbe limitato alle occasioni in cui si può chiamare exists da solo: nella maggior parte dei casi resterebbe la doppia chiamata alle API, dal momento che non si potrebbero recuperare le informazioni già nel corso della prima validazione."

`get_extra_info` dovrebbe per forza essere inizializzato a False, perché normalemente dove poter ritornare un booleano. Per il processo di validazione ci sarebbe un altra funzione, esterna agli ID managers, per confrontare i risultati ottenuti dall'API con i dati forniti dagli utenti.


<u>**Per info complete vedi [Managing Wiki Identifiers](notes/Managing%20Wiki%20Identifiers.md)**</u>