---
title: "Errors map"
tags:
- validation
- report
- json
- errorreport
---

🔗[Map on GitHub](https://github.com/eliarizzetto/thesis_resources/blob/b1e763e5d181ac445cfea73ecbaef215b565c949/errors_map.csv)


|Error label            |Description                                                                                                                                |table|Corresponding validation function                |validation_level  |error_type|field                                                           |valid|located_in|message|Report example|notes        |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------|-----|-------------------------------------------------|------------------|----------|----------------------------------------------------------------|-----|----------|-------|--------------|-------------|
|duplicates             |a bibliographic record is expressed more than once inside the table                                                                        |both |?                                                |csv_wellformedness|warning   |                                                                |     |row       |       |              |multiple rows|
|extra space in id field|inside the id field, spaces are found at the beginning of the string or at the end, or extra spaces are found between two single ids       |both |if item == '': (FORMERLY wellformedness_id_field)|csv_wellformedness|error     |citing_id', 'cited_id' or 'id'                                  |no   |item      |m1     |              |             |
|wrong id syntax        |the single id (item) is not well-formed according to OC syntax (lowercase accepted prefix followed by colon and other non-space characters)|both |wellformedness_single_id                         |csv_wellformedness|error     |citing_id', 'cited_id' or 'id'                                  |no   |item      |m2     |              |             |
|date                   |the date is badly formatted (does not match the one regex that defines any correct possibility)                                            |both |wellformedness_date                              |csv_wellformedness|error     |citing_publication_date', 'cited_publication_date' or 'pub_date'|no   |item      |m3     |              |             |
