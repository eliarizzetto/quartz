---
title: "Meeting 005"
tags:
- timeline
- meeting
date: 2022-09-29 02:50
---
<span 
		class="ob-timelines"
		data-date="2022-09-29-00">
</span>
📑 [**Reference session: 005**](notes/sessions/session%20005.md)

🔙 [**Previous meeting: 004**](notes/meetings/meeting%20004.md)


### Wikidata Manager
1. **Mail con Simone Persiani**:
	> Nel progetto “Wikipedia Citations in Wikidata” rappresentavo le pagine di Wikipedia EN come BibliographicResource con identificativo “wikipedia:\<PAGE-ID>” e con titolo “\<PAGE-TITLE>”.  
	> Per seguire il tuo esempio, la pagina su Tim Berners-Lee verrebbe mappata in un’entità con :hasTitle “Tim Berners-Lee” a cui sarebbe associata un’entità di tipo Identifier con :hasLiteralValue “30034” e :usesIdentifierScheme datacite:wikipedia
	
	> Se vi dovete riferire alle pagine di Wikipedia intese come risorse bibliografiche, <u>vi conviene usare i page id come identificativi (i titoli delle pagine di Wikipedia possono essere modificati, mentre il page id rimane sempre lo stesso</u>!!!).

2. **Redirect e disambiguation**:
	* Ho capito quali parametri usare nella richiesta all'API per sapere se la pagina cercata è un articolo vero e proprio o se invece è una pagina di reindirizzamento e/o di disambiguazione (in un'unica richiesta). <u>Resta da vedere se è il caso di escludere tutte le pagine che risultano essere redirect o disambiguation al momento della richiesta</u>, visto che un articolo potrebbe diventare una di queste due cose in seguito ad uno spostamento (cioè a un cambio di titolo); ad esempio "Charles, Prince of Wales" è stato il titolo dell'*articolo* che ha page id "125248" (https://en.wikipedia.org/wiki/Charles_III); ora che Carlo è re, però, il titolo della pagina è cambiato e "Charles, Prince of Wales" è *automaticamente* diventato il titolo di una pagina di reindirizzamento (con un suo proprio e differente page id!).

### Flowchart
 <iframe frameborder="0" style="width:100%;height:1213px;" src="https://viewer.diagrams.net/?highlight=0000ff&edit=_blank&layers=1&nav=1&title=validation_flowchart.drawio#R%3Cmxfile%20pages%3D%222%22%3E%3Cdiagram%20id%3D%22C5RBs43oDa-KdzZeNtuy%22%20name%3D%22General_structure%22%3E7Vvbkto4EP0aqnYfhvLd8DgDk%2BtkEzK3ZF9SwgjwRrYcWdzy9SvZEraRGRzAhlCZl7Hakmx19zndUpuW2QuWrwmIph%2FwCKKWoY2WLbPfMgy9Y%2BjsH5esUonlaKlgQvyR6JQJ7v2fUAhlt5k%2FgnGhI8UYUT8qCj0chtCjBRkgBC%2BK3cYYFZ8agQlUBPceQKr02R%2FRaSrtGG4mfwP9yVQ%2BWXe66Z0AyM5iJfEUjPAiJzJvW2aPYEzTq2DZg4grT%2Brl%2Be3qGd19d16%2FG8Q%2FwOPN%2B4d%2Fnq7SyV79ypD1EggM6d5TPz6%2BM%2BaD%2BPHzwO2%2BscPe28Hz4MqUa6MrqTA4YvoTTUzoFE9wCNBtJr0heBaOIJ9WY62szx3GERPqTPgfpHQlnAHMKGaiKQ2QuFtxPWLdMZ4RD76wCF1YjAIygfSFjrwX78iXmPMOoa%2FXEAeQkhXrQCAC1J8XXQgIT5ys%2B2XaZhdC4b%2BgfPnec4Bm4lH3D9efH1STIMTgwlW%2FmPoU3kcg0ceCIfYQxc4hoXD5oiLE3Su7o7V1Kx0miMAWrrPIUKVrQjbNIUr2O0R9tze3PwYjbxk9Lf991zffDxaReXUJris9crfr2mfluvK9c67rh9GMMlHbi%2BecqH2m8E0DMQqN%2BCXTH0AIIjwhIGBajSDx2YtBsnnvU3Zjl%2B%2BP%2FSWU8adeLHQVLOhWCRiMEjA4RwBDOZHrirIbQANc%2BvQLH962Retr7k5%2FKWZOGivRODaC7IoIMo9O%2FmLoJ%2ByHNO8fpuIfprth9%2FRtxcgN069f5QB42go8%2BxxFgR9CnssBCpJcirAkR%2FGbolfsQh0O6SsQ%2BIiv9A1Ec0h9D4gbEo2GaPcwwiR5iKklf0wOkD8Jmcxj7pDAPKYEf4dlXRmloJx8nPw1HPWMMqTXFfbK%2Fdi8gLgnwbgbtW5DqHW69qatrU22rhm1pqWg9sPtw%2FVV7%2F6p1OR3YMg2awUzKWji3s4Aia7FjcAfjVKPgLH%2FEwyT%2BbhPRHxVyTrtm5bdLzX5i%2F6o4Gq9pxNPaeW3TaV4Y2jTtG4xsqatfa0ru%2BDxOIb1GE2l2t7bh%2FutRrtUnDoN4dQ0FE62jIZxqia%2Fz1PfmzLRx16iLO7w6%2FhqvlIcQaIyQfAnHPvUxxydQ0wpDkpgS%2FFG3JWZdLCc8HOb9hjhhTcFhLbjWcDC%2FOTbeBZ66bQXHqRtp20WScM01SBtG227wSjt2JeAfqci%2Bm3r2Og%2FLEVyFIC2DAexFdwM2cWEXzwxn2Z5MAeIod3BeXLsyV5I%2B8vzaSKP%2F26Z13Ice4%2F10PVcREo8HETIB6EHE6ejnAqUJ2ZhoWzKbdn4NqpYB%2FKtEb6ELSKCPRjHW9L6ovvFqZNpba1jFv84IRBPOKGTTp%2BejGoNk016R57r6i%2FQzxQT%2Fyd7IJAqQRv6XD%2BN8Q7t%2BwQK8uwjSuqkL6sknjklewyrZI%2FRqY29nEtgL7cqe2nnxV7uIezF3gEcmbiyTcgf4vpDXJK4XOcMicu6hKKAWbWeJUuVpzjS7HQb3nSpxbKXWREJVjS2kyEIOHuEwzhKDqkZjJjVuYVWDG9LxZOqkVHOL4bA%2Bz5JvOvjjCJ%2B9lpzacLRFDu5p8ekcQGYtOS3C7swaZmNYVI5sNQVI9YMSqmVXwalWRGU3DQwACGLxPFl4FHXjJMD0tzTalZFq5VllR97%2FQ8XYkG9c2oL2p1TUCrTLVl9EeOTxtfWuvbLm1nBN2nJiu%2B6StzKasRZxbiRKrHc4p2g3nSYodWsJ8Sltj%2BPYpB0zKMUgzTLFqnD%2BZZ%2FHDUCrqAaqn6%2FhKcyYpzGEh5XoWKn4cqPVErO2hGIY25wESRLij1kioPhrEJwK7pAbSHNVTdzdseV1ZFcTOuUhLTaPmCy1TrqFqZrKsrp%2B0U5%2FYRRzqoa5Rqr1qq%2B5mzW3LZg9poQsMp1EzFp%2B7MM9UDK0Tv5KSuNsbXCd9TsIn2T44YN41LDRlUXbC5sqBl842FD%2FbDn9wsbpvrZ6xmEDUdV7UmDRi5OfC2EifKgcWT0SVDtDgBHPzk%2BzIplJxIbVqz08UsA2cK%2FYfItppgkvwwqRc85fBBT9LmtVZ39y2i1MYFtu21ng1GtMiawy764sWvyIVfNzS8jpMoqz%2B6jZ%2F2sQO2qOc7vldGfyo7HJ%2Bf9EmpLzZy6xsaPDCuM6TSSUKsHZTzFOkqCVR%2BRqicanZKz5bKEao%2BjZdbMfqya6j37ya95%2Bz8%3D%3C%2Fdiagram%3E%3Cdiagram%20id%3D%22xzgZwGghKsRzE2Puvifx%22%20name%3D%22Level1_OC_table_format%22%3E5Vvbcts2EP0azbQP1vAu6tHxpXloOmmcNklfMjAJUWggggYhS8zXFyAA8QK6pm2RSmjP2MaNALgHu3t2Sc7ci83%2BNwqy9TsSQzxzrHg%2Fcy9njrNcevyvaChkgxdYsiGhKJZNdtVwg75D1aiHbVEM88ZARghmKGs2RiRNYcQabYBSsmsOWxHcXDUDCTQabiKAzdZPKGZr2Ro6i6r9LUTJWq9sB0vZswF6sLqTfA1isqs1uVcz94ISwmRps7%2BAWMhOy2Wb5O%2FudukXQu6KRYo%2F7q4%2F%2B2dysuunXHK4BQpT9uyp88XX%2BNNf3%2F%2B8uPvnw9876wJ%2Fw5me%2Bh7grZLXzcfzDx%2FVDbNCSzFnlHw7CJDf%2Bps122BetHmRCyYT4zb7RByh%2BQqTXbQGlM1zxv9%2BFWN2a8TgTQYiMXDHh%2FE2tTakDO5bYD1yp%2FZB%2FPzYQrKBjBb8OjWLpwArmtVdBb8%2Bw%2Bsa8noYUAcuOcxbCZUXlFyfIGPXkDFKsy0TZz6%2FnzkB5ht4c0t5KRElBm651A0AlIy5aADGEBMu6k0LBgOjDFLE7wHS9oXvq45OZCjZpjGM1cSARkqvbTFrLsvW3HEHBNH2myi6i7lv4GhbXUD6AwHpGUAaMMGYGx9VJZStSUJSgK%2Bq1ppoLV6rxvxOSKbk%2FS9krFASB1tGmjDLNcVCzxA33yzZ0gj%2Bz11qIw1oAtljx9qEj0IMGLpvbu7oUNjhFEXv9xS9vTil7H1DDSgEMW%2BZl%2FbMWiFhv8S6%2FA%2F%2FxSgXxo6sBItAEUNc5hRxTtBGsGl1uuxSDQtwmxO8ZfC8Mk5NU%2BV1msShzJUTtsxVh7FyOoyVP5TXCV6FsXJ7aox%2FSoUJDSh4EcWAwVJbqFCP6xTuWVmFONbawtZihGYL%2FAIwKZ0J2i7%2B1Dpjmx4%2BJVNUm2VfR%2FNSvVGXvicoZRXwfstY2sGB3OlJ5NbUdS1YDxt5PtKOGfgUHR7pFUEt%2FcPRoV7YTaidhTUq0EsD5wzkuYDaUkaY0xED92eGuzGMUC7mGzfeDSr10QrlmKY09E1LGg4VKmnUJ04%2Fwp7KtTwl%2FTjkr0YVPRcwLT6r68vKF1HhB1VVL%2Ff1zsuiXqvlJ8rGoXG0%2B0ZeA1lJv5Wycq1%2BVvKcUlDUhmViQN5%2FHWfpt46XnPGoJtg2A8Yc3Atmi0mk7K8FUsF6WZFBTX4hpYSW9LceNxbGWR6DAa9Iyq7BBmEht7cQ30OGIqA66rkxUb8gmNBybxxG8SNWwCjhbuEy4ue1zLrJNbqG8vAZ19pX5c%2BQ2dI2GfPseRga7qObidtzZzAXspyix7AXPS1NeEqXoXfZyFkjhvgx%2Fi4UFG4yVrTU0uKiFGgRCrX25kql84OCVxo%2FoTjWc1tWtdKKugL5zryDgi2M0Od4jv9HCWfhHrHPtXKNCfBaRQREpahV2jTgwCkOBKMfpxhaqTW5HVypH4ixlm7TgvdkD0cLps1c40SD6d5AOy%2FN0PeLpt3lyEg7pl8Qxh2JPcpsJQb5A3nMaEtLGLgUxGP0a%2BOA%2FGRRt9eOug%2BJyQZt6rD5bdSOZ%2FEnyZmcvpzJtboRO7LW%2BWPbV5ONPfwAYbcm5eM3oWNTIllB0Hqrw%2B5Qtq7XAdzB3ut4NTlkLcLHFXCcJHLQfldnYAV0TYIzzedC%2FYF%2BaUbzgfzUomVp29mFoYE209agDGdBea3kMSmMYJ7LmLekOYIBZRTmJQZj0Zopp6SC9ht8YYet9zps%2FWDEyv2pQ%2BkfNGx2%2B0ZTx%2BF1T02Wh1bL6%2FiNZPmj47XxGjS57prJ9amSkL6nxRuHhCzGJiEPxN5srf7Vou9RA%2BxJeyK%2FGeIHbocnsruejAzliTzbgHVEt1O5mi%2B1nkfczhOfAA9uSPq%2BEvPiaOZllMN8dWUmMq2uLYVUL5afEOQZSBsHI7jbim9T3kRSYc7F1pPbX%2FjJ5Duy9L9fyyksoZxnK6XG5%2BVprjRZ98tX%2F0Wv7WT7eodcVPSkhG4ArvXtlFqITs%2BSO7YwP26QnvE9RyhNzCu5HrMzZRhE38E26D7ED3SqZrX0XsoeRkGar%2FhcetYUyt4doXFzxcOFtyD6lpRactaSFiefUlCOt1QFX8ssRnmGgZIXSjHSK3HbCVhreY2G%2Fs7jRc%2FAJeL8XEnQ5YwPHY8TZIGm7BO8dpBqOaf%2FNMWdIuXzelM%2BbxTKF45M%2BfT911wA2TL1Nnj90fsO8WPnVA%2FdO42KGRW8uo%2FKAqdJ5xYd3wbai1HpnJlYuPrj8ljM%2FTTfX7ZT9WEXae4yj8HTpcyr1Re4Uu%2Bqz5jdq%2F8A%3C%2Fdiagram%3E%3C%2Fmxfile%3E"></iframe>




## Appunti incontro
+ devono modificare comportamento e valori di default di `exist` in id_manager (deve ritornare True di default)
+ il metodo check_digits() deve essere tolto dalla classe base (perché non tutti gli id manager ce l'hanno) e forse sarebbe anche meglio non metterlo come metodo pubblico, dal momento che tendenzialmente lo si usa solo dentro is_valid (ui secondo me è da vedere, perché forse riesco ad immaginarmi dei casi in cui posso usarlo separatamente nel processo di validazione). 
-----------------------------
++++++++++++++++++++++



+ **la validazione è solo intra-tabella** (per esempio, utente specifica un issn e mi dice che è un publication type che un issn, *per il fatto di essere un issn*, non può essere, tipo journal_article!). Non devi mai controllare le informazioni semantiche estraibili con l'API con i dati inseriti dall'utente, ma fidarti dell'utente:
	+ es., se l'utente inserisce un doi sbagliato per la risorsa che ha caricato (quindi inserisce un titolo che non è quello che corrisponde a quello che otteniamo dall'API per l'identificativo inserito), noi ci fidiamo e prendiamo per buono quello che dice l'utente, senza nemmeno segnalare niente!
	+ get_extra_info tu non lo dovresti usare mai! perché l'unica cosa che devi vedere con l'api è se la risorsa esiste, per il resto di fidi dell'utente rispettp ai dati che ha inserito. anche perchè, silvio fa notare, potrebbe tranquillamente essere che i dati che otteniamo dall'api siano scorretti.  

+ COSA SALVARE NEL REPORT DELLA VALIDAZIONE? SOLO GLI ERRORI O ANCHE I VALORI PER I DATI CHE HANNO PASSATO LA VALIDAZIONE? Silvio dice che il report dovrà essere un json (quindi prima potrà essere un dizionario); se salvi tutto quanto, salvi anche dati "inutili", però rendi le cose più facili dopo per la creazione dell'interfaccia; se salvi solo gli errori, risparmi spazio, e comunque puoi localizzare successivamente gli errori sul documento originale (credo?), ma le cose sono più difficili al momento di creare l'interafaccia. Silvio prima osserva che a questo punto conviene salvare direttamente tutto, poi consiglia di **mettere, a monte del processo di validazione un flag (booleano), che potrebbe essere un parametro della funzione principale (??) che regoli che cosa ritornare in output**: es. se true ritorna tutti i dati, se false ritorna solo gli errori. 
+ per wikipedia usa page id che è sicuro, in quanto non cambia. così si risolve anche il "problema" delle pagine di redirect (mentre una pagina può essere spostata, cioè cambiare di titolo, e il titolo può essere associato ad una pagina di redirect, il page id non cambia). Riguardo alle pagine di disambiguazione Silvio dice che non sono un problema e che anzi vanno mantenute come bibliographic resources valide. 
+ correggi flowchart: se non passa validazione il processo finisce; togli triangolino e metti delle frecce che confluiscono; 

## domande
1. che cosa fare con le extra info? confronto con dati inviati dall'utente può andare? risposta: vedi sopra!

