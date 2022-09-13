---
title: "Meeting 002"
tags:
- timeline
- meeting
date: 2022-09-06 03:46
---
<span 
		class="ob-timelines"
		data-date="2022-09-06-00">
</span>
📑 [**Reference session: 002**](notes/sessions/session%20002.md)

🔙 [**Previous meeting: 001**](notes/meetings/meeting%20001.md)



## **Q&A**
### ***Q1: importare packages:***
*Perché devo aprire direttamente index e non oc che è la cartella che lo contiene?
E c'è un modo per importare moduli collocati in una directory con un percorso diverso da quella in cui sto lavorando? ovvero,  devo per forza collocare il file in cui sto lavorando all'interno di oc/index per poter importare i moduli?*

*esiste una versione aggiornata/definitiva e installabile del software/libreria per gli indici?
è possibile installare questo software in un percorso tale per cui posso importare le sue parti da qualunque posizione nel mio pc?*

### ***Q2:  isbn manager***
*Esiste già un isbn manager? È il caso di crearlo? O meglio, il fatto che non sia presente nel [pacchetto](https://github.com/opencitations/index/tree/master/index/identifier)  allo stato attuale significa che OC non lo gestisce o non lo gestirà?*



## **To do**
* [ ] vedere dataset diversi (CROCI)
* [ ] (introdurre distinzione su tipo di tabella)
* [ ] proseguire su verifica id: isbn manager?
* [ ] introdurre/migliorare condizioni per chiamare funzioni di identifier manager




* quanti e  quali id gestiamo su meta? 
	vedi meta di arcangelo per capire quali 
 isbn, doi, orcid, isnn



#### Tipi di ID gestiti in meta:

* **Bibliographic resources (br)**:
	* 'doi', 'issn', 'isbn', 'pmid', 'pmcid', 'url', 'wikidata', 'wikipedia'


* **Responsible Agents (ra)** (authors, editors, publishers):
	* 'crossref', 'orcid', 'viaf', 'wikidata', 'ror'[^1] 

[^1]: 'ror' might be introduced in the meawhile, since it is becoming very common. UPDATE: i think it has been [introduced by Arcangelo on Sept. 06](https://github.com/opencitations/oc_meta/commit/80b6fd41d7ac679c88fafd1580e03829be8860fe).

Dovrai forse creare classi aggiuntive per fare i check degli identificativi per cui le classi non esistono già su meta o su index. 
Ma prima devono decidere quali sono le classi definitive per checkare gli id e poi mettere tutto su un unico repo. devono anche fare in modo che index sia installabile con pip. meta è già installabile con pip. 



Ricordati: 
* togliere il "10" dopo il prefisso del doi nel match con la regex, perché si tratta già, concettualmente, di un altro tipo di validazione
* è da introdurre uno step in cui si fa un **minimo** di pulitura della cella, **indipendentemente dal tipo di id"**, (ad esempio si tolgono gli spazi  e i doppi spazi prima dell'id), per poi fare la validazione sintattica e poi ancora semantica su una stringa che ti consenta di scegliere il processo giusto di validazione, ad es. matchando la regex che hai già nel codice. È comunque opportuno segnalare "errori" o anomalie nel formato già in questa fase di pre-processing molto basilare (es. "Qui c'è uno spazio, ma non dovrebbe esserci").
* ok che fornisci già ora il feedback, ma devi pensare a localizzarlo non già ora in funzione del feedback, ma in un modo che renda l'istanza più retrievable possibile all'interno degli oggetti che hai salvato (andrebbe bene una specie di dizionionario o lista di dizionario che mappi gli errori, il tipo di errore, la posizione...)