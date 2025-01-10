Necessità di standard per la Computer Graphics

- 1977 specifica del prima sistema per grafica 2D sviluppato da ACM SIGGRAPH
- 1984 prima API industriale proprietaria della Silicon Graphics per grafica 3D
- 1985 primo standard ufficiale di un API per grafica 2D, ANSI/ISO
- 1988 primo standard ufficiale di un API per grafica 3d, ANSI
- 1992 standard che integra PHIGS con X Window System
- 1993: primo standard industriale 3D portabile su diverse piattaforme hardware (OpenGL) 
- 1993: API ad alto livello su base OpenGL che segue paradigmi simili a PHIGS, ad oggetti (OpenInventor)
- 1995: API proprietaria Microsoft in concorrenza con OpenGL (Direct3D/DirectX) 
- 1994/97: derivato da OpenInventor il primo linguaggio per la grafica 3D e la realtà virtuale su Web (Virtual Reality Modeling Language – VRML)
- 1999: API Java per la grafica 3D (Java3D)
- 20??: estensioni di OpenGL e DirectX per H/W grafico programmabile e mobile, Web (es. WebGL)

Le prime API Standard (Core, GKS, PHIGS, PEX) avevano l'ambizione di essere completamente indipendenti dall'architettura H/W sottostante
	scelta inefficiente

Le API proprietarie al contrario ancorate ad una piattaforma H/W o S/W specifica (GL, Direct3D/X)
	scelta limitante

[[OpenGL]] Prevede un modello di architettura H/W di riferimento (frame buffer) ma ha l'obiettivo di poter essere implementato su qualunque piattaforma che rispetti tale modello (tutte quelle esistenti)
	scelta vincente, ma programmazione di basso livello

Le API di alto livello assumono lo stesso modello di architettura H/W ma tentano di portare l'interfaccia di programmazione ad un livello più astratto (OpenInventor, Java3D, VRML)
	Problemi di efficienza

Nuove API via via rilasciate (nuove gen di OpenGL e Direct3D/X) sono di basso livello ma estendono le proprie funzionalità per sfruttare al meglio le schede grafiche moderne
	problemi di mancanza di consistenza tra diversi produttori di H/W (NVIDIA, ATI) si rischia di avere API molto legate ad un determinato hardware