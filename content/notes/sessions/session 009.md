---
title: "Session 009"
tags:
- timeline
- session
date: 2022-10-25 04:33
---
<span 
		class='ob-timelines'
		data-date="2022-10-25-00">
</span>

👥 [**Reference meeting: 009**](notes/meetings/meeting%20009.md)

🔙 [**Previous work session: 008**](notes/sessions/session%20008.md)


## Modifiche al dizionario degli errori
Viste le [indicazioni dell'altra volta](notes/meetings/meeting%20008.md), ho creato un [branch specifico](https://github.com/eliarizzetto/thesis_resources.git) per modificare il dizionario dell'errore in modo che abbia una struttura fissa e coerente e che sia semanticamente ricco e interpretabile (nello specifico, è ideale che sia indicata nel dizionario la posizione precisa e quale errore è emerso):
+ i 2 JSON Schema per il report (dizionario che rappresenta un errore): https://github.com/eliarizzetto/thesis_resources/blob/033002e63ca6a2e38e58aaf14ec17503afd6dd5b/check_output/error_report_schema.json
+ la funzione che crea il report https://github.com/eliarizzetto/thesis_resources/blob/033002e63ca6a2e38e58aaf14ec17503afd6dd5b/CITS/create_report.py
+ la bozza della struttura principale, in cui vengono chiamate le altre funzioni (quelle di validazione e quella che crea il report, *ogni volta* che una funzione di validazione non viene passata. https://github.com/eliarizzetto/thesis_resources/blob/033002e63ca6a2e38e58aaf14ec17503afd6dd5b/CITS/validate_cits.py

### [[Errors map]]
Ho iniziato a compilare un .csv in cui elencare tutti i possibili errori, associandovi le funzioni che li rilevano e quali valori sono da specificare nel relativo report. 


## Domande
Con la [struttura](https://github.com/eliarizzetto/thesis_resources/blob/033002e63ca6a2e38e58aaf14ec17503afd6dd5b/CITS/validate_cits.py) così com'è in questo momento, ogni elemento che si trova all'interno di un iterable, se possibile, viene considerato per sé, poi viene passato in input a una funzione di validazione, e infine se non passa questa funzione viene prodotto un dizionario per rappresentare l'errore specifico (della funzione) che quell'elemento del .csv non ha passato. 

Quindi, all'interno di un field di cui si considerino gli item singolarmente (es. `id`), ogni item viene controllato con la stessa funzione, **cioè per lo stesso errore**, e se non passa viene creato un dizionario che ripete le stesse informazioni per ciascun item (es. `error_type`, `validation_level`, `row`, ecc.). L'unica cosa che cambia è il valore di `position[item]`, che così come le cose stanno ora, in casi come questo ha necessariamente **sempre** una lista con un integer solo, perché la funzione che crea il dizionario viene chiamata **all'interno** del loop. 

Se non è troppo caotico, si potrebbe fare in modo che **tutti** gli item all'interno di una *stessa* cella (cioè stessa row stesso field) che hanno uno *stesso* errore (cioè hanno fallito la stessa funzione) vengano aggiunti a un lista *ad hoc* che viene poi aggiunta al dizionario come valore di `position[item]`. 

Ad esempio, per **uno stesso errore di formato su due diversi item di `citing_id`**, invece che:

```json
 {'error_type': 'error',
  'message': '',
  'position': {'field': ['citing_id'],
               'item': [0],
               'located_in': 'item',
               'row': [0]},
  'validation_level': 'csv_wellformedness'},
 {'error_type': 'error',
  'message': '',
  'position': {'field': ['citing_id'],
               'item': [2],
               'located_in': 'item',
               'row': [0]},
  'validation_level': 'csv_wellformedness'},

```


si avrebbe:

```json
 {'error_type': 'error',
  'message': '',
  'position': {'field': ['citing_id'],
               'item': [0,2],
               'located_in': 'item',
               'row': [0]},
  'validation_level': 'csv_wellformedness'},
```

