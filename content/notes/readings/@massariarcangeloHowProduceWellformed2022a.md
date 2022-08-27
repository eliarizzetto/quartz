---
title: "massariarcangeloHowProduceWellformed2022a"
authors: Arcangelo Massari
tags:
- reading
- oc
- meta
- croci
---
# Notes on *How to produce well-formed CSV files for OpenCitations*
Author(s): **Arcangelo Massari**
Year: **2022**
DOI: **10.5281/ZENODO.6597140**

🔗 [Go to web version](https://zenodo.org/record/6597140)
🗃️ [Open this document in Zotero](zotero://select/items/@massariarcangeloHowProduceWellformed2022a)

> [!abstract]+ Abstract
>
> OpenCitations processes two types of CSV files, one for metadata and one for citations. This document describes how to structure them.

This document is the most complete reference for the syntax of the CSV files for sending metadata and citation data to OpenCitations and allow this data to be processed. 

## META-CSV: syntax
The csv file for the metadata is structured as a table with 11 columns, where each row corresponds to a specific document. If an ID is specified, the other fields are not mandatory; otherwise, certain fields become mandatory, according to the [type](notes/readings/@massariarcangeloHowProduceWellformed2022a.md#type) of resource. 

<p align=center style="font-weight:bold;color:#0099cc">
For a complete set of examples of META-CSV data entries (sample table) see <a href="https://github.com/opencitations/oc_meta/blob/master/example_metadata.csv"><u>oc repo on github</u></a>.
</p>

### id
There may be one or more IDs, and they are separated by a single space (Unicode Character “SPACE”, U+0020)

Syntax: 

<p align=center style="font-weight:bold;">ID abbreviation + “:” + ID value</p>

> [!example]- Examples
> 
>  "doi:10.3233/ds-170012" indicates a DOI identifier with value “10.3233/ds-170012”

### title
Text string corresponding to the document's title. 
### author
Author's/authors' full name and, optionally, identifier. An author's ID is indicated with the same syntax as for a [document's id](notes/readings/@massariarcangeloHowProduceWellformed2022a.md#id), but inside square brackets. If there are no IDs, there will be no square brackets at all. **The author's *last name* is optional, but even without the first name, the comma after the last name is still necessary!**

Syntax: 

<p align=center style="font-weight:bold;">Family Name + “,” + “ ” + Given Name + “ ” + “[” + IDs + “]”</p>

> [!example]- Examples
> 
>  * Peroni, Silvio [orcid:0000-0003-0530-4305]
>  * Peroni, [orcid:0000-0003-0530-4305]


### pub_date
The publication date of the document, expressed according to ISO 86014, the ISO standard for “Representation of dates and times”: 

<p align=center style="font-weight:bold;">YYYY-MM-DD</p>

YYYY is a value between and including 0000 and 9999, MM between and including 01 and 12, DD between and including 01 and 31.  Year, month and day are separated with a hyphen “-”(Unicode Character “HYPHEN-MINUS”, U+002D), as required by the standard.
The publication ***year*** is **mandatory**; month and day are optional, but **if the day is specified, the month must be specified too**. 

> [!example]- Examples
> 
>  * 1996-05-16
>  * 2022-08
>  * 2023

> [!warning]+ Attention!
> 
> Dates like "1996-4-14" or "1996-4" are NOT VALID, despite being listed as valid examples on the paper. 

### venue
The bibliographical resource to which the document belongs. For example, if a row describes the metadata of a journal article, the venue will be the journal to which that article belongs.

<p align=center style="font-weight:bold;">Venue Title + “ ” + “[” + IDs + “]”</p>

**If there are no identifiers, the square brackets are not necessary.**

> [!example]- Examples
> 
>  * Archives on Internal Medicine [issn:0003-9626]
>  * Disaster Medicine and Public Health Preparadness [issn:1935-7893 issn:1938-744X]


### volume
The `volume` field stores the the identifier (<u>e.g. a number</u>) of the volume (part of a sequence "inside" a journal) containing the described document. <u>One or more volumes constitute a journal</u>. 
**Required only if the document is contained in a journal volume**.

### issue
The `issue` field stores the the identifier (<u>e.g. a number</u>) of the issue (part of a sequence "inside" a volume) containing the described document. <u>One or more issues constitute a volume (inside a journal)</u>. 
**Required only if the document is contained in a journal issue**.

### page
The page *range* of the resource described in the row. The value is composed of 2 numbers, first and last page respectively, divided by a hyphen “-”(Unicode Character “HYPHEN-MINUS”, U+002D).

> [!example]- Examples
> 
>  * 22-75

### type
The type of resource described in the row. Here is a complete list of the currently supported bibliographic resource types: book, book chapter, book part, book section, book series, book set, book track, component, dataset (or data file), dissertation, edited book, journal, journal article, journal issue, journal volume, monograph, other, peer review, posted content (or web content), proceedings, proceedings article, proceedings series, reference book, reference entry, report, report series, standard, and standard series.

### publisher
The entity responsible for making the resource available.  Publisher name + “ ” + “[” + IDs + “]” square brackets should not be entered if there is no ID.

<p align=center style="font-weight:bold;">Publisher name + “ ” + “[” + IDs + “]”</p>

Square brackets should not be present (i.e. are not necessary???) if there is no ID. 

### editor
Described in the same way as the `author` field.


#### Mandatory fields
If the ID of the document is specified, no other field is mandatory. Otherwise, i.e. if the ID is not specified, certain fields become compulsory, depending on the [type](notes/readings/@massariarcangeloHowProduceWellformed2022a.md#type) of the resource, as described by the table below. 

| ![massariarcangeloHowProduceWellformed2022a](images/massariarcangeloHowProduceWellformed2022a.jpg) |
| --------------------- |
| Fig. 2. Summary of mandatory fields in a metadata CSV if no identifier was specified in a specific row. “M” is an abbreviation for mandatory. Conversely, “O” stands for OR, is always present in pairs, and means that at least one element of the pair is compulsory. |

## CITS-CSV

The csv for citations is structured as a 4-column table, where each row corresponds to a citation (remember citations in OC are represented ad first class data entities).
The citing_id and cited_id fields are mandatory, while the citing_publication_date and cited_publication_date are optional. 

<p align=center style="font-weight:bold;color:#0099cc">
For a complete set of examples of CITS-CSV data entries (sample table) see <a href="https://github.com/opencitations/oc_meta/blob/master/example_citations.csv"><u>oc repo on github</u></a>.
</p>

### citing_id
**Mandatory**. The ID of the citing entity. Expressed in the same way as the ID in META-CSV.

<p align=center style="font-weight:bold;">ID abbreviation + “:” + ID value</p>

### citing_publication_date
**Optional**. Publication date of the citing entity, expressed in the same way as the publication date in META-CSV (ISO 86014 format).

<p align=center style="font-weight:bold;">YYYY-MM-DD</p>

When specifying a value in this field, it is mandatory to specify at least the publication year. On the other hand, month and day are not required. However, if the day is specified, the month must be specified.

### cited__id
**Mandatory**. Publication date of the cited entity. Follows the same rules as the citing_id field.
### cited_publication_date
**Optional**. Follows the same rule as the citing_publication_date field. 

