---
title: "Session 007"
tags:
- timeline
- session
date: 2022-10-13 04:42
---
<span 
		class='ob-timelines'
		data-date="2022-10-13-00">
</span>

👥 [**Reference meeting: 007**](notes/meetings/meeting%20007.md)

🔙 [**Previous work session: 006**](notes/sessions/session%20006.md)

### [JSON Schema](https://json-schema.org/) per l'output del processo di validazione

```json {title="C:\Users\media\Desktop\report\report.json"}
{  
  "$schema": "https://json-schema.org/draft/2020-12/schema",  
  "$id": "http://example.com/example.json",  
  "type": "object",  
  "required": [  
    "validation_level",  
    "error_type",  
    "message"  
  ],  
  "properties": {  
    "validation_level": {  
      "type": "string",  
      "enum": ["oc_format", "external_syntax", "semantic"]  
    },  
    "error_type": {  
      "type": "string",  
      "enum": ["error", "warning"]  
    },  
    "message": {  
      "type": "string"  
    },  
    "valid": {  
      "type": "boolean"  
    }  
  },  
  "if": {  
    "properties": {"error_type": {"const":"error"}}  
    },  
  "then": {  
    "properties": {"valid": {"const": false}}  
  }  
}
```


### Domande
1. Rendere esplicita la posizione dell'errore, ovvero inserire una proprietà come "position" sia per le rows che per gli elementi interni a un field? suppongo sia necessario...
2. A proposito di JSON Schema: ha senso pensare di utilizzare una libreria basata su JSON Schema per validare il .csv? Ora che sto iniziando a capire la logica di JSON Schema, mi sembra meno difficile di quando ho iniziato a dare un'occhiata a Cerberus e python-jsonschema. Suppongo che entrambe le librerie ammettano di integrare l'utilizzo di funzioni esterne alla libreria, ma il problema maggiore resta la difficoltà (l'impossibilità?) di personalizzare l'output della validazione.
	1. python-jsonschema
	2. pydantic