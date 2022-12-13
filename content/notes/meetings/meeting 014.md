---
title: "Meeting 014"
tags:
- timeline
- meeting
date: 2022-11-29 20:56
---
<span 
		class="ob-timelines"
		data-date="2022-11-29-00">
</span>
📑 [**Reference session: 014**](notes/sessions/session%20014.md)

🔙 [**Previous meeting: 013**](notes/meetings/meeting%20013.md)

### Quasi finito il primo livello di validazione
Ora sono gestiti anche: 
* field `venue`, `volume`, `issue`, `page` e `type`
* duplicate rows
* required fields

## Domande

### Caratteri legali nelle stringhe dei nomi di persona
Quali caratteri sono legali nella stringa di un nome (a parte i separatori) per le *people*? In particolare: cifre? segni di punteggiatura che non siano il trattino o l'apostrofo? parentesi? ... Considera la possibilità di avere [nomi anagrafici particolarmente strani](https://www.wikidata.org/wiki/Q93418989), degli alias o dei nomi d'arte, ecc.

<u>**Risposta**</u>:
Nessun controllo ulteriore rispetto a quelli già in atto. 

### Caratteri legali e/o obbligatori nei field `volume` e `venue`
Che controlli devo fare per quanto riguarda il formato dei field 'volume' e 'venue'? → per ora l'unica cosa che controllo è che non ci siano spazi extra all'inizio, alla fine o in mezzo.

<u>**Risposta**</u>:
Nessun controllo ulteriore rispetto a quelli già in atto. 

### Caratteri legali per field `page`
Che controlli devo fare per il field 'page'? Soprattutto:
* È possibile avere una sola pagina (senza intervallo e quindi senza trattino)? Come esprimo l'intervallo di `page` per una risorsa che  non si estende per più di una pagina? Es. "25-25" sarebbe corretto?
* È possibile avere numeri romani (come per esempio per i capitoli introduttivi)? Per ora li gestisco, ma non tengo presente i casi in cui vengano scritti numeri romani "sintatticamente" impossibili, in mancanza di una definizione universale: per esempio, risulta valido anche "VD".
* È necessario gestire gli intervalli "impossibili", tipo "20-15"? Se possibile farlo con le regex, penso sia un pattern piuttosto lungo, e d'altro canto trattare le singole parti del field separatamente mi è sembrato un po' un overkill...

<u>**Risposta**</u>:
* Anche per risorse su una pagina sola, la maniera corretta di esprimere è sempre l'intervallo con il trattino, quindi ad esempio "25-25"*
* Ci sono anche pagine alfanumeriche, es. "12a-20a", quindi la regex deve essere ampliata per includere anche quelle
* Gli intervalli impossibili sono un errore, o per lo meno un warning: devi controllarli, è rilevante; un workflow sensato sarebbe, **se possibile**, convertire i caratteri non numerici (tipo numeri romani) in integers (anche tranquillamente con una libreria); poi, se la conversione è riuscita, controllare che il primo carattere sia minore del secondo. Anche se non è possibile farlo per tutti i tipi di numero di pagina (ad esempio i caratteri alfanumerici rimarrebbero fuori) vale comunque la pena farlo per quelli per cui si riesce. 

### Required fields per le righe che hanno specificato un valore in `volume` e/o `issue`:
Nel caso in cui nel campo 'id' non sia specificato alcun valore, si applicano almeno gli stessi requirement che nel caso in cui esso sia specificato, e *<u>in aggiunta</u>* quelli che si applicano nel caso in cui non sia specificato (in base al tipo)? Ovvero, se non c'è nessun ID:
* devono essere specificati 'venue', 'volume' e/o 'issue' e inoltre il type deve essere uno tra 'journal article', 'journal volume' e 'journal issue'
* in aggiunta si applicano gli eventuali requirement applicati nei casi in cui non sia presente l'id? (di base, solo per 'journal article' che in questo caso vuole anche `title`, `pub_date`, `author` e/o `editor`)

**È effettivamente così?**

https://github.com/opencitations/metadata/blob/master/documentation/csv_documentation.pdf

![mandatory_fields](images/mandatory_fields.png)

![required_fields_table_updated](images/required_fields_table_updated.png)


<u>**Risposta**</u>:


con id e issue è oblgoato venue e basta e il tippo deve essere jounral article or journal issue (non journla volume)


id e volume: tipo deve essere  jounral ariticle, journla  voluem o journal  issue; il campo venue è obbligatorio

		se il tipo è jounral issue allora è obbligatorio title OR issue


Per quanto riguarda le restrizioni/requirements derivanti dalla presenza di un valore per i campi `volume` e/o `issue`, il fatto che `id` sia specificato è irrilevante. 

**Se specifico come tipo `journal volume` allora non posso specificare la `issue`, a prescindere dal fatto che sia specificato `id` oppure no**.

Quindi, indipendentemente dal fatto che sia specificato l'`id` oppure no, se un valore è specificato per `volume` or `issue`, le regole sono le seguenti:
*  è sempre obbligatorio specificare la `venue` 
* è sempre obbligatorio specificare il type, ma con un valore specifico che dipende da specifiche condizioni, spiegate qui sotto
	* se `issue` è specificato → `type` == (`journal article`|`journal issue`)
	* se `volume` è specificato →  `type` == (`journal article`|`journal issue`|`journal volume`)
{* se  `type` == `journal issue` → è obbligatorio specificare (`title`|`issue`)}


