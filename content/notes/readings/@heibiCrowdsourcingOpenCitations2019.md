---
title: "Crowdsourcing open citations with CROCI – An analysis of the current status of open citations, and a proposal"
authors: Ivan Heibi, Silvio Peroni, David Shotton
tags: 
- reading
- oc
- coci
- croci
- crossref
---
# Notes on *Crowdsourcing open citations with CROCI – An analysis of the current status of open citations, and a proposal*
Author(s): **Ivan Heibi, Silvio Peroni, David Shotton**
Year: **2019**

🔗 Go to web version: http://arxiv.org/abs/1902.02534
🗃️ [Open this document in Zotero](zotero://select/items/@heibiCrowdsourcingOpenCitations2019)

> [!abstract]+ Abstract
>
> In this paper, we analyse the current availability of open citations data in one particular dataset, namely COCI (the OpenCitations Index of Crossref open DOI-to-DOI citations; http://opencitations.net/index/coci) provided by OpenCitations. The results of these analyses show a persistent gap in the coverage of the currently available open citation data. In order to address this specific issue, we propose a strategy whereby the community (e.g. scholars and publishers) can directly involve themselves in crowdsourcing open citations, by uploading their citation data via the OpenCitations infrastructure into our new index, CROCI, the Crowdsourced Open Citations Index.

## Summary
In this paper the authors analyse the state of availability and "openness" of the papers included, at Oct. 3rd 2018, in the COCI index. COCI was built from a dump of two datasets from Crossref:
* a set of **completely open reference lists**
* and a set of **limited access reference lists**, available only to users of the Crossref Cited-by service and to Metadata Plus members of Crossref, of which OpenCitations is one

The results of this (comparative) analysis have shown the need for a new index, CROCI (Crowdsourced Open Citations Index), to collect citations directly from institutions and users authenticated users. 

------------

> [!QUOTE]
> "The availability of **open scholarly citations – i.e. citation data that are structured, separate, open, identifiable and available**[^1] – is a public good, which is of intrinsic value to the academic world as a whole [...], and is particularly crucial for the scientometrics and infometrics community, since it supports reproducibility [...] and enables fairness in research by removing such citation data from behind commercial paywalls."
> p. 1
> see [@shottonPublishingOpenCitations2013](notes/readings/@shottonPublishingOpenCitations2013.md)

[^1]: Peroni, S. & Shotton, D. (2018). Open Citation: Definition. Version 1. Figshare. DOI: 10.6084/m9.figshare.6683855


Despite the recent initiatives to free scholarly citations from the restrictions dictated by commercial interests, many citation are still not freely available, and the incomplete coverage of open citation data is one of the major obstacles to open scholarship. 

In this paper the authors analyse [COCI](https://opencitations.net/index/coci), the OpenCitations Index of Crossref open DOI-to-DOI citations. COCI is provided by OpenCitations[^2],  <u>"a scholarly infrastructure organization dedicated to open scholarship and the publication of open bibliographic and citation data by the use of Semantic Web (Linked Data) technologies</u>" (p. 1). 

[^2]: Peroni, S., Dutton, A., Gray, T. & Shotton, D. (2015). Setting our bibliographic references free: towards open citation data. Journal of Documentation, 71: 253-277. DOI: 10.1108/JD-12-20130166

COCI was launched in July 2018 and it is the first of OpenCitations' indexes to have citations represented as first-class data entries with accompanying properties. 

## Findings
Analysing COCI, the authors are addressing the following research questions (RQ):
#### RQ1: What is the ratio between open vs closed citations inside COCI (for each of the five scholarly entities in COCI, i.e. journals, books, proceedings, datasets and others)?

![fig1](images/heibiCrowdsourcingOpenCitations2019-fig1.jpg)

#### RQ2: What are the top 20 *publishers* that has received the highest number of *open* citations for their publications, according to the data in COCI?

![fig2](images/heibiCrowdsourcingOpenCitations2019-fig2.jpg)

#### RQ3: How much do these publishers (that, in a way, are benefiting from the availability of open citations) contribute to the open citation movement be making their references open in Crossref?

| ![fig3](images/heibiCrowdsourcingOpenCitations2019-fig3.jpg) |
| ---------------|
| "Figure 3. The contributions to open citations made by the twenty publishers listed in [Figure 2](images/heibiCrowdsourcingOpenCitations2019-fig2.jpg), as of 24 January 2018, according to the data available through the Crossref API. The counts listed in the first three results columns of this table refers to the number of publications for which each publisher has submitted metadata to Crossref that include the publication’s reference list, the categories closed, limited and open referring to publications for which the reference lists are not visible to anyone outside the Crossref Cited-by membership, are visible only to them and to Crossref Metadata Plus members, or are visible to all, respectively. [...] The fourth results column in the table shows the total number of publications for which the publisher has submitted metadata to Crossref, whether or not those metadata include the reference lists of those publications, and the fifth results column shows the total number of publications for which the publisher has submitted the reference list with the other metadata. The percentage values given in parentheses show the percentage of publications in each category whose metadata submitted to Crossref includes the reference lists, these percentages being obtained by dividing the values in each column by the total number of publications for which that publisher has submitted metadata to Crossref shown in the fourth results column." (p. 4) |

## Observations

The journal category is the one receiving the highest number of open citations overall; however, the number of *closed* citations to journal articles represents the 43% of the total number of closed citations within Crossref. 

The largest publisher of journal articles, Elsevier, owns the works referenced by 1/3 of the closed citations in Crossref: "Elsevier’s present refusal to open its article references is contributing significantly to the invisibility of Elsevier’s own publications within the corpus of open citation data that is being increasingly used by the scholarly community for discovery, citation network visualization and bibliometric analysis." (pag. 5).

### CROCI, the Crowdsourced Open Citations Index

Despite the remarkable results of the I4OC, many citations are still not available to the general public. 

In order to solve this problem, without ending up fighting against publishers like Elsevier, the authors propose a new index for OpenCitations: **CROCI: the Crowdsourced Open Citations Index**. 
Any individual identified by an [ORCiD](https://en.wikipedia.org/wiki/ORCID) identifier can submit the citation data they are legally entitled to submit; the citation data is published under [CC0 license](https://creativecommons.org/share-your-work/public-domain/cc0/) to ensure its free reuse. 

<p style="background-color: #D0FDEF;">
"Since citations are statements of fact about relationships between publications (resembling statements of fact about marriages between individual persons), <b>they are not subject to copyright</b>, <b>although their specific textual arrangements within the reference lists of particular publications may be</b>. <u>Thus, the citations from which the reference list of an author’s publication has been composed may legally be submitted to CROCI, although the formatted reference list cannot be</u>. Similarly, citations extracted from within an individual’s electronic reference management system and presented in the requested format may be legally submitted to CROCI, irrespective of the original sources of these citations."(p.6)
</p>

Researchers and anyonw who wants to contribute to CROCI by submitting data must provide a CSV file with 4 columns (fields): `citing_id`, `citing_publication_date`, `cited_id` and `cited_publication_date`.[^3]


[^3]: For more information on the format, see page 7 of the paper


If submissions include citation already present in CROCI, the duplicates will automatically be ignored. 


