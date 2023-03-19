---
title: "Meeting 006"
tags:
- timeline
- meeting
date: 2022-10-06 04:17
---
<span 
		class="ob-timelines"
		data-date="2022-10-06-00">
</span>
📑 [**Reference session: 006**](notes/sessions/session%20006.md)

🔙 [**Previous meeting: 005**](notes/meetings/meeting%20005.md)

- Metti etichette per identificare livelli di validazione, invece che numeri (fai in modo che sia più chiaro)

- Il valore di field deve essere sempre lista che contiene 0 o più componenti e ognuno dei componenti è descrittivo 


- Output dev'essere descrittivo, deve dire tutto tutto, compresa la spiegazione dell'errore (non puoi fare elaborazioni successivamente, dev'essere già tutto pronto nel report).


- Ricorda flag per mettere sia dei dati in senso attivo, quindi rendere esplicito anche che un dato è risultato valido, oppure no. In ogni caso, vedi tu se riportare sia le stringhe dei dati che la posizione nel report.

- JSON schema per definire report!


- Metti INDICE per la posizione degli elementi all'interno di un field, non (o meglio sicuramente non SOLO) il valore della stringa


- Controlla direttamente i singoli elementi all'interno del field 'id' sul controllo del formato OC, invece che chiamare prima il controllo sulla cella: non ha senso, chiami la stessa cosa due volte(?)

- Silvio: un validatore non dice "forse", un validatore dice "sì" o "no". Lui sarebbe strict nel validare. Bisogna pensare se ha senso tenere in considerazione warning, nel caso in cui individuassimo dei casi in cui ci viene da dire che warning sarebbe proprio perfetto. Un esempio di warning, in quest'ottica, potrebbe essere per un titolo che venga inserito con tutte le lettere maiuscole: non è sbagliato rispetto alla sintassi della tabella, quindi non è un errore, tuttavia è un po' strano --> warning

- Validazione severa! e valuta se ha senso avere anche degli warning oppure no. DEVI VALIDARE RISPETTO ALLA DEFINIZIONE DELLA SINTASSI, NON A QUELLO CHE FA IL SOFTWARE DI OPEN CITATIONS. NON C'è SOLO META, C'è ANCHE CROCI. Non pensare a quello che Meta gestisce o no, considera valido solo quello che è definito valido nella sintassi delle tabelle di CITS-CSV e META-CSV. 


- TITOLO MAIUSCOLO --> WARNING NON ERROR, se vogliamo tenere warning

- Nel caso di spazi multipli, ad esempio, tra un ID e l'altro all'interno del field 'id', devi decidere se considerarlo errore (bloccante) o no in base alla definizione della tabella; non importa se il software di OC lo gestisce, se è sbagliato è sbagliato. Devi coordinarti con chi è responsabile della sintassi della tabelle di META-CSV e CITS-CSV.


 



