---
title: "Managing Wiki Identifiers"
tags:
- wikidata
- wikipedia
- qid
---

## MediaWiki Action API
All Wikimedia services (including Wikipedia and Wikidata) are accessible by means of **one** API, the [**MediaWiki Action API**](https://www.mediawiki.org/wiki/API:Main_page). 
All Wikimedia wikis have endpoints that follow this pattern: `https://www.example.org/w/api.php`.

The **endpoints** of interest are the following (the links below, leading to the endpoint-specific documentation, are also the URLs to use when accessing the service-related API):
- [Endpoint for **Wikipedia**](https://en.wikipedia.org/w/api.php)
- [Endpoint for **Wikidata**](https://www.wikidata.org/w/api.php)

## Accessing Wikidata and Wikipedia data
### Wikidata
In order to access Wikidata's data, amongst other solutions, the [Wikidata:Data access](https://www.wikidata.org/wiki/Wikidata:Data_access/en#Using_Wikidata's_data) page suggests to use:
- the **Linked Data Interface** [https://www.wikidata.org/wiki/Wikidata:Data_access/en#Linked_Data_Interface_(URI)](https://www.wikidata.org/wiki/Wikidata:Data_access/en#Linked_Data_Interface_(URI)) for "when you need to obtain individual, complete entities that are already known to you".
- the **MediaWiki Action API** [https://www.wikidata.org/wiki/Wikidata:Data_access/en#MediaWiki_Action_API](https://www.wikidata.org/wiki/Wikidata:Data_access/en#MediaWiki_Action_API) when "you need small groups of entities in JSON format (up to 50 entities per request)"; this help page also mentions that "the API is also poorly suited to situations in which you want to request the current state of entities in JSON. (For such cases consider using the [Linked Data Interface](https://www.wikidata.org/wiki/Wikidata:Data_access/en#Linked_Data_Interface), which is likelier to provide faster responses.)".

### Wikipedia
Similarly, for Wikipedia one can use either the API or the the URI, provided, of course, that the correct URL has been specified accordingly:
* for the English **Wikipedia Action API endpoint**: [https://en.wikipedia.org/w/api.php](https://en.wikipedia.org/w/api.php)
* for the English Wikipedia namespace: *either* [https://en.wikipedia.org/wiki/](https://en.wikipedia.org/wiki/) (with redirection by the browser and without the possibility of specifying parameters) *or* [https://en.wikipedia.org/w/index.php](https://en.wikipedia.org/w/index.php/) (with the possibility of specifying some parameters and without being redirected by the browser).

**NB**: Unlike Wikidata, which has a proper Linked Data Interface, <u>when accessing data via Wikipedia's *bare URLs* (i.e. not by means of the API)</u>, even with the alternative URL (https://en.wikipedia.org/w/index.php), **many useful parameters cannot be specified** in the request: e.g., you cannot specifiy the output format in json! This means that, if you requested a resource via the URL instead of the API, you would have to resort to some scraping library (e.g. Beautiful Soup) to convert the retrieved resource into a useable format in Python. <u>**For Wikipedia, it seems more convenient to use the MediaWiki Action API!**</u>

Bear in mind that, while the Wikidata has the same namespace for every language, **Wikipedia refers to a specific language**. Therefore, you'll have to choose the domain for the English Wikipedia, since it is the most "international" one.


### [**Wikidata's Linked Data Interface (URI)**](https://www.wikidata.org/wiki/Wikidata:Data_access/en#Linked_Data_Interface_(URI))

**url: [https://www.wikidata.org/wiki/Special:EntityData/](https://www.wikidata.org/wiki/Special:EntityData/)**

Eg:

>As a human being with eyes and a browser, you will likely want to access data _about_ Douglas Adams by using the concept URI as a [URL](https://en.wiktionary.org/wiki/Uniform_Resource_Locator "wikt:Uniform Resource Locator"). Doing so triggers an HTTP redirect and forwards the client to the data URL that contains Wikidata's data _about_ Douglas Adams: [https://www.wikidata.org/wiki/Special:EntityData/Q42](https://www.wikidata.org/wiki/Special:EntityData/Q42)"
>
> from https://www.wikidata.org/wiki/Wikidata:Data_access/en#Linked_Data_Interface_(URI)

Each Wikidata entity is identified by an **entity ID**, which is a number prefixed by a letter.

-   <u>[**items**](https://www.wikidata.org/wiki/Help:Items "Help:Items"), also known as Q-items, are prefixed with `Q` </u>(e.g. [Q12345](https://www.wikidata.org/wiki/Q12345 "Q12345")),
-   [properties](https://www.wikidata.org/wiki/Help:Properties "Help:Properties") are prefixed by `P` (e.g. [P569](https://www.wikidata.org/wiki/Property:P569 "Property:P569")) and
-   [lexemes](https://www.wikidata.org/wiki/Help:Lexemes "Help:Lexemes") are prefixed by `L` (e.g. [L1](https://www.wikidata.org/wiki/Lexeme:L1 "Lexeme:L1")).
-  there also seem to be [Entity Schemas](https://www.wikidata.org/wiki/EntitySchema:E2), prefixed by  `E`.


The only way to retrieve precisely a resource from Wikidata is the entity's ID (e.g. Q42); in other words, with aliases and labels in the URL there is no way of getting to the relevant URI.
E.g. `https://www.wikidata.org/wiki/Special:EntityData/Q42` leads to Douglas Adams' page, but `https://www.wikidata.org/wiki/Special:EntityData/Douglas_Adams` doesn't!

#### Sintactic validation: regex for Wikidata identifiers
I have obtained the regex for Q-IDs, as described within wikidata itself, by looking for the property [`format as a regular expression`](https://www.wikidata.org/wiki/Property:P1793 "Property:P1793") in the [wikidata page of the Q-identifier](https://www.wikidata.org/wiki/Q43649390).[^1] 

**Wikidata Q Identifier:**

* [format as a regular expression](https://www.wikidata.org/wiki/Property:P1793 "Property:P1793") : `Q[1-9]\d*`

[^1]: To get the regex for other, external, IDs, provided that they're registered in wikidata, you can do the same by checking if the same property is specified in the wikidata page of the ID you're interested in (either the item page or the property page; e.g. you can look for the `format as a regular expression` property in the [VIAF ID property page](https://www.wikidata.org/wiki/Property:P214))

If needed, the same process can be applied for retrieving the regular expressions for the P-identifier (looking into its wikidata page) and the other [identifiers](https://www.wikidata.org/wiki/Wikidata:Identifiers). The reason why I chose not to do so, is because I want to allow as bibliographic resource or responsible agent in the OC indexes **only** Wikidata's **items** (wikidata properties (P), lexemes (L) and entity schemas (E) seem a very unlikely candidate for being represented in any OC index).



#### Wiki Universe
On the differences between Wikipedia, MediaWiki, Wikimedia, etc. see:
* [https://www.mediawiki.org/wiki/Differences_between_Wikipedia,_Wikimedia,_MediaWiki,_and_wiki](https://www.mediawiki.org/wiki/Differences_between_Wikipedia,_Wikimedia,_MediaWiki,_and_wiki)
* [https://meta.wikimedia.org/wiki/Learning_patterns/Wikiblabla_(confusing_Wikimedia_lexicon)](https://meta.wikimedia.org/wiki/Learning_patterns/Wikiblabla_(confusing_Wikimedia_lexicon))
* [https://www.mediawiki.org/wiki/Manual:What_is_MediaWiki%3F](https://www.mediawiki.org/wiki/Manual:What_is_MediaWiki%3F)
