---
title: "@heibiSoftwareReviewCOCI2019"
authors: Ivan Heibi, Silvio Peroni, David Shotton
tags:
- reading
- coci
- cito
- oc
- croci
- sw
- oci
- ocdm
---
# Notes on *Software review: COCI, the OpenCitations Index of Crossref open DOI-to-DOI citations*
Author(s): **Ivan Heibi, Silvio Peroni, David Shotton**
Year: **2019**

🔗 Go to web version: https://doi.org/10.1007/s11192-019-03217-6
🗃️ [Open this document in Zotero](zotero://select/items/@heibiSoftwareReviewCOCI2019)

> [!abstract]+ Abstract
>
> In this paper, we present COCI, the OpenCitations Index of Crossref open DOI-to-DOI citations (http://opencitations.net/index/coci ). COCI is the first open citation index created by OpenCitations, in which we have applied the concept of citations as first-class data entities, and it contains more than 445 million DOI-to-DOI citation links derived from the data available in Crossref. These citations are described using the resource description framework by means of the newly extended version of the OpenCitations Data Model (OCDM). We introduce the workflow we have developed for creating these data, and also show the additional services that facilitate the access to and querying of these data via different access points: a SPARQL endpoint, a REST API, bulk downloads, Web interfaces, and direct access to the citations via HTTP content negotiation. Finally, we present statistics regarding the use of COCI citation data, and we introduce several projects that have already started to use COCI data for different purposes.



## Summary
In this paper the authors introduce COCI, the representation of citations as [first-class data entities](https://en.wikipedia.org/wiki/First_class_(computing)) and the ingestion workflow implemented to create the index; they also provide details on the data contained in COCI and describe services and resources made available by OpenCitations to access the index; finally, they present some statistics about the use of COCI and list some studies and tools that have made use of COCI up to 2019. 

### **COCI**, the OpenCitations Index of Crossref open DOI-to-DOI citations
Launched in **2018**, COCI is the first index of OpenCitations to <u>represent citations as **first-class data entities**</u> with accompanying properties, i.e. individuals of the class `cito:Citation`[^1] , instead of being defined simply as relationships between two entities (the one representing the papers) through the property `cito:cites`.

[^1]: As defined in [CiTo](http://www.sparontologies.net/ontologies/cito), the Citation Ontology, described in Peroni, S., & Shotton, D. (2012). FaBiO and CiTO: Ontologies for describing bibliographic resources and citations. Web Semantics, 17, 33–34. https://doi.org/10.1016/j.websem.2012.08.001.

At mid-2019, COCI contains +445 millions DOI-to-DOI links represented as entities; all the data is under a CC0 license (public domain) and can be accessed and queried in the following ways:
* via SPARQL endpoint
* via dedicated HTTP REST API
* via searching/browsing Web interfaces
* by bulk download (CSV and N-Triples formats)
* by direct access via HTTP content negotiation

While citations are normally defined as links between published entities, <u>regarding, instead, each citation as a data entity in its own right lets one  provide a citation with **descriptive properties**</u>; in the case of COCI the properties that are associated with a citation entity are the following:
1. **citing entity**
2. **cited entity**
3. **citation creation date** (it has the same numerical value of the citing entity's publication date)
4. **citation timespan** (the temporal characteristic of a citation, i.e. the interval between the publication date of the *citing* entity and the the publication date of the *cited* entity)

Treating citations as first-class data entities brings some advantages:
* all the information about each citation is available in one place
* citations are easier to describe, distinguish, count and process; moreover it's possible to keep separate each occurence of a citation, for example we could count the number of times a published entity is cited inside another published entity, or see in which section(s), etc.
* citations are easier to analyse with bibliometric methods (e.g. to determine how citation time spans vary across different disciplines).

### Data Model
The [OCDM (OpenCitations Data Model)](http://opencitations.net/model) (see p. 1218, fig. 2) has been extended according to the new organization: each citation is a member of the `cito:Citation` class.

So as to identify each citation precisely in the open dataset, a new identifier has been develop for citations: the <u>Open Citation Identifier (**OCI**)</u>. An OCI has the following structure: `oci:<citing_entity_identifier>-<cited_entity_identifier>`.
For example, `oci:0301-03018` is a valid OCI for a citation defined within the OpenCitations Corpus, while `oci:0200101080636010705066308070202630663050902001010806360107050663080702026305630301` is a valid OCI for a citation included in Crossref. <u>OCIs are *not* opaque identifiers, instead they carry information about the citation and the published entities linked by the citation</u>[^2]. 

[^2]: There is also an [OCI Resolution Service](http://opencitations.net/oci) based on a Python app that returns citation data from a valid OCI in input. 

### Ingestion Workflow
4 Phases (see pp. 1219-1221):
1. **Global data generation**: the data in Crossref is parsed and processed to extract all the publications having a DOI and their available list of references. 3 datasets are generated in this way: Dates, ISSN and ORCiD.
2. **CSV generation**: a CSV file is generated where each row represents a citation. 
3. **converting into RDF**: the CSV is converted into RDF according to the N-Triples format, following the OWL data model (see fig. 2).
4. **updating the triplestore**: the triples generated in phase 3 are the ones that are added to the OpenCitations indexes


### Statistics, COCI-related services and impact
##### Statistics 
See p. 1221-1222 and [@heibiCrowdsourcingOpenCitations2019](notes/readings/@heibiCrowdsourcingOpenCitations2019.md) for some stats on COCI in mid 2019.

##### Services for accessing and querying COCI
See the beginning of [this paragraph](notes/readings/@heibiSoftwareReviewCOCI2019.md#COCI%20the%20OpenCitations%20Index%20of%20Crossref%20open%20DOI-to-DOI%20citations) and pp. 1222-1224 for a list of services and tools made available for accessing and querying the data.

##### Impact and use
See p. 1225 for a list of tools and services that have made use of COCI. 