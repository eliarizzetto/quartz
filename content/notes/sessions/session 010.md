---
title: "Session 010"
tags:
- timeline
- session
date: 2022-10-31 15:47
---
<span 
		class='ob-timelines'
		data-date="2022-10-31-00">
</span>

👥 [**Reference meeting: 010**](notes/meetings/meeting%20010.md)

🔙 [**Previous work session: 009**](notes/sessions/session%20009.md)

- [x] represent the position of an error as a tree, not as lists
- [x] update the JSON schemas of the error reports
- [x] config file for messages
- [ ] re-think main structure
- [x] update calls of `create_error_dict` inside main structure
- [ ] duplicates on CITS-CSV




* All'interno di `error_dict[position][table]`, `None` è un valore possibile ***solo*** per la chiave `item`, che a questo punto può avere come valori:
	* o una <u>**lista di integers**</u>, che sono gli indici degli item interessati dall'errore
	* o **None**, ma MAI all'interno di una lista
	`None` si usa solo quando un field è una stringa vuota o contenente solo "null", "nan" o "none" (ma questa cosa è da vedere meglio[^1]); invece, se una cella contiene solo spazi, quegli spazi costituiscono a tutti gli effetti degli item, e vanno quindi rappresentati come item all'interno di una lista. La ragione per cui `None` non va dentro ad una lista è che se la stringa è vuota, all'interno di quella stringa non ci entro proprio. <u>Tieni comunque presente che se splitti una stringa vuota, il risultato è una lista contenente una sola stringa vuota, quindi il comportamento della struttura di base potrebbe essere semplicemente che per ogni field la lista si crea comunque, e i controlli si passano per tutti gli elementi di quella lista (se c'è una stringa vuota all'interno di un field in cui non dovrebbe esserci, la validazione fallirà comunque!). **Se però il field è tra quelli che contengono un solo item "semantico", ad esempio titolo o data, la lista non puoi farla con .split(), ma devi invece considerare la stringa come lista con un solo elemento.**</u>


[^1]: Probabilmente è il caso di considerare come contenuto vuoto solo il caso in cui la stringa sia *effettivamente* vuota. Nel caso in cui una cella contenga "null", "nan" o "none", quella cella, più che non avere contenuto, ha un contenuto sbagliato, il che è diverso! --> bisognerebbe cambiare la funzione `content()`. Questo perché "null", "nan" o "none" costituiscono comunque dei dati che nella maggior parte dei casi non si possono prendere come buoni (anche se non possiamo saperlo a priori), e che comunque fallirebbero la validazione in altri controlli. 