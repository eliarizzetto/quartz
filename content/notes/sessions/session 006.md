---
title: "Session 006"
tags:
- timeline
- session
date: 2022-10-06 04:16
---
<span 
		class='ob-timelines'
		data-date="2022-10-06-00">
</span>

👥 [**Reference meeting: 006**](notes/meetings/meeting%20006.md)

🔙 [**Previous work session: 005**](notes/sessions/session%20005.md)

## Report degli errori
Nell'ottica di dover validare quanti più dati possibile ad ogni esecuzione, è necessario che l'output del processo di controllo sia un report completo, cioè i risultati che è stato possibile ottenere per ciascun dato da **tutta** la tabella. 
L'idea, per ora, è innanzitutto di costruire un **report** (dizionario) che contenga le informazioni **minime**, ovvero:
1. la *posizione* di un errore all'interno del .csv (implicita, in base alla <u>struttura</u> del dizionario)
2. il *livello* di validazione a cui l'errore è stato riscontrato (conformità al modello OC, sintassi esterna, validità semantica)
3. il *tipo di errore* che è stato riscontrato (error, warning, info)

La posizione di un errore è implicita nella struttura del dizionario, cioè non è necessario aggiungere una chiave all'interno del dizionario, ma basta fare in modo che questo sia strutturato in maniera coerente; da qui poi si potrà dedurre come mappare queste informazioni localizzandole nel .csv inviato dall'utente. 

Dal livello di validazione e del tipo di errore (entrambi resi espliciti con le rispettive coppie key-value nel dizionario, ma resi espliciti solo in caso di errore) si potrà poi:
* mappare l'errore rispetto alla posizione del dato in cui è stato riscontrato, localizzandolo nel .csv in input
* dedurre, insieme alle informazioni sulla posizione, quale sia il messaggio da dare all'utente e renderlo il più preciso possibile in base alla combinazione di queste tre informazioni. 


```python {title="General minimal schema for the error report"}
report = {  
    row_position : {  
        field: {  
            element_in_field: {  
                'validation_level' : ['1', '2', '3'],#1 solo valore di questi
                'error_type': ['error', 'warning', 'info']#1 solo valore di questi
            }  
        }  
        'row_format': {  
            'validation_level' : '1',  
            'error_type': ['error']  
        }  
    }  
}
```


L'esempio del report di un csv con 2 soli errori,  uno in uno degli ID, diciamo `doi:10/1234567`, nel campo 'id' della terza riga, e l'altro nel campo autore - preso nel complesso - della sesta riga, potrebbe essere:
```python (title="Report example")
report = {
		  2:{
			 'id':{
			 'doi:10/1234567':{
				 'validation_level': '2', 
				 'error_type': 'error'
				 }
			 }
		 }
		  
		  5:{
		  'author':{
			  'validation_level': '1', 
			  'error_type': 'error'
			  }
		  }
}
```

## Struttura del codice
L'idea per ora è che ci sia una **funzione centrale** che richiama delle **funzioni che si occupano ciascuna di uno specifico controllo**.

#### Funzioni di controllo
Le funzioni di controllo idealmente funzionano tutte allo stesso modo:
* restituiscono `True` se il controllo non dà errori di alcun tipo
* altrimenti, se viene rilevato uno dei tre tipi di errori, restituiscono un dizionario contenente soltanto le chiavi `'validation_level'` e  `'error_type'` con il relativo valore (uno solo per chiave).

Per tutti e tre i "contenitori" interni al .csv che vengono iterati (quindi lista di rows, row e field, ma non l'eventuale singolo elemento all'interno di alcuni field, che costituisce l'elemento minimo)  **solo se nell'elemento più interno ci sono degli errori**, viene creato un dizionario con delle chiavi che corrispondono a un *pointer* dell'elemento interno che contiene l'errore (cioè la posizione della riga all'interno della lista di righe, la chiave del field all'interno della riga, la stringa dell'elemento all'interno del field); a queste chiavi sono associati come valori dei dizionari che contengono le informazioni relative all'errore (ovvero i dizionari con le chiavi  `'validation_level'` e  `'error_type'`).

Quindi:
1. Apro il .csv
2. Itero il contenitore lista di dizionari (rows):
	+ se una delle row contiene errori a livello del contenitore riga, aggiungo il dizionario con la chiave che  è la posizione della riga e il valore che è il dizionario degli errori
	+ altrimenti, se non ci sono errori a livello di riga, il dizionario non viene creato
	3. Itero il contenitore row:
		+ se uno dei field contiene errori a livello del contenitore field, aggiungo il dizionario con la chiave che è il nome del field e il valore che è il dizionario degli errori
		+ altrimenti, se non ci sono errori a livello di field, il dizionario non viene creato
		4. Itero il contenitore field (se si tratta di un field per cui ha senso farlo):
			+ se uno degli elementi all'interno del field (laddove è possibile suddividerlo) contiene errori, aggiungo il dizionario con la chiave che è la stringa all'interno del field e il valore che è il dizionario degli errori. 
			+ altrimenti, se non ci sono errori a livello di elemento interno al field, il dizionario non viene creato.

#### Funzione principale

Nella funzione principale gestisco il salvataggio degli errori per ogni livello e l'ordine in cui vanno chiamate le varie funzioni di controllo.


### Codice finora

Struttura funzione: https://github.com/eliarizzetto/thesis_resources/blob/402233889133d016023eddec3d5ac72c9ba5a6aa/validation_process/validation/validate_main.py

Report errori: https://github.com/eliarizzetto/thesis_resources/blob/402233889133d016023eddec3d5ac72c9ba5a6aa/validation_process/validation/report_schema.py

------------------
1. Ha senso la struttura, in particolare l'idea di creare dei dizionari **nel contenitore più esterno**<u> a condizione che</u> ci sia un certo output dalla funzione che  controlla un elemento interno? È possibile fare questa cosa
	+ Se creo un dizionario *prima* di verificare se ogni elemento dell'iterable passa il controllo, sto chiaramente creando un dizionario che potenzialmente potrebbe rimanere vuoto (nel caso in cui non ci siano errori), e lo sto facendo per ogni elemento che sia all'interno di un contenitore/iterable! Come nel codice qui sotto.
	+ D'altra parte, altrimenti come posso creare un dizionario unico per tutti gli elementi all'interno dell'iterable senza metterlo al di fuori del for loop?

```python 
def id_lev1(whole_id_field):
id_field_pattern_oc_prefixes = r'...'

if match(id_field_pattern_oc_prefixes, whole_id_field):  
	return True
else:  
	result = dict()  # <----- {} per ogni elemento in field, a prescindere!
	for item in whole_id_field.split():  
		if id_items_lev1(item) != True: 
			result[item] = id_items_lev1(item)# <--lo popolo solo se ci sono errori
	return result
```

2. Ha senso che io mi preoccupi di dividere gli elementi interni a un field?
3. Ha senso preoccuparmi di verificare aspetti diversi di uno stesso livello di validazione, per sperare di riuscire a fornire un feedback preciso; ovvero, ha senso che faccia controlli separati con le regex con i prefissi specifici e quelle senza. 
4. Ha senso pensare di salvare prima tutti gli errori e poi condizionare in base ai risultati nel report (anche in base al tipo di errore) il passaggio ai successivi livelli di validazione?