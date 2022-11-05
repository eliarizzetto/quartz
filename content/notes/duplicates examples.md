---
title: "Duplicates examples"
---
|citing                 |cited                                                                                                                                      |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|A B C                  |x y z                                                                                                                                      |
|B D                    |x                                                                                                                                          |
|D                      |z y                                                                                                                                        |
|                       |                                                                                                                                           |


1. Che `B` cita `x` lo dico sia in riga 0 sia in riga 1 → è un errore perché si ripete una citazione tra singoli identificativi.
```json
{
	0: {
		citing: [1],
		cited:[0]
		},
	1: {
		citing:[0], 
		cited[0]
		}
}
```

2. So che `A`, `B`, `C` e `D` sono tutti la stessa entità bibliografica, così come `x`, `y` e `z`. → è un errore perché si ripete una citazione tra entità bibliografiche (rappresentabili con metaID).
```json
{
	0: {
		citing: [0,1,2],
		cited:[0,1,2]
		},
	1: {
		citing:[0,1], 
		cited[0]
		}
	2: {
		citing:[0], 
		cited[0,1]
		}
}
```

