---
title: "Session 005"
tags:
- timeline
- session
date: 2022-09-28 02:51
---
<span 
		class='ob-timelines'
		data-date="2022-09-28-00">
</span>

👥 [**Reference meeting: 005**](notes/meetings/meeting%20005.md)

🔙 [**Previous work session: 004**](notes/sessions/session%20004.md)

> [!info]+ My goals for this session
> 
> The description of your goal goes here



### Redirects e Disambiguation
##### Redirect
vedi https://www.mediawiki.org/wiki/Help:Redirects#Viewing_a_redirect (fondamentale per capire i redirect). soprattutto tieni presente che "**When a page is [moved](https://www.mediawiki.org/wiki/Special:MyLanguage/Help:Moving_a_page "Special:MyLanguage/Help:Moving a page"), a redirect from the old to the new pagename is automatically created**."!


## Soluzione mega importante!
Questa richiesta sembra fornire le informazioni di cui abbiamo bisogno per capire se la/ pagina/e che cerco sono redirect e/o disambiguation: https://en.wikipedia.org/w/api.php?action=query&format=json&prop=redirects%7Cpageprops&titles=Elizabeth%20II%7CQueen%20Elizabeth%7CPrince%20Charles%7CCharles%20III&redirects=1&rdprop=pageid%7Ctitle&ppprop=disambiguation. Qui ho praticamente cercato tutte insieme delle pagine "normali" (Elizabeth II e Charles III), delle pagine di disambiguazione (Queen Elizabeth), delle pagine di redirect (Prince Charles). C'è un parametro che in teoria non serve, ovvero `prop=redirects` (che elenca le pagine *che reindirizzano* alla pagina che si sta cercando), mentre invece va lasciato il parametro `redirects` (il cui valore è un booleano, quindi se viene specificato nella richiesta ha valore True, indipendentemente da il valore effettivamente inserito; se invece si vuole che `redirects` sia false di deve semplicemente NON metterlo nella richiesta!), che in teoria dice a quale pagina la pagina che sto cercando reindirizza (quindi se nella risposta non c'è niente, ovvero nessuna voce `"redirects": {...}`, allora significa che quella pagina **non** è una pagina di reindirizzamento). Lo stesso vale per `ppprop=disambiguation`: se la pagina che cerco è una pagina di disambiguazione nella risposta ci sarà una coppia key-value `"disambiguation": ""`, se non lo è allora non ci sarà niente. 

Questa è la risposta in json della richiesta menzionata sopra:
+ [sandbox con i parametri già inseriti nell'interfaccia](https://en.wikipedia.org/wiki/Special:ApiSandbox#action=query&format=json&prop=redirects%7Cpageprops&titles=Elizabeth%20II%7CQueen%20Elizabeth%7CPrince%20Charles%7CCharles%20III&redirects=1&rdprop=pageid%7Ctitle&ppprop=disambiguation)
+ url (richiesta) generato dalla sandbox: https://en.wikipedia.org/w/api.php?action=query&format=json&prop=redirects%7Cpageprops&titles=Elizabeth%20II%7CQueen%20Elizabeth%7CPrince%20Charles%7CCharles%20III&redirects=1&rdprop=pageid%7Ctitle&ppprop=disambiguation
+ risposta alla richiesta (sempre generata dalla sandbox):
```json
{
    "continue": {
        "rdcontinue": "Charles_III|1720180",
        "continue": "||pageprops"
    },
    "query": {
        "redirects": [
            {
                "from": "Prince Charles",
                "to": "Charles III"
            }
        ],
        "pages": {
            "125248": {
                "pageid": 125248,
                "ns": 0,
                "title": "Charles III",
                "redirects": [
                    {
                        "pageid": 160475,
                        "ns": 0,
                        "title": "Charles Windsor, Prince of Wales"
                    },
                    {
                        "pageid": 247754,
                        "ns": 0,
                        "title": "Charles Philip Arthur George Windsor-Mountbatten"
                    },
                    {
                        "pageid": 247755,
                        "ns": 0,
                        "title": "Charles Philip Arthur George Mountbatten-Windsor"
                    },
                    {
                        "pageid": 411507,
                        "ns": 0,
                        "title": "Charles, the Prince of Wales"
                    },
                    {
                        "pageid": 672479,
                        "ns": 0,
                        "title": "Charles, prince of Wales"
                    },
                    {
                        "pageid": 962107,
                        "ns": 0,
                        "title": "HRH Prince Charles"
                    },
                    {
                        "pageid": 1145872,
                        "ns": 0,
                        "title": "Charles Windsor"
                    },
                    {
                        "pageid": 1238910,
                        "ns": 0,
                        "title": "Charles, prince of wales"
                    },
                    {
                        "pageid": 1449884,
                        "ns": 0,
                        "title": "Charles Mountbatten-Windsor"
                    },
                    {
                        "pageid": 1648561,
                        "ns": 0,
                        "title": "Prince Charles of Wales"
                    }
                ]
            },
            "12153654": {
                "pageid": 12153654,
                "ns": 0,
                "title": "Elizabeth II"
            },
            "165355": {
                "pageid": 165355,
                "ns": 0,
                "title": "Queen Elizabeth",
                "pageprops": {
                    "disambiguation": ""
                }
            }
        }
    }
}
```


# Raffiniamo la richiesta
Togliamo il valore redirects al parametro prop, e lasciamo soltanto redirects *come parametro* (oltre agli altri parametri e valori, ovviamente).
Concentriamoci sugli effetti del parametro `redirects`.
**Questa è la risposta alla richiesta con il parametro `redirects`**, ovvero: https://en.wikipedia.org/w/api.php?action=query&format=json&prop=pageprops&titles=Elizabeth%20II%7CQueen%20Elizabeth%7CPrince%20Charles%7CCharles%20III&redirects=1&ppprop=disambiguation

```json
{
    "batchcomplete": "",
    "query": {
        "redirects": [
            {
                "from": "Prince Charles",
                "to": "Charles III"
            }
        ],
        "pages": {
            "125248": {
                "pageid": 125248,
                "ns": 0,
                "title": "Charles III"
            },
            "12153654": {
                "pageid": 12153654,
                "ns": 0,
                "title": "Elizabeth II"
            },
            "165355": {
                "pageid": 165355,
                "ns": 0,
                "title": "Queen Elizabeth",
                "pageprops": {
                    "disambiguation": ""
                }
            }
        }
    }
}
```

Fai attenzione soprattuto al fatto che della pagina "Prince Charles", avendo io specificato il parametro `redirects`, l'API non restituisce i dati relativi alla pagina di reindirizzamento (cioè quella con nome "Prince Charles"), ma soltanto quelli relativi alla pagina a cui mi ha automaticamente reindirizzato. Questo, però, è il solo modo che abbia trovato finora per segnalare che la pagina che sto cercando non è una pagina "normale", ma una pagina di reindirizzamento. Qui sotto la risposta alla richiesta formulata senza specificare il parametro `redirects`: della pagina con titolo Prince Charles (che è una pagina di disambiguazione) vengono restituiti gli stessi dati che per le pagine normali!!
**richiesta**: https://en.wikipedia.org/w/api.php?action=query&format=json&prop=pageprops&titles=Elizabeth%20II%7CQueen%20Elizabeth%7CPrince%20Charles%7CCharles%20III&ppprop=disambiguation

**risposta**:

```json
{
    "batchcomplete": "",
    "query": {
        "pages": {
            "125248": {
                "pageid": 125248,
                "ns": 0,
                "title": "Charles III"
            },
            "12153654": {
                "pageid": 12153654,
                "ns": 0,
                "title": "Elizabeth II"
            },
            "20756878": {
                "pageid": 20756878,
                "ns": 0,
                "title": "Prince Charles"
            },
            "165355": {
                "pageid": 165355,
                "ns": 0,
                "title": "Queen Elizabeth",
                "pageprops": {
                    "disambiguation": ""
                }
            }
        }
    }
}
```


### Domande
1. Cosa succede se cerco un titolo che prima era quello di articolo normale, ma poi, essendo stato la pagina *spostata* (ovvero essendo stato cambiato il titolo dell'articolo, che pur mantenendo lo stesso page id ha assunto un titolo diverso), è diventato il titolo di una pagina di reindirizzamento? Ad esempio, "Charles, Prince of Wales" probabilmente era il titolo dell'articolo nel 2003, ma ora è il titolo di una pagina di reindirizzamento (con un suo proprio pag id!): vedi la schermata informativa che appare se specifichi redirects=no nell'URL adatto con il browser: https://en.wikipedia.org/w/index.php?title=Charles,_Prince%20of%20Wales&redirect=no. ![redirect_page](images/redirect_page.jpg).
	In un caso come questo possiamo escludere che il dato inviato dall'utente venga considerato buono, solo perché la pagina reindirizza a qualcos'altro, quando magari era effettivamente il titolo della pagina che lui aveva consultato, al tempo in cui l'ha consultata? D'altro canto, non c'è un altro modo di sapere se la pagina è un reindirizzamento o no, da quanto ne so. **NB: questo problema non si porrebbe con i page id, che rimangono fissi per tutta la vita della pagina, anche quanto essa viene spostata (cioè le viene cambiato il titolo)**

2. Le pagine di reindirizzamento possono reindirizzare a pagine inesistenti o, immagino, a pagine di disambiguzione.
3. Può il titolo di una pagina normale divenire, una volta spostata la pagina, il titolo di una pagina di disambiguazione?

