---
title: "Meeting 004"
tags:
- timeline
- meeting
date: 2022-09-17 18:59
---
<span 
		class="ob-timelines"
		data-date="2022-09-17-00">
</span>
📑 [**Reference session: 004**](notes/sessions/session%20004.md)

🔙 [**Previous meeting: 003**](notes/meetings/meeting%20003.md)


## **Q&A**
### .is_valid come primo step di validazione sintattica e semantica per gli ID
*Una volta che il primo livello della validazione è stato passato (formato dati OC), ha senso usare direttamente `.is_valid`? Se lo passa, significa che livello sintattico e (gran parte del) livello semantico sono passati, e i dati possono essere ammessi. Se non lo passa, si possono sottoporre  i dati a tutti i controlli sintattici e semantici uno alla volta.*


### Cosa considerare come ID valido per wikidata e wikipedia?
* **Page ID** è facile da controllare e va bene sia per wikidata che per wikipedia a livello univocità, ma non mi sembra che abbia troppo senso dal punto di vista semantico per le entità di Wikidata
* **Page title** è semanticamente adatto per le entità di wikidata (perché consiste nel Q-ID e rappresenta l'entità stessa), ma sembra un po' "vago" per le pagine di Wikipedia, o perlomeno prono ad errori.

Possibili soluzioni:[^1]
1. Page title sia per gli input id prefissati da `wikidata:` (Q-ID, cioè così come è ora) che per quelli prefissati da `wikipedia:`. 
2. Page ID sia per `wikidata:` che per `wikipedia:`.
~~3. *Solo* Page Title per `wikipedia:`, Page Title *o* Page ID (Q-ID) per `wikidata:`.~~
~~4. *Solo* Page ID (numero intero positivo) per `wikipedia:`, Page Title *o* Page ID (Q-ID) per `wikidata:`.~~

[^1]: Scarto la possibilità di ammettere entrambi i tipi di id per Wikipedia perché non sono automaticamente distinguibili. Inoltre, non è ammissibile avere due possibili identificativi per lo stesso prefisso all'interno di OC (la stessa risorsa, se è rappresentata con due identificativi diversi per lo stesso prefisso, es. `wikidata:`, risulta come due risorse diverse all'interno degli inidici)

### È necessario ammettere che l'utente invii come ID per wikidata o wikipedia l'URL completo?
Es. "wikipedia://en.wikipedia.org/w/index.php?curid=8091"  

### .get_extra_info



