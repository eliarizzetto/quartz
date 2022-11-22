---
title: "Meeting 013"
tags:
- timeline
- meeting
date: 2022-11-22 04:32
---
<span 
		class="ob-timelines"
		data-date="2022-11-22-00">
</span>
📑 [**Reference session: 013**](notes/sessions/session%20013.md)

🔙 [**Previous meeting: 012**](notes/meetings/meeting%20012.md)

### Updates:
* https://github.com/eliarizzetto/thesis_resources/tree/main/CITS
* regex per controllare il formato degli ***item*** dei RA (il formato di tutto l'item, <u>indistintamente</u> rispetto alla posizione degli errori all'interno dell'item stesso: l'errore è lo stesso se si trova negli ID o nella parte dei nomi).

### Domande:
* **Ha senso controllare che al di fuori delle quadre in `author`, `editor` e `publisher` non ci siano delle stringhe che potrebbero facilmente essere degli identificativi?** 
	Per ora trovo con una regex casi in cui c'è un singolo ID o una serie di IDs senza quadre intorno, ma non funziona sempre (ad esempio, se c'è uno spazio all'inizio della stringa non trova match). --> se è utile cerco di sistemarlo (il problema principale è che non so come includere più casistiche rispetto a quelle rilevabili finora *senza* includere anche quelle che voglio lasciare fuori, ovvero quando l'ID si trova giustamente dentro le quadre).
* Mi confermate che se un ID per BR (qualsiasi ID per BR) si trova su più di una riga su META-CSV, allora si tratta certamente di un duplicato?
