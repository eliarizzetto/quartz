---
title: "persianiProgrammingInterfaceCreating2022"
authors: Simone Persiani, Marilena Daquino, Silvio Peroni
tags:
- reading
- ocdm
- shacl
- shex
- oc
- sw
---
# Notes on *A Programming Interface for Creating Data According to the SPAR Ontologies and the OpenCitations Data Model*
Author(s): **Simone Persiani, Marilena Daquino, Silvio Peroni**
Year: **2022**
DOI: **10.1007/978-3-031-06981-9_18**

🔗 [Go to web version]()
🗃️ [Open this document in Zotero](zotero://select/items/@persianiProgrammingInterfaceCreating2022)

> [!abstract]+ Abstract
>
> The OpenCitations Data Model (OCDM) is a data model for bibliographic metadata and citations based on the [SPAR Ontologies](http://www.sparontologies.net/) and developed by OpenCitations to expose all the data of its collections as sets of RDF statements compliant with an ontology named OpenCitations Ontology. In this paper, we introduce oc_ocdm, i.e. a Python library developed for creating OCDM-compliant RDF data even if the programmer has no expertise in Semantic Web technologies. After an introduction of the library and its main characteristics, we show a number of projects within the OpenCitations infrastructure that adopt it as their building block unit.



> "Data models are crucial artifacts that datasets suppliers should make available to document data and to enable users to understand and, thus, use appropriately suppliers’ data. 
> 
> Sometimes, data models may be created (re)using terms defined in the same ontologies with different nuances, thereby generating diversity in data representation. Of course, a data model can employ clearly defined ontological terms to ensure data consistency and facilitate integration tasks. However, even if ambiguities are entirely avoided from a terminological perspective, creating datasets compliant with a particular data model can still be a challenge for people who are not experts in the related technologies, such as OWL and RDF. Further challenges can be due to data dynamics (e.g. extensions and modifications) which must be performed accordingly to the data model either to correct possible mistakes in an entity or to introduce new data. 
> 
> Additional complexities in data handling are introduced when the data model asks for tracking entities’ provenance and changes every time an entity is modified. To enable users (e.g. domain experts) to programmatically access the data organised according to a particular data model and to permit their modifications, **applications (visual interfaces, web editors, etc.) must be developed to facilitate human-data interaction**. **<u>However, an additional interface layer should be provided to permit programmers to develop such applications</u>**, since such programmers are experts in coding but not necessarily skilled in the technologies used by an underlying data model. <u>Such an interface layer would enable creating and manipulating data transparently from the actual technologies used for their representation, such as RDF and, particularly, OWL ontologies</u>. The situation introduced above describes what happened in OpenCitations in the past few years. [...]. A few years ago, OpenCitations released the OpenCitations Data Model (OCDM), a data model based on SPAR Ontologies, PROVO, and other existing models, for describing all the entities in its collections, keeping track of their provenance and modifications in time. In addition of being reused by OpenCitations, the OCDM has also been recently adopted by other external projects dealing with bibliographic metadata and citations. The more the OCDM is adopted, the more it is necessary to have a library to simplify the creation of applications dealing with OCDM-compliant data.
> 
> In this paper, we introduce a Python library, i.e. **oc_ocdm**, for enabling data owners and publishers to develop applications using OCDM-based data and provenance information. This library has already been used by OpenCitations in several components and projects, and it is the building block for all the future applications dealing with RDF data in OpenCitations’ collections."
> (Introduction, pp. 1-2).


![Data model 2022](images/persianiProgrammingInterfaceCreating2022-fig2-datamodel.jpg)


> #### Shape Validation
> When using the methods `import_entities_from_graph` and `import_entity_from_triplestore`, the user can specify to perform shape validation on the imported graph, in order to filter out all the entities that do not respect the shape constraints described in the OCDM (constraints on a given property regarding its range datatype/class, the minimum/maximum amount of attributes associated to an entity, etc.). This operation is currently handled via the `PyShEx` library.
> 
> The shapes described in the OCDM were formalised into a proper ShExC file, that is the required input of `PyShEx`. Such a resource is included within the *oc_ocdm* package. `ShEx` was a design choice that we inherited from the initial phase of the development, which started a few years ago. We chose the ShExC format because of its simplicity and compactness, which makes it easy to be written and read also by non-expert users.
> 
>(p. 8)



>[!important]+ Important!
>
>My task here is to create a new validation procedure using SHACL and pySHACL instead of ShEx and PyShEx, due to the fact that the development of the latter has been gradually slowed down since 2021 and currently has efficency problems. The validation process in SHACL should later be integrated in the broader *oc_ocdm* library.




> In this paper, we have introduced oc ocdm, a Python library for enabling the development of applications using OCDM-based data and provenance information. After showing the main requirements for the development, we have introduced its organisation in terms of Python modules and classes and we have presented its current and future uses in the context of several components and projects related to OpenCitations, being the main building block for all the applications dealing with creating and modifying RDF data in OpenCitations’ collections.
> 
> (Conclusions, p. 15)