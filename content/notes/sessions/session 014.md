---
title: "Session 014"
tags:
- timeline
- session
date: 2022-11-29 20:57
---
<span 
		class='ob-timelines'
		data-date="2022-11-29-00">
</span>

👥 [**Reference meeting: 014**](notes/meetings/meeting%20014.md)

🔙 [**Previous work session: 013**](notes/sessions/session%20013.md)

### Field `author`, `editor` e `publisher`
Ho applicato le correzioni secondo [meeting 013](notes/meetings/meeting%20013.md). Ora le particolarità di `publisher` sono gestite separatamente. Inoltre, è presente una funzione, `orphan_ra_id()`, che individua possibili identificativi al di fuori delle quadre nei campi  `author`, `editor` e `publisher`: viene creato un *warning*.

### Funzione duplicates in META-CSV e correzione della funzione duplicates per CITS-CSV
Ho creato la funzione per individuare le righe duplicate all'interno delle tabelle di Meta e ho aggiunto la chiamata e tutto il resto all'interno della funzione principale. Dopo una prima fase, ho individuato dei bugs, che erano presenti anche nella funzione per CITS-CSV: corretto entrambe le funzioni.

### Field `venue`, `volume`, `issue`, `page` e `type`
Ho creato le funzioni per validare il formato OC per i campi `venue`, `volume`, `issue`, `page` e `type`, le cui chiamata e creazione del report sono già aggiunte nella funzione principale. Funzionano, ma ci sono probabilmente delle modifiche/aggiunte da fare, che discuterò in [meeting 014](notes/meetings/meeting%20014.md), relative a quanto devono essere restrittive le regex → cosa considerare errore nei field `venue`, `volume` e `page`?
Per ora, funziona così:
* per `venue` e `volume`: l'unica cosa che controllo è che non ci siano spazi all'inizio o alla fine della stringa, e che non ci siano doppi spazi in mezzo
* per `page`: ho deciso di ammettere due formati, quello con le cifre e quello con i numeri romani (ma qui la sintassi di META-CSV non è chiara). Devono sempre esserci due numeri (espressi entrambi nello stesso formato, cioè cifre XOR numeri romani) separati da un trattino (cioè mai un numero solo, nemmeno nell'ipotesi che la risorsa descritta non sia più lunga di una pagina → questo potrebbe essere un punto da cambiare, se è compatibile con il software che processa Meta...). Per ora, sono ammessi anche intervalli impossibili (in cui la pagina di fine è minore della pagina di inizio, es. 20-15)[^1] e numeri romani 'impossibili' (es. VD), in mancanza di una sintassi universale e precisa per questo tipo di numerazione. 

### Ipotesi di dover rivedere i caratteri legali nelle stringhe dei nomi di persona
Vedi domande in [meeting 014](notes/meetings/meeting%20014.md)

### Meta required fields
Ho creato la funzione per controllare che tutti i fields obbligatori, in base agli altri field, siano presenti in ciascuna riga. Ci sono sicuramente delle modifiche da fare, discusse in [meeting 014](notes/meetings/meeting%20014.md), perché la definizione della sintassi di META-CSV contiene delle incoerenze e dei punti poco chiari per quanto riguarda i campi obbligatori e i valori richiesti per il field `type` in determinati casi vedi [meeting 014](notes/meetings/meeting%20014.md).


## N.B.: Quasi finito il primo livello!
Quando saranno fatte le necessarie correzioni/integrazioni, il primo livello, quello relativo al formato di OC, dovrebbe essere concluso (rimarrà soltanto da definire più chiaramente i messaggi).



[^1]: Non controllo questa cosa perché non credo sia possibile farlo con le regex, e fare controlli sulle singole parti dell'intervallo mi sembrava un po' un overkill... 