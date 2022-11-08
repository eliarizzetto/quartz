---
title: "Session 011"
tags:
- timeline
- session
date: 2022-11-08 02:19
---
<span 
		class='ob-timelines'
		data-date="2022-11-08-00">
</span>

👥 [**Reference meeting: 011**](notes/meetings/meeting%20011.md)

🔙 [**Previous work session: 010**](notes/sessions/session%20010.md)

# Duplicates

* Ho fatto in modo di creare, nella struttura principale, una lista che viene appositamente aggiornata ogni volta che si incontra un field rilevante (`citing_id`,`cited_id` o `id`) in una data `row`, aggiungendovi un set con tutti gli item che sono risultati validi per una specifica cella contente `'id'`.
* Seguendo le indicazioni di Silvio in [meeting 010](notes/meetings/meeting%20010.md), usando le informazioni nella lista sopra (cioè il fatto che 2 o più ID siano presenti insieme nella stessa cella), ho costruito una lista in cui ogni set corrisponde NON al contenuto di una cella, ma a una entità bibliografica (appunto rappresentata come set contenente gli identificativi con cui essa è rappresentata nel documento).
	* vedi `group_ids()` in [helper_functions.py](https://github.com/eliarizzetto/thesis_resources/blob/1f8c325eb96eabb5261e7004df458eecab107ee8/CITS/helper_functions.py) nel branch dedicato ai duplicates
	* **FONTE FONDAMENTALE**: https://stackoverflow.com/a/51739608/20184608
		* Avevo anche pensato, considerando le altre soluzioni proposte su SO, di leggere la tabella come un grafo, ma le cose si facevano complesse. Questa soluzione funziona e non è troppo difficile da capire, ma potrebbe non essere molto efficiente. Ho anche provato a ristrutturarla e scriverla come una funzione ricorsiva, per tentare di renderla più elegante (?), poi ho lasciato stare. 
* Ho creato una funzione ([get_duplicates.py](https://github.com/eliarizzetto/thesis_resources/blob/1f8c325eb96eabb5261e7004df458eecab107ee8/CITS/get_duplicates.py)) che legge la tabella alla luce della "mappatura" tra ID e entità bibligrafiche "implicite", e ritorna un dizionario con gli errori relativi. Gli errori sono di due tipi:
	* self-citation: quando la stessa bibliographic entity è presente sia in citing_id che in cited_id nella stessa row
	* duplicate citation o duplicate row: quando la stessa citazione (collegamento tra entità bibliografiche) è rappresentata in più di una riga.
	
	Gli errori nel dizionario in output da `get_duplicates_cits()` hanno già tutte le informazioni che servono per essere aggiunti al final_report.

