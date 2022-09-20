---
title: "Meeting 003"
tags:
- timeline
- meeting
date: 2022-09-13 02:31
---
<span 
		class="ob-timelines"
		data-date="2022-09-13-00">
</span>
📑 [**Reference session: 003**](notes/sessions/session%20003.md)

🔙 [**Previous meeting: 002**](notes/meetings/meeting%20002.md)

### Arcangelo
Proposta di automatizzare tutto il workflow di ingestione dei dati in OC tramite le issue di GitHub.

### Giuseppe
Per fare modifiche a oc/index fare un branch/fork (uno dei due??) dal branch `develop` e poi fare la pull a develop. 
## **Q&A**
### *Question 1: *
Answer
### *Question 2: *
Answer



## **To do**
* [ ] Task 1
* [ ] Task 2
* [ ] Task 3



# Utenza validatore
- Chi usa il validatore? Con dati di quale dimensione? Ha senso in base a questi parametri pensare a un'interfaccia interattiva per la correzione diretta del documento inviato?
- Prefissi wikipedia, wikidata, ror
- per id che non hanno una api o che la hanno a pagamento (issn e isbn) si può anche pensare di ricorrere a api di servizi esterni (es librerie digitali)




* wikidata per rappresentare entità, wikipedia per rappresentare documenti (che possono essere a proosto di entita) --> vedi repo su oc/wcw in cui si raccolgono informazioni utili per wikipedia e wikidata, i loro prefissi, etc.

* per controlli di natura semantica si ricorre all'API: negli ID manager, oltre al controllo di esistenza, si può controllare che il tipo assegnato dall'utente sia corrispondente a quello menzionato nell'api
* separata validazione con shacl in quarto step a parte che si chiami qualcosa tipo "OC Data Model Validation"

* cambia la dicitura "note"  in "info" per le classi concettuali degli errori
* tutti i passaggi della validazione sono sequenziali (se non passa il primo livello di validazione rispetto al formato dati di OC, non accederà al secondo, e così via). Questo però significa che devono essere per forza fatti uno alla volta, e che al termine di ognuno c'è bisogno di una correzione da parte dell'utente? 