---
title: "Meeting 008"
tags:
- timeline
- meeting
date: 2022-10-20 04:02
---
<span 
		class="ob-timelines"
		data-date="2022-10-20-00">
</span>
📑 [**Reference session: 008**](notes/sessions/session%20008.md)

🔙 [**Previous meeting: 007**](notes/meetings/meeting%20007.md)

+ stringhe di messaggi all'utente vanno messe in un file di configurazione esterno, non nella struttura interna del codice!
+ correggi il dizionario dell'errore. Deve essere "ricco" abbastanza da fare in modo che la macchina possa dedurre automaticamente qual è l'errore e in quale posizione è localizzato. Sicuramente è da aggiungere una key-value che renda esplicito il tipo di localizzazione, qualcosa come:
```python
{
...
	"position":{
		...
		"localization_category": 'row' #uno dei seguenti valori: 'row', 'field', 'item'
		...
	}
}
```
Inoltre bisogna rendere "sempre uguale" il modo in cui la posizione viene espressa, mantenendo *sempre* all'interno del dizionario `position` le chiavi per `idx_in_field` (che dovrebbe diventare `item`) e `field` (cioè cambiano solo i valori). Vedi figura:
![Silvio's draft example of the position data expressed in a rich way](images/example_of_rich_position_dictionary.jpg)


