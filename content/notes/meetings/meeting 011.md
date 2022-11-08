---
title: "Meeting 011"
tags:
- timeline
- meeting
date: 2022-11-08 02:18
---
<span 
		class="ob-timelines"
		data-date="2022-11-08-00">
</span>
📑 [**Reference session: 011**](notes/sessions/session%20011.md)

🔙 [**Previous meeting: 010**](notes/meetings/meeting%20010.md)

## **To do**
* [x] Duplicates
* [x] Correggi JSON schemas e `create_report()` in base a indicazioni in [meeting 010](notes/meetings/meeting%20010.md) (`None` fuori dalla lista;  label specifica per ogni tipo di errore nel report)
* [ ] Completa/rivedi messaggi all'utente
* [ ] Estendi [Errors map](notes/Errors%20map.md)
* [ ] Aggiungi controlli per i **required fields**
* [ ] Verifica strategia per l'ordine/priorità con cui validare la tabella: quali controlli si fanno sempre e quali invece si fanno solo quando altri controlli sono passati?

## Duplicates
Ora si riescono a rilevare gli errori legati alla duplicazione di ID, di bibliographic entities o di citazioni:
- ripetizione della stringa di un ID nella stessa cella → <u>warning</u>
- ripetizione di un entità bibliografica nel campo `citing_id` e `cited_id` della stessa riga  → <u>error</u>
- ripetizione di una citazione (rapporto tra 2 entità bibliografiche) (cioè quando una stessa citazione è rappresentata in più di una riga) → <u>error</u>



## Incontro

* la ripetizione di uno stesso ID nella stessa cella è un **errore** e ha **valid = False**, perché - per lo meno Arcangelo con META - il campo contenente gli ID viene gestito come lista, non come set. Poi è una modifica che Arca potrebbe fare, ma il principio è che se un dato inviato dall'utente richiede delle modifiche al codice per essere integrato, allora è un dato sbagliato!

* la self-citation è **valid=True** perché è una cosa in linea di principio possibile! Però segnalalo comunque come **warning**, perché il più delle volte (?) deriva da un uso sbagliato del data model (?) 