---
title: "Prova 3"
---

# Le immagini

## E se volessi inserire un'immagine?
Il mio dubbio riguarda il formato del link. In questo momento provo semplicemente:
* mettendo il wikilink con path assoluto e cartella thesis_diary come root (impostazioni settate su obsidian in generale), cioè con il comando `![[images/desperate-lisa.png]]"` --> funziona su Obsidian ma non sul sito
![[images/desperate-lisa.png]]
* **questo markdown link dovrebbe funzionare (comando `![Lisa disperata](/images/desperate-lisa.png)`), sia su obsidian che sul web**, ma funziona solo su obsidian
![Lisa disperata](/images/desperate-lisa.png)

L'immagine è salvata nella cartella /thesis_diary/images, quindi all'interno del vault


Ora proviamo invece con un'altra foto salvata all'esterno del vault, in modo che dobbiamo per forza specificare il path assoluto con /content come root.
La cartella in cui è salvata l'immagine è content/images. Proviamo con il markdown link per intero `![Lisa invecchiata](C:/Users/media/Documents/GitHub/quartz/content/images/badly-aged-lisa.jpg)`  --> funziona solo su Obsidian
![Lisa invecchiata](C:/Users/media/Documents/GitHub/quartz/content/images/badly-aged-lisa.jpg)


e poi con il percorso assoluto relativo a /content `![Lisa invecchiata](/images/badly-aged-lisa.jpg)`, che percò chiaramente non viene mostrata in Obsidian, che naturalmente non riesce a trovare il file. 

![Lisa invecchiata](/images/badly-aged-lisa.jpg)