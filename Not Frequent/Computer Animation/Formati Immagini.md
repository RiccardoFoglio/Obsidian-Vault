immagini analogiche realizzate scomponendo l'immagine in righe
I fotogrammi della TV analogica sono composti da:
- 525 linee per standard NTSC (Stati Uniti)
- 625 linee per lo standard PAL (Europa)
circa un VGA : 640x480

Un immagine digitale realizzata tramite pixel, disposti in righe (linee) e colonne
Ogni pixel ha una certa intensità
- pixel (Picture Element) è uno dei molti minuscoli punti che compongono la rappresentazione di un'immagine nella memoria di un elaboratore elettronico

Immagine digitale monocromatica è una matrice di valori di intensità luminosa (livelli di grigio)
- M = numero di righe o linee dell'immagine
- N = numero di colonne dell'immagine

La risoluzione di un immagine digitale è una misura della capacità di distinguere dettagli
- Misure fisiche: 300 pixel per pollice (DPI)
- Risoluzione: 1024x768

Esempio: 4x4 pollici con 300 DPI --> 4x4x300x300 = 1440000 pixel * 8bit = 1.37MByte

![[Screenshot 2025-04-15 at 4.31.41 PM.png|500]]

![[Screenshot 2025-04-15 at 4.32.36 PM.png|500]]

Quand'è che l'utente riesce a fondere linee separate come un unica rappresentazione?
Quando dalla posizione dell'osservatore guardando 2 linee consecutive si forma un angolo minore o uguale a 1' --> $\sin(1')=0.00029$ circa 0.3mm
Esempio: 480 righe --> distanza pari a 7 volte l'altezza dello schermo


Informazioni di colore

al tempo: 16 bpp --> 5R 6G 5B (occhio più suscettibile al verde) --> 2ˆ16 colori

oggi: 32bpp --> 8R 8G 8B 8$\alpha$ --> 2ˆ24 colori --> più combinazioni ma tante collassano l'una con l'altra 

occhio umano percepisce circa un milione di colori

Ogni valore di intensità luminosa deve essere rappresentato su un numero finito di bit
se b = numero di bit per pixel, ogni pixel può assumere un valore nell'intervallo \[0, 2ˆb -1]

Problemi di aliasing spaziale: sfumature di grigio si fondono tra loro creando l'illusione di una immagine "liscia"
Filtro low-pass --> perdita di informazioni 

Bit plane slicing : un immagine digitale da 8bit può essere interpretata come la sovrapposizione di 8 piani immagine da 1 bit
I bit in ordine più elevato contengono la maggior parte dei dati visuali più significativi
Gli altri piani contribuiscono a definire i dettagli più fini dell'immagine originale

![[Screenshot 2025-04-15 at 4.54.41 PM.png|400]]

![[Screenshot 2025-04-15 at 4.55.02 PM.png|500]]

In un eventuale compressione con perdita, vale la pena perdere i bit plane più bassi (da 8 bit per pixel a meno, diminuendo il bit-ratio)

Rappresentazione dei colori: modello RGB è il modello di rappresentazione più usato
corrispondono approssimativamente ai 3 tipi di coni presenti sulla retina dell'occhio umano

Valori tipici:
- 8 bit -->  256 colori
- 16 bit --> 65mila colori
- 24 bit --> 16 milioni di colori (TrueColor)

Indipendenza dei 3 canali di colore
- 1024 x 768 x 8bit x 3 canali = 2304 kB

Codifica colori YUV : Luminanza (Y) + Crominanza (U V)
Ha permesso di avere compatibilità del segnale a colori con le vecchie TV in bianco e nero (che trattano solo la luminanza)

Le TV prima trasmettevano solo Y, e buttavano U V. Migliore tech = uso U V per colori e compatibilità delle TV con il sistema

Y = 0.3R + 0.6G + 0.1B
V = R - Y
U = B - Y

Y resta intantto, e U V da modificare per migliorare l'immagine
![[Screenshot 2025-04-15 at 5.08.53 PM.png|500]]

Codifica colori YUV
![[Screenshot 2025-04-15 at 5.11.00 PM.png|500]]

Compressione senza perdita --> file più piccolo senza perdere informazioni
Compressione con perdita --> JPEG --> eliminazione delle alte frequenze poco percettibili all'occhio

Rappresentazione dell'immagine come quadrati 4x4: Y sempre al massimo e poi U e V dimezzate o ridotte a 1/4
- 4:4:4 --> Altissima qualità per immagini che devono subire altre trasformazioni
- 4:2:2 --> per applicazioni domestiche
- 4:1:1  --> per applicazioni domestiche

# Compressione delle immagini digitali

 Lossless (senza perdita) e Lossy (con perdita)

## Codifica Run-Length

La codifica run-length si basa sul conteggio delle occorrenze successive dello stesso valore di un dato pixel all’interno di una sequenza 
	Per ottenere una versione compatta della sequenza, si sostituisce ogni successione di caratteri uguali con l’indicazione del carattere stesso seguita dal valore che indica il numero di volte con cui esso compare

![[Screenshot 2025-04-15 at 5.18.49 PM.png]]

Buona compressione per immagini in bianco e nero puro (immagini binarie)
Utilizzo la codifica di Huffman per trasmettere la lunghezza delle sequenze di pixel uguali
Tecnica usata nel FAX

## Formato GIF

Graphic Interchange Format introdotto nel 1987 da CompuServe per fornire un formato adatto alle immagini a color. Metodo efficiente per trasmettere delle immagini sulle reti di dati
Questo formato usa una forma di compressione a dizionario LZW (Lempel-Zig-Welch) che mantiene inalterata la qualità dell'immagine

Codifica LZW: tecnica massicciamente utilizzata nella compressione dati, utilizza una tabella per rappresentare sequenze ricorrenti di simboli

Limite di 256 colori

è stato brevettato --> nessuno voleva pagare --> muore

## Formato PNG

Migliore formato di compressione lossless, l'informazione d tutti i bit originali sono preservati
è basato su predizione e codifica entropica

![[Screenshot 2025-04-15 at 5.27.57 PM.png]]

PNG supporta 
- la funzione interlacciata
- sia TrueColor che Palette colori
- supporta trasparenze

## Formato JPEG

anni 90 formato standad di memorizzzione immagine foto con riduzione dello spazio occupato
Nasce nel 1992, immagini a tono continuo sia a livelli di grigio che a colori, cioè immagini naturali

Standard JPEN definisce
- specifiche rigide per decompressione
- solamente guidelines per compressione

elaborazione chiave della compressione JPEG è la DCT: Discrete Cosine Transform, nella versione bidimensionale (2D)

è una trasformata che in generale consente il passaggio del segnale dal dominio del tempo al dominio della frequenza

La trasformata calcola i coefficienti necessari per rappresentare il segnale originario tramite delle funzioni di base
![[Screenshot 2025-04-15 at 6.04.49 PM.png|400]]

### DCT

Coefficienti derivano dalla DCT rappresentano le ampiezze di quei segnali armonici che sommati ricostruiscono il segnale

Nel JPEG viene usata la versione bidimensionale della DCT ed in questo caso non si parla di tempo e frequenza ma spazio e frequenze spaziali
![[Screenshot 2025-04-15 at 6.05.59 PM.png]]

Matrice 8x8 pixeel, la trasformata spaziale bidimensioanle DCT è la seguente
![[Screenshot 2025-04-15 at 6.06.12 PM.png]]

alto a sx: costante
basso dx : massima variazione spaziale
Noi vediamo poco le brusche variazioni di luminosità, tagliate dall'occhio (filtro low-pass)

Pipeline di algoritmo JPEG
![[Screenshot 2025-04-15 at 6.09.28 PM.png]]

- Conversione RGB -> YUV

- Sottocampionamento : 4:4:4 - 4:2:2 - 4:1:1

- DCT sui blocchi -> 8x8 pixel, applico DCT su ogni blocco --> 64 pixel diventano 64 coefficienti delle funzioni base 

- Quantizzazione : perdita. --> matrice di quantizzazione riduce i valori risultanti in valori tra 0-1. matrice di quantizzazione ha coeff. più alti nell'angolo in basso a dx --> maggiore divisione, prossimi allo zero. Arrotondamento all'intero più vicino

![[Screenshot 2025-04-15 at 6.16.27 PM.png|300]]

Fattore di scala (Q) moltiplica la matrice di quantizzazione, regola la compressione *Qualità*

- Lettura zig-zag
  ![[Screenshot 2025-04-15 at 6.22.25 PM.png|300]]
- Run Length Encoding --> vettore della lettura zig-zag contiene molti zeri, rappresentato tramite coppie *skip, value*
- Codifica di Huffman : ultima codifica applicata ai dati. suddivisione in stringhe di bit, analizzata la frequenza statistica di ciascuna stringa e ognuna viene codificata con un codice a lunghezza variabile in funzione della frequenza di apparizione
![[Screenshot 2025-04-15 at 6.25.22 PM.png|400]]
![[Screenshot 2025-04-15 at 6.25.35 PM.png|500]]
![[Screenshot 2025-04-15 at 6.25.54 PM.png|500]]
![[Screenshot 2025-04-15 at 6.26.10 PM.png|500]]
![[Screenshot 2025-04-15 at 6.26.21 PM.png|400]]
![[Screenshot 2025-04-15 at 6.26.32 PM.png|500]]
![[Screenshot 2025-04-15 at 6.26.47 PM.png|500]]
![[Screenshot 2025-04-15 at 6.27.00 PM.png|500]]

I più probabili hanno codifica di bit più corto
Le meno probabili hanno la codifica in bit più lunga

## JPEG 2000

successore del JPEG. Si usa trasformata Wavelet (DWT) anzichè DCT
Si quantizzano i coefficienti prodotti dalla trasformata Wavelet
Bitstream è organizzato in modo da consentire decodifica parziale (ridotta risoluzione o qualità)

![[Screenshot 2025-04-15 at 6.30.07 PM.png|500]]

Complessità è un ordine di grandezza più grande del JPEG
Fornisce molte funzionalità aggiuntive, ma ormai JPEG è lo standard


Confronto algoritmi:
- lossless : si confronta solo capacità di ridurre il numero di bit
- lossy : si confronta la capacità di ridurre il numero dei bit consideranto la riduzione di qualità dell'immagine

Rapporto di compressione
- alto = spazio molto più piccolo rispetto ad originale

Misura della qualità
- Prove soggettive --> esperti danno giudizio, costano
- analisi di algoritmi per valori oggettivi
	- misure di distorsione (errore quadratico medio, rapporto segnale-rumore)

![[Screenshot 2025-04-15 at 6.37.29 PM.png|600]]
all'aumentare il bitrate, diminuisce la distorsione e aumenta la qualità

Il JPEG è simmetrico:
![[Screenshot 2025-04-15 at 6.41.37 PM.png]]


# Formati Video

Segnale video = rapida successione di immagini statiche

Compressione dati video:

720 punti / linea
572 linee (288 pari e dispari)
50 semiquadri al secondo (50Hz)
3 byte/pixel (RGB)
--> 720x288x50x3 = 31.104.00 byte/sec = 31MByte/sec = 250Mbps

2h di film = 2x60x60x31 = 223.200 MByte = 223GByte


Codifica inter-frame : codifico ogni frame utilizzando la conoscenza delle immagini precedenti, frame son simili l'uno con l'altro

2 standard
- MPEG --> Movie Picture Expert Group
- ITU-T --> International Telecomunication Union

Nel tempo: 
- MPEG-1 (1992) --> VideoCD supportato dalla maggior parte dei lettori DVD, codifica audio diventa MP3
- MPEG-2 (1994) --> DVD, TV Broadcasting (satellitare, terrestre, cavo)
- MPEG-4 (1998) --> Internet streaming
- MPEG-4 Part 10 (2003) Equivalente a H.264

Multimedia container != codifica, come `.AVI`
- Multimedia container contiene dati video, audio, sottotitoli ecc...


## MPEG

Codifiche
- Intraframe : DCT based, JPEG
- Interframe. : block-based motion

Codifica
- Causale (predictive coding)
- non causale (interpolative coding)

Miglior tradeoff tra qualità e compressione
Lavora su 16x16 per luminanza e 8x8 per crominanza (4:2:2)

MPEG stream includes 3 types of image coding: 
- I-frames - Intra-coded frames - access points for random access, yields moderate compression 
- P-frames - Predictive-coded frames - encoded with reference to a previous I or P frame. 
- B-frames - Bi-directionally predictive coded frames - encoded using previous/next I and P frame, maximum compression

I sistemi di video MPEG bufferizzano: accumulano storia passata e futura

![[Screenshot 2025-04-15 at 6.59.21 PM.png|400]]

![[Screenshot 2025-04-15 at 6.59.34 PM.png|400]]


più è alto il gap tra i frame I, e più è alta la compressione
è l'unico fotogramma che può essere decodificato senza storia passata e futura
	se vai avanti, fino al prossimo I