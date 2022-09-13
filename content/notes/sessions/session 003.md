---
title: "Session 003"
tags:
- timeline
- session
date: 2022-09-13 02:31
---
<span 
		class='ob-timelines'
		data-date="2022-09-13-00">
</span>

👥 [**Reference meeting: 003**](notes/meetings/meeting%20003.md)

🔙 [**Previous work session: 002**](notes/sessions/session%20002.md)

## Librerie per validazione
* [Cerberus](https://docs.python-cerberus.org/en/stable/)
* [Json Schema](https://json-schema.org/)
Ho provato a capire la documentazione di cerberus, ma mi è sembrato meno vantaggioso di quanto mi aspettassi: ci metto più tempo a capire come funzionano le "validation rules" -ed ad adattarle alle mie esigenze - che non a scriverle da me.

È stato comunque utile per capire quanto e come, in generale, possano essere utili dei software pronti all'uso per la validazione, e ad avere un esempio di riferimento come framework. Eventualmente posso ritornarci sopra quando avrò definito meglio il workflow per la validazione, anche se ne dubito. 


## Framework per il processo di validazione
Dal tentativo di capire Cerberus sono emerse alcune riflessioni, che ho provato a generalizzare qui sotto. In particolare, mi sembra rilevante la necessità di **definire uno schema** che rappresenti dati corretti. È una cosa che - se effettivamente necessaria - spero di poter fare in itinere, vale a dire una volta che ho avrò del codice funzionante da poter "astrarre" in uno schema formale. 


<h3 style="text-align:center;">Software general structure</h3>

Validation consists in checking an input document against a schema: any document that matches the schema is said to “validate” against the schema specified in input.

* **Input**: document, schema, set of error types
* **Output**: boolean value for a specified type of error; error position in document; suggested correction/action.


#### 3 types/levels of validation:

1. **Against OC accepted format** (table structure; string format):
    * problems regarding the OC _table structure_ (CITS-CSV, META-CSV) may occur while checking if, e.g.:
        * all fields are present (header)
        * the values for any required fields are specified coherently with the constraints listed in [@massariHowStructureCitations2022](notes/readings/@massariHowStructureCitations2022.md)
        * etc.
    * problems regarding the string format may be, e.g.:
        * no incorrect spaces in the cell
        * correct use of square brackets in the `id` or `author `fields
        * lowercase in the id prefix
        * etc.
2. **Against ID-specific syntax**
	Validate according to the syntax defined by an external schema, one for each type of accepted ID. The accepted ID in OpenCitations are the following:
	* **Bibliographic resources (br)**: 'doi', 'issn', 'isbn', 'pmid', 'pmcid', 'url', 'wikidata', 'wikipedia'
	* **Responsible Agents (ra)** (authors, editors, publishers): 'crossref', 'orcid', 'viaf', 'wikidata', 'ror'
3. **Semantic Validation**
	Involves:
	* Checking the actual existence of the input entities, represented in the table by their identifier, via request to the ID-specific API
	* (?)Checking the coherence of the information contained in other fields (e.g. type or author) with the information of the entity as it is represented by its identifier in the OC database or in the relative API
	* (?)Checking the conformity of the table with the OC data model (pySHACL)


#### 3 types of “error”

An error type is assigned to an “error” instance (i.e. anything raising any error at any of the validation layers) according to its gravity level, and consequently to its level of “acceptability”:
* **note**
* **warning**
* **error**





## Proposta di impostazione del codice per validare IDs
```python {title="ID_validation_structure1.py"}
import re

id_field = {'id': 'doi:10.1007/978-3-540-88851-2 isbn:9783540888505 isbn:9783540888512'}

id_field_content = id_field['id'].split()

for i in id_field_content:
    if re.match(id, '^doi'):
        #oc-format validation for DOIs
        #syntax validation for DOIs
        #semantic validation for DOIs
        pass

    elif re.match(id, '^issn'):
        #oc-format validation for ISSNs
        #syntax validation for ISSNs
        #semantic validation for ISSNs
        pass
    elif re.match(id, '^isbn'):
        #oc-format validation for ISBNs
        #syntax validation for ISBNs
        #semantic validation for ISBNs
        pass
    elif re.match(id, '^pmid'):
        #oc-format validation for PMIDs
        #syntax validation for PMIDs
        #semantic validation for PMIDs
        pass
    elif re.match(id, '^pmcid'):
        #oc-format validation for PMCIDs
        #syntax validation for PMCIDs
        #semantic validation for PMCIDs
        pass
    elif re.match(id, '^url'):
        #oc-format validation for URLs
        #syntax validation for URLs
        #semantic validation for URLs
        pass

    elif re.match(id, '^wikidata'):
        #oc-format validation for wikidata
        #syntax validation for wikidata
        #semantic validation for wikidata
        pass
    elif re.match(id, '^wikipedia'):
       #oc-format validation for wikipedia
        #syntax validation for wikipedia
        #semantic validation for wikipedia
        pass

    else:
        #ritorna l'errore del tipo giusto, con il messaggio giusto, con la posizione giusta
```

## Test e revisione di IdentifierManager
* Ho iniziato a capire come eseguire dei test con la libreria unittest
* Ho capito la struttura generale delle varie classi di Identifier Manager e dei relativi metodi, in preparazione della scrittura di classi che estendano IdentifierManager consentendo la gestione degli ID finora mancanti.