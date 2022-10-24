---
title: "Meeting 007"
tags:
- timeline
- meeting
date: 2022-10-13 04:41
---
<span 
		class="ob-timelines"
		data-date="2022-10-13-00">
</span>
📑 [**Reference session: 00X**](#insert path)

🔙 [**Previous meeting: 00X**]("#insert path")

> [!abstract]+ Summary
> 
> In this meeting we talked about:

## **Introduction**




## **Q&A**
### *Question 1: *
Answer
### *Question 2: *
Answer



## **To do**
* [ ] Task 1
* [ ] Task 2
* [ ] Task 3



### appunti

* warning potrebbe essere quando su meta vengono messe due row con lo stesso identificativo (duplicazione ): non è sbagliato ma 
* non ci possono essere duplicati della stessa informazione 
* assolutamente da mettere posizione esplicita nel json dell'output della validazione, non lasciare che venga inferita dalla struttura!!
* non è un problema che il report del json non sia compatibile con lo schema di JSON Schema per l'output delle validazioni fatte da implementazioni di JSON Schema! Loro hanno il loro schema, tu hai il tuo
* vedi matrice di silvio per questione dei duplicati da trovare  e unificare (è semplice!)
* non ha senso cercare di mappare l'output con lo schema che vuoi tu a delle funzioni di librerie per validare come python-jsonschema
* il JSON Schema del report serve a te per:
  1. Avere un'idea più chiara di cosa ti serve nell'output e come deve essere strutturato
  2. verificare che quello che emette una tua funzione di validazione nel codice che hai scritto sia compatibile con questo schema