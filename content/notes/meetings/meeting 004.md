---
title: "Meeting 004"
tags:
- timeline
- meeting
date: 2022-09-17 18:59
---
<span 
		class="ob-timelines"
		data-date="2022-09-17-00">
</span>
📑 [**Reference session: 004**](notes/sessions/session%20004.md)

🔙 [**Previous meeting: 003**](notes/meetings/meeting%20003.md)


## **Q&A**
### .is_valid come primo step di validazione sintattica e semantica per gli ID
*Una volta che il primo livello della validazione è stato passato (formato dati OC), ha senso usare direttamente `.is_valid`? Se lo passa, significa che livello sintattico e (gran parte del) livello semantico sono passati, e i dati possono essere ammessi. Se non lo passa, si possono sottoporre  i dati a tutti i controlli sintattici e semantici uno alla volta.*


### Cosa considerare come ID valido per wikidata e wikipedia?
* **Page ID** è facile da controllare e va bene sia per wikidata che per wikipedia a livello univocità, ma non mi sembra che abbia troppo senso dal punto di vista semantico per le entità di Wikidata
* **Page title** è semanticamente adatto per le entità di wikidata (perché consiste nel Q-ID e rappresenta l'entità stessa), ma sembra un po' "vago" per le pagine di Wikipedia, o perlomeno prono ad errori.

Possibili soluzioni:[^1]
1. Page title sia per gli input id prefissati da `wikidata:` (Q-ID, cioè così come è ora) che per quelli prefissati da `wikipedia:`. 
2. Page ID sia per `wikidata:` che per `wikipedia:`.
3. *Solo* Page Title per `wikipedia:`; Page Title *o* Page ID (Q-ID) per `wikidata:` nell'input dell'utente, solo uno dei due nell'output della validazione. 
4. *Solo* Page ID (numero intero positivo) per `wikipedia:`; Page Title *o* Page ID (Q-ID) per `wikidata:` nell'input dell'utente, solo uno dei due nell'output della validazione. .

[^1]: Scarto la possibilità di ammettere entrambi i tipi di id per Wikipedia perché non sono automaticamente distinguibili. Inoltre, non è ammissibile avere due possibili identificativi per lo stesso prefisso all'interno di OC (la stessa risorsa, se è rappresentata con due identificativi diversi per lo stesso prefisso, es. `wikidata:`, risulta come due risorse diverse all'interno degli indici). È comunque possibile lasciare che l'utente mandi l'identificativo che vuole, per poi cambiarlo noi nel tipo di identificativo che ammettiamo noi, in seguito alla richiesta all'API.

### È necessario ammettere che l'utente invii come ID per wikidata o wikipedia l'URL completo?
Es. "wikipedia://en.wikipedia.org/w/index.php?curid=8091"  

### .get_extra_info



# Incontro 
### Arcangelo
Affinata la proposta di automatizzare il processo di ricezione dei dati per la validazione (con GitHub issues)

### TO DO
* manda mail a Simone Persiani e metti in Silvio in cc: dobbiamo sapere che cosa è stato usato come ID delle pagine di Wikipedia nel software di wcw. Usare il Title Page (invece che il Page ID) è la soluzione che preferiamo per ora, ma resta da capire del tutto come funziona: i redirect? come vengono gestiti gli spazi? etc.
* il controllo sintattico va ASSOLUTAMENTE  separato dagli altri e fatto passare prima di tutti gli altri metodo dell'ID Manager. Un buon worflow potrebbe essere:
  1. Controllo sul formato OC
  2. Controllo sintattico sul dato COSì COME L'HA INVIATO L'UTENTE (<u>**senza normalise**</u>). **Se non lo passa, l'*utente* deve correggerlo**.
  3. <u>Chiamare direttamente .is_valid</u>: avendo già passato il controllo sintattico, il normalise cambierà soltanto delle cose minime, poi ri-controllerà la sintassi sul dato normalizzato, e poi ritornerà le info semantiche. **Se non passa .is_valid, è  sicuramente perché "non ha passato" .exist, dal momento che se ha già passato il controllo sintattico sul dato non normalizzato, non può non passarlo su quello normalizzato.** 
  * in generale Silvio è dell'idea che sia meglio lasciare che siano *gli utenti* a prendere delle decisioni, e quindi andarci cauti con i suggerimenti rispetto a potenziali correzioni. Sicuramente tu non devi cambiare niente! (per questo il normalise va usato soltanto dopo che il dato ha passato il controllo sintattico!).