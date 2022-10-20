---
title: "Session 008"
tags:
- timeline
- session
date: 2022-10-20 04:02
---
<span 
		class='ob-timelines'
		data-date="2022-10-20-00">
</span>

👥 [**Reference meeting: 008**](notes/meetings/meeting%20008.md)

🔙 [**Previous work session: 007**](notes/sessions/session%20007.md)

## JSON Schema del report di errori e funzione di controllo

**Folder nella repo GH**: https://github.com/eliarizzetto/thesis_resources/tree/main/check_output

Ho prodotto **2 schemi** per rappresentare l'output della validazione:
* uno per l'output completo, cioè una lista di dizionari (o un array di oggetti in JSON), in cui ciascun dizionario rappresenta l'errore con i relativi dettagli
* uno per l'output di un qualsiasi "step" del processo di validazione, preso isolatamente, ovvero un dizionario che è esso stesso la rappresentazione dell'errore con i relativi dettagli.

Nella stessa cartella c'è anche uno **script** che usa python-jsonschema per validare contro uno di questi due JSON schemas uno qualsiasi dei dizionari prodotti in una fase del processo di validazione, così da poter controllare, ogni volta che creo un nuovo dizionario corrispondente a un errore, di farlo secondo il JSON schema che ho definito. 

N.B. Ho cambiato idea radicalmente su come strutturare il report degli errori. Ora si configura come una lista (piatta!!) di dizionari, in cui ogni dizionario rappresenta un'errore. Ciascuno di questi dizionari contiene coppie key-value che rappresentano:
* il tipo di errore (error, warning)
* il livello di validazione in cui è emerso l'errore (csv_wellformedness, external_syntax, semantic)
* il messaggio (costruito in base a quale funzione di validazione è stata "fallita")
* la posizione, ovvero una chiave che ha come valore a sua volta un dizionario con key-values che indicano row, field e posizione all'interno del field in cui l'errore è collocato. Ciò che cambia è come/dove si ottiene l'informazione della posizione dell'errore; non più implicitamente dalla struttura, ma direttamente a partire dal dizionario corrispondente.

> [!example] Esempio di singolo dizionario, rappresentante un errore

```python {title="Un esempio di dizionario per un errore"}
error =  {
  'error_type': 'error',
  'message': 'The value in this field is not expressed in compliance with the '
             "syntax of OpenCitation CITS-CSV. Each identifier in 'citing_id' "
             "and/or 'cited_id' must have the following form: ID abbreviation "
             '+ “:” + ID value. No spaces are admitted within the ID. Example: '
             'doi:10.48550/arXiv.2206.03971. The accepted prefixes (ID '
             "abbreviations) are the following: 'doi', 'issn', 'isbn', 'pmid', "
             "'pmcid', 'url', 'wikidata', 'wikipedia'. ",
  'position': {
	  'field': 'citing_id', 
	  'idx_in_field': 0, 
	  'row': 0
  },
  'validation_level': 'csv_wellformedness'
}
```

## Funzione base per creare i dizionari dei singoli errori

Vedi [script](https://github.com/eliarizzetto/thesis_resources/blob/17c4967d10395f15a98d4c23677587443d72e534/CITS/create_report.py) su GH. **MANCA CHIAVE VALID E RELATIVO FLAG: AGGIUNGI!!**


## Prime funzioni per controllare conformità a sintassi per CITS-CSV
Vedi [script](https://github.com/eliarizzetto/thesis_resources/blob/17c4967d10395f15a98d4c23677587443d72e534/CITS/validation_functions.py) su GH. 

## Bozza per la struttura del processo di validazione principale

Vedi [script](https://github.com/eliarizzetto/thesis_resources/blob/17c4967d10395f15a98d4c23677587443d72e534/CITS/validate_cits.py) su GH. Mancano naturalmente delle parti ma il criterio si capisce: per ogni cosa che voglio controllare ho una funzione specifica (che ritorna un booleano) e un messaggio specifico. Itero tutti gli elemento iterabili del csv e per ognuno di questi passo le/a funzioni/e rilevanti; se la funzione di controllo ritorna False, allora viene chiamata la funzione per la creazione del dizionario d'errore, con gli argomenti corretti: le key-values di  `position`  si deducono dalla posizione nell'iterazione, `validation_level` dal tipo di funzione che è stata mandata, `error` si decide in base a che peso si vuol dare a quello specifico errore, ecc.


```python {title="La bozza della struttura per il processo di validazione (CITS-CSV)"}

#assumiamo che stiamo iterando tutte le row nel csv, letto come lista di dizionari (le rows)
for field, value in row.items():  
    if content(value):  
  
        if field == 'citing_id' or field == 'cited_id':  
            if not wellformedness_id_field(value):  
                message = "The value in this field is not expressed in compliance with the syntax of OpenCitation " \  
                          "CITS-CSV. The content of 'citing_id' and 'cited_id' must be either a single ID or a sequence" \  
                          " of IDs, each separated by one single space character (Unicode Character “SPACE”, U+0020). " \  
                          "Each ID must not have spaces within itself, and must be of the following form: " \  
                          "ID abbreviation + “:” + ID value. "  
  
                error_final_report.append(create_error_dict(validation_level='csv_wellformedness', error_type='error', message=message,  
                                          row=0, field=field))  
  
            for id_idx, id in enumerate(value.split()):  
  
                if not wellformedness_single_id(id):  
                    message = "The value in this field is not expressed in compliance with the syntax of OpenCitation " \  
                              "CITS-CSV. Each identifier in 'citing_id' and/or 'cited_id' must have the following " \  
                              "form: ID abbreviation + “:” + ID value. No spaces are admitted within the ID. Example: " \  
                              "doi:10.48550/arXiv.2206.03971. The accepted prefixes (ID abbreviations) are the " \  
                              "following: 'doi', 'issn', 'isbn', 'pmid', 'pmcid', 'url', 'wikidata', 'wikipedia'. "  
  
                    error_final_report.append(create_error_dict(validation_level='csv_wellformedness', error_type='error',  
                                              message=message, row=0, field=field, idx_in_field=id_idx))  
  
                    # -------------ADD CHECK ON LEVEL 2 (EXTERNAL SYNTAX) AND 3 (SEMANTICS) FOR THE SINGLE IDs  
  
        if field == 'citing_publication_date' or field == 'cited_publication_date':  
            if not wellformedness_date(value):  
                message = "The value in this field is not expressed in compliance with the syntax of OpenCitation " \  
                          "CITS-CSV. The content of 'citing_publication_date' and/or 'cited_publication_date' must be " \  
                          "of one of the following forms (according to standard ISO 86014): YYYY-MM-DD, YYYY-MM, " \  
                          "YYYY. If year is required, month and day are optional. If the day is expressed, " \  
                          "the day must also be expressed. Examples: '2000'; '2000-04'; '2000-04-27'."  
  
                error_final_report.append(create_error_dict(validation_level='csv_wellformedness', error_type='error',  
                                          message=message, row=0, field=field))
```

