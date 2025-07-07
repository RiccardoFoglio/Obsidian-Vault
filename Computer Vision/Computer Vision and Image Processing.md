
# Introduction

Le immagini digitali sono funzioni f(x,y) discretizzate nello spazio e nella luminosità, solitamente definite come matrici con fino a diversi milioni di valori. La visione artificiale cerca di simulare la visione umana nelle macchine. L'**Image Processing** si riferisce alla manipolazione delle immagini. La **Computer Vision** e l'**Image Processing** sono correlate alla **Image Analysis**.

Un **pixel** (o pel) è un campione creato da un rilevatore che può fornire un output analogico o digitale.
Un **Datum** è un elemento di dati nella memoria del computer. Se il rilevatore è digitale, Pixel e Datum corrispondono, altrimenti i dati sono forniti dalla velocità di digitalizzazione del frame grabber, che può produrre più dati che pixel. Gli algoritmi di elaborazione delle immagini funzionano con i dati.

L'elemento più piccolo accessibile da un mezzo di visualizzazione è chiamato **Disel** (ad esempio con una stampante (600 dpi) e con 1024x768 pixel, la dimensione dell'immagine è 43x32 mm, troppo piccola, di solito c'è una mappatura uno a molti).

L'occhio umano è in grado di distinguere un oggetto grande minimo 1/10 di mm quindi, di conseguenza, la grandezza minima di un pixel è di 1/10 mm anche se, nella maggior parte dei casi, vengono usati pixel di ⅕ mm.
Nonostante questo limite, l'occhio umano è in grado di riconoscere anche particolari più piccoli di 1/10 mm grazie al contrasto.

Il segnale più piccolo supportato da un sistema analogico è chiamato **Resel** (se il resel è troppo grande non sarà possibile vedere tutti i disels e la risoluzione del sistema sarà data dai resels invece)

Risoluzione del sistema = min{disels,resels}

L'effetto distorto che si ottiene quando si fotografa un display è dovuto al fatto che le matrici rispettivamente del display e del sensore della fotocamera non si allineano perfettamente e, venendo illuminate insieme, producono questo effetto chiamato *effetto moiré*.

# Image Enhancement

Image Enhancement è la procedura che consente di migliorare la qualità e il contenuto informativo dei dati originali prima dell'elaborazione. 
Alcune delle tecniche più comuni di miglioramento dell'immagine nel dominio spaziale ruotano attorno all'utilizzo dell'istogramma dell'immagine. 

L'istogramma è una funzione discreta 
$$h(r_k)=n_k$$
- $r_k$ è il k-esimo livello di grigio 
- $n_k$ è il numero di pixel che hanno quel livello di grigio. 

La funzione dell'istogramma è solitamente normalizzata come 
$$p(r_k)=n_k/n$$
- $n$ è il numero totale di pixel.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe1qweW6Xoyha8b_USXQWLJHd5dopeoazibEGHaq_ZM4LhjcOARw-4a34Qhng2mVvg0B2v4eVrhm01xHBLEPBkvUujItUOXEAM7zuiXkMz4qQSTVMINsq2c8v6mwm15vaA1TBOtTd6hRjk2JR3Sl8ciZEE?key=IlMU9hpgd3pUHC0chA8ahA)

$r_k$ (l'asse x dell'istogramma) varia da 0 a 255 per le immagini a 8 bit (28=256).
L'istogramma può essere utilizzato per verificare se un'immagine è sottoesposta (molti valori sul lato sinistro dell'istogramma) o sovraesposta (molti valori sul lato destro dell'istogramma) e può essere modificato per correggere l'immagine.

La maggior parte delle tecniche di miglioramento delle immagini nel dominio spaziale sono descritte da funzioni di trasformazione:

- <font color="#00b0f0">Thresholding</font> (sogliatura) : viene utilizzato per creare immagini binarie (in cui i pixel sono bianchi o neri). La forma più semplice di soglia parte dall'istogramma, in cui si sceglie un valore di soglia sull'asse x. Ogni pixel al di sotto della soglia sarà nero, mentre ogni pixel al di sopra di tale valore sarà bianco. La funzione di trasformazione in questo caso è una funzione gradino di Heaviside (Heavyside step function).
  
  In computer vision la sogliatura è utilizzata per distinguere lo sfondo dal resto dell’immagine. Per il riconoscimento di immagini, infatti, è preferibile utilizzare algoritmi più semplici perché, nella maggior parte dei casi, anche quelli più efficienti. Se si vogliono ottenere risultati più complicati come l’identificazione e riconoscimento di un volto è più conveniente utilizzare reti neurali.  
  ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdZd2Ni1aP1DviRWh5Z-jzUjX4HdIf6qGccolQACqzucgOC1wfANFJtrtdoQVdWRgiU95JVahJOefyehS0wnkQQV-j-ntCGRc28IBcMqMlDPElrlTp4Y-APkQIbWwmG0aus4c0t8ccQyuDMeYo17EfoYgk?key=IlMU9hpgd3pUHC0chA8ahA)


- <font color="#00b0f0">Log transformation</font>  : consiste nel sostituire tutti i valori dei pixel presenti nell'immagine con i loro valori logaritmici. La trasformazione logaritmica viene utilizzata per migliorare la qualità dell'immagine, poiché espande i pixel scuri dell'immagine rispetto ai valori dei pixel più alti. (recupera dettagli)
  
  ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdylhVV3Yq-fQa6DVM_OYbRTfnKO8J1u2JSqmjdVD08WWGputiFgGnUMb9O20VWQtFxGzF4Om9YTfRNphL1LshQlD7v4gBiR4F0EASzzoR6ADcuk0h0n7XyH0p3hydCKhfpXR18Q5O5968HQFTgNH0Tyk4?key=IlMU9hpgd3pUHC0chA8ahA)


- <font color="#00b0f0">Gamma correction</font> : La correzione gamma di un'immagine è espressa nella sua forma più semplice elevando l'istogramma di un fattore e viene utilizzata per controllare la luminosità complessiva dell'immagine. Se 0<<1 l'immagine apparirà più luminosa, mentre se >1 l'immagine apparirà più scura. Ogni monitor ha il proprio gamma, quindi la correzione gamma è necessaria per garantire che un'immagine appaia uguale su dispositivi diversi.
  
  ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcx-IdkWddzcaTsmcods6dni6RNuTDxf6Gdz3DB4UCajSphEl5SEWpJ3Ld5QByeeS1HjeCVfCRlMzk3pi7L_g2Zrswkp0eg9CXOi4Ptjj9hRgK2kZfgadCYa-QD7UyY7BJ53t307h0ZdZ-aHcfGhfAItZQ?key=IlMU9hpgd3pUHC0chA8ahA)


- <font color="#00b0f0">Contrast stretching (normalizzazione)</font> : Il contrast stretching è un metodo di miglioramento dell'immagine che cerca di migliorare un'immagine estendendo la gamma dei valori di intensità. Il contrasto esteso è possibile solo se il valore minimo e massimo di intensità non sono uguali ai valori minimi e massimi di intensità possibili. In caso contrario, l'immagine generata dopo il contrasto esteso sarà uguale all'immagine di input.
  
  ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd13sDbdW_a5RAGZhsH8x6KAUKS1k5MBMdNv1H2wlRQJeElHou-Gb8uGO3mmdp-FFii884wexgFu2qeJFYIoV8wPlpQWhbansLjyDDwDtNlBueMLMuJZSY-iJdUlDnECBJQWqSCu7CaoTHAdw4ACN12MU0?key=IlMU9hpgd3pUHC0chA8ahA)


- <font color="#00b0f0">Range highlighting / slicing</font> : Analogamente al thresholding, lo slicing trasforma un'immagine in modo da ridurre il numero di sfumature di grigio.


- <font color="#00b0f0">Bit plane slicing</font> : ogni pixel è memorizzato in n bit. Quando prendiamo solo un bit per ogni pixel, ci riferiamo a un piano di bit. I piani di bit sono memorizzati dal più significativo (quello che contribuisce maggiormente all'immagine complessiva) al meno significativo. Possiamo utilizzare un numero ridotto di piani di bit per rappresentare l'immagine.
  
  ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcWcktKr0TcbgDlJt_HzdvAoCSsWLy2PDUhMqdb4wzsGf09yZ1K-kJNWzEWXkHbto_O-xlwZfzSYdnwwNE5Ocp_cz49cZbf8-NRrrYAj0Pg2Kr4lvBmlbvkOM7_beSS5Mt3-Y-EyZcDdGG7OFf2Y4lB_HI?key=IlMU9hpgd3pUHC0chA8ahA)


- <font color="#00b0f0">Histogram equalisation</font>  : L'equalizzazione cercherà di produrre un istogramma con quantità uguali di pixel in ciascun livello di intensità e influisce principalmente sul contrasto. L'immagine risultante potrebbe apparire poco realistica, quindi questo metodo viene utilizzato principalmente per migliorare le informazioni contenute nell'immagine.
  
  ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfOMucWNjEaHclJbqZLNet770AUuj_2heiaDoD5gGXJeDxtvLDQ6WPqBJF8bsUMkq-8VYHHrnGcUQTj4nR5xcL-SnsDlrbUpi8sXf6jBpQMGA2nfWJglO5IzPSioJGu6XrJL617lUg6jr0_5oU5U3G61X8?key=IlMU9hpgd3pUHC0chA8ahA)    
  
  L'equalizzazione dell'istogramma può essere eseguita a livello globale o locale (un'area di n x n pixel). 
  
  Calcolando la derivata della funzione di trasferimento possiamo ottenere l’inclinazione della retta e, di conseguenza il contrasto. In particolare inclinando la retta si amplificano/riducono i valori scuri. Per trasformare un’immagine da negativa ad una diapositiva basta invertire l’inclinazione della retta.
  Se una funzione si trova al di sopra della diagonale principale si ottiene un accentuamento dei toni.  
  Una funzione di trasferimento con degli scalini è il risultato di una funzione di partenza non continua.


- <font color="#00b0f0">Histogram specification </font> : La specificazione dell'istogramma è la trasformazione di un'immagine in modo che il suo istogramma corrisponda a un istogramma specificato. Il noto metodo di equalizzazione dell'istogramma è un caso speciale in cui l'istogramma specificato è distribuito uniformemente.
  

Un'altra possibilità per l'image processing è quella di eseguire operazioni matematiche tra immagini. Queste includono sottrazione, addizione, divisione, moltiplicazione e calcolo della media.

Il **Linear Spatial Filtering** modifica un'immagine sostituendo il valore di ciascun pixel con una funzione lineare dei valori dei pixel vicini, memorizzati in una matrice chiamata **filtro** o **maschera** 
Inoltre, si presume che questa funzione lineare sia indipendente dalla posizione del pixel (i, j).
Se il filtro è simmetrico, la funzione può anche essere definita come una **maschera di convoluzione spaziale**.
In fotografia, l'immagine risultante a livello del sensore è la convoluzione dell'immagine originale (perfetta) con l'apertura finita dell'obiettivo (diaframma).
Un filtro spaziale viene utilizzato per: blurring, sharpening, embossing, edge detection ecc...

Un filtro di sfocatura può essere definito da maschere come:
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXctWKgzZpc1YcMWU6WABIDScYajkOQo0ie81ruNWdIUS7Hei3WbkkrQV18QMfMezNkrqfm_jBQbX6ueU5B6LOu_UV8PYdvYasIbHyvFwSd8iD_NzEVp5BZMgF_iB00aDKsGcrk3jTbUxWLSNgqDFywJYsU?key=IlMU9hpgd3pUHC0chA8ahA)

La prima matrice definisce un **averaging filter**, mentre la seconda un **weighted average filter**.

Per i filtri non lineari, viene utilizzata una funzione non lineare dei pixel vicini per calcolare l'output. Un esempio di filtro non lineare è il **filtro mediano**, che utilizza la mediana dei pixel sotto la maschera ed è particolarmente utile per rimuovere il rumore sale e pepe (pixel il cui valore è anormalmente chiaro/scuro). Esistono anche filtri max, min e mean.

  
L'opposto di un blurring filter è lo sharpening filter, la cui maschera viene utilizzata per calcolare il laplaciano dell'immagine.
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd1-19kXIZYJEgtsjHFhlSoZZ7HeZ6a0hd-SUKxANMsEgeoOpaLFez-93-luyTQRPQQpjohvECTv2cFx8_aTy85sfjS8_099R-geMitZhKkMGKru6NxOvctG9xJOq2nirKOYPVys1Z_IehGoSfXzyqv3E4?key=IlMU9hpgd3pUHC0chA8ahA)
Per ottenere un'immagine più nitida come risultato finale, il laplaciano viene sottratto (o aggiunto se si utilizza una maschera diversa) all'immagine originale.


L'**unsharp-masking** applica una sfocatura gaussiana a una copia dell'immagine originale e poi la confronta con l'originale. Se la differenza è maggiore di una soglia specificata dall'utente, le immagini vengono (di fatto) sottratte. Il filtro maschera di contrasto non crea nuovi dettagli, ma rende più visibili quelli già presenti nell'immagine.

Anche la derivata prima (**gradiente**) dell'immagine è utile per i filtri. Ad esempio, il **sobel filter** evidenzia le aree dell'immagine in cui il gradiente è più elevato, come i bordi.
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdM9DPLNWbQ00bWd5s-G8EJlmUjrwI9Jor1Ri5TpvjLdxoXG7dHhT2d9vWeOXv_clD_uHIAZwsM1YacFpwp440pN7_1B1b23JC_tpRnHsgEbAWRD26sEYPxQD7h-MBOFZDMrh1C12JjJN4hQVLoBQZPt18?key=IlMU9hpgd3pUHC0chA8ahA)

Per migliorare un'immagine, è possibile utilizzare più filtri contemporaneamente.
# Imaging Systems

Il **contrasto** è la differenza di luminosità o colore che rende distinguibili gli oggetti. Un contrasto maggiore consente di distinguere dettagli più fini. La formula per il contrasto è 
$$Lmax-Lmin/Lmax+Lmin$$
dove L è la luminosità. 
È anche possibile distinguere tra contrasto macro (sull'intera immagine) e contrasto micro (sui dettagli più piccoli).  

La **PSF**, o **Point Spread Function**, descrive la risposta di un sistema di imaging a una sorgente puntiforme o a un oggetto puntiforme. Un termine più generico per indicare la PSF è la risposta impulsiva di un sistema, essendo la PSF la risposta impulsiva di un sistema ottico focalizzato. La parte centrale della PSF è l' **Airy Disk**.

La trasformata di Fourier della PSF è l'OTF, ovvero la Optical Transfer Function. 
L'OTF è una funzione a valore complesso che descrive la risposta di un sistema di imaging in funzione della frequenza spaziale. 
**Modular Transfer Function** (**MTF**) =  grandezza dell'OTF complesso. 
**Phase Transfer Function** (**PTF**) = fase dell'OTF complesso.

La formula per l'OTF è
$$OTF = MTF*eˆ[iPTF]$$

ma spesso, quando non sono presenti spostamenti di messa a fuoco o aberrazioni, possiamo semplificarla come $OTF = MTF$.

L'MTF di una lente è una misura della sua capacità di fornire contrasto a una certa risoluzione in un'immagine ed è una buona misura della capacità di un sistema di riprodurre un input.

Quando sono disponibili diversi MTF di diversi obiettivi, l'MTF del sistema potrebbe essere il prodotto di tutti gli MTF; TUTTAVIA, un obiettivo può minimizzare le aberrazioni degli altri, modificando l'MTF finale; il prodotto è valido per MTF indipendenti.

L'MTF può essere calcolato con un segnale sinusoidale o con uno slant edge target. Quest'ultimo è più semplice e può essere calcolato con uno sforzo minore.

Dato che la modulazione M è la variazione di un segnale sinusoidale rispetto al suo valore medio, l'MTF sarà
$$MTF=M_{output}(f)/M_{input}(f)$$

L'output di una MTF non può essere maggiore dell'input perché violerebbe il principio di conservazione dell'energia. Nonostante ciò, a volte questo rapporto diventa maggiore di uno, questo perché le fotocamere applicano una maschera di contrasto. Per amplificarlo si usano elementi attivi (che prendono energia dall'ambiente) come, ad esempio, i transistor. Usando un filtro elettronico ci si deve preoccupare degli sfasamenti.

Il metodo dello slant edge target viene utilizzato per calcolare la step function response del sistema e può essere eseguito con programmi come Imatest.

I target (mire) vengono utilizzati per acquisire l'immagine in ingresso e valutare le prestazioni di un sistema ottico. I target standard più utilizzati sono l'USAF 1951 e l'ISO 12233.

Le mire sono praticamente un quadrato di cristallo con sopra del cromo opaco che quindi crea ombre quando illuminato.

![|500](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcpfSeURB7jWV2nRUtsk8uslBWfZl93eNco9F_O2PuX26Nb8XuQ5IncuJmFhIBNTakzGoD1HsIPsqHPheEXpOxQLbGOqqb8vmlnnq9CDxhW9C7nTuhAwuTKmk1EGAm7N1HiJ1cawiI-K6qAONHwPiHTTr4?key=IlMU9hpgd3pUHC0chA8ahA)
![|500](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcCmnCwVpQsmXO3R6Zeq6b_2Wyrkp4YWRLLT5kmADSiSSX6YfIIsI2ihAQfwMJ4XAXTeaajn-i8r0D_ecIfLduXy4adfnilP5IWHaPFAbVVDhP89dZeyg3qHj1PmnuJDTrp_2lkYyiEb_QBSw0wH8iqqpk?key=IlMU9hpgd3pUHC0chA8ahA)

Un altro esempio di mira è la stella di Siemens (di seguito), in cui la frequenza spaziale aumenta verso il centro. In questa l’OTF può essere considerato nullo nonostante ci possa essere uno sfasamento di 180° (anche se in realtà non è esattamente nullo).

![|500](https://lh7-rt.googleusercontent.com/docsz/AD_4nXds4GIhGOWFP_lM_JX7dinMFXcWVYNfhiK3XUDkB2v9BiCFLjMl0-6MLeoY0eJjSDQHBpXpk6ZZaNby8Morc1zU1VU5LMK2v0mX83_mfS_jaTzyxdnihsOa4z4-clAHdNCK9WrRUttBfC-TQh3eMgB4Zvk?key=IlMU9hpgd3pUHC0chA8ahA)

La **funzione di trasferimento del contrasto** (CTF) descrive quanto l'immagine è influenzata dalle aberrazioni e può essere approssimata come $1/MTF$. 
L'intersezione tra l'MTF del sistema e la CTF dell'occhio è chiamata **frequenza di risoluzione** e dipende dalle dimensioni del display e dalla distanza di visione.

Da ciò otteniamo la formula per l'MTFA
$$MTFA = \int\limits^{RES}_{0}[MTF_{SYS} − CTF_{EYE}]du$$

dove RES è la frequenza all'intersezione. 
Altri parametri per valutare una lente sono l'SQF (fattore di qualità soggettivo) e l'SQRI (integrale della radice quadrata).

Imatest è un programma in grado di calcolare tutte queste funzioni a partire da uno slant edge target.

In un'immagine, il disco di Airy è il centro luminoso del modello di diffrazione creato da un sistema ottico ideale con apertura circolare. Il diametro del disco di Airy è dato da
$$2,44 * \lambda * f_{number}$$
Qualsiasi punto più piccolo di un disco di Airy non può essere messo a fuoco, poiché la fotocamera è limitata dalla diffrazione.

Il disco di Airy è l'oggetto più piccolo visibile con l'ottica tradizionale.

Nei cellulari si hanno diaframmi più aperti perché la focale è più corta di una macchina fotografica, il sensore è piccolo, i pixel devono essere molto piccoli e quindi, per dare senso a questi piccoli pixel, è necessario utilizzare un sensore più grande.

Una telecamera può essere considerata come uno oscilloscopio bidimensionale perché scatta più foto al passare del tempo; per vedere sia parti poco illuminate che parti molto illuminate basta aumentare il range di bit del convertitore analogico-digitale.


Le differenze tra l'immagine originale e l'output del sistema sono note come aberrazioni.
Esistono 7 tipi di aberrazioni, 5 monocromatiche e 2 cromatiche.

Le aberrazioni monocromatiche sono:

- <font color="#00b0f0">Aberrazione sferica</font>: la sfocatura ai bordi di un'immagine. L'uso di una lente sferica su una fotocamera fa sì che la luce vicino al bordo della lente (più lontana dall'asse ottico) converga più vicino alla lente.

- <font color="#00b0f0">Coma</font>: Un tipo di aberrazione che colpisce solo i raggi di luce provenienti da un punto che attraversano la lente con un angolo. Con la coma, i raggi non si rifocalizzano su un punto, ma si allontanano da esso. Questo fa sì che i punti luminosi sembrino una cometa con una coda sfocata, da cui il nome.

- <font color="#00b0f0">Astigmatismo</font>: si verifica quando la lente non riesce a mettere a fuoco le linee dell'immagine che corrono in direzioni diverse sullo stesso piano; in una foto di una staccionata, ad esempio, i pali verticali sono nitidi con una messa a fuoco diversa da quella delle assi orizzontali.

- <font color="#00b0f0">Curvatura di campo</font>: L'aberrazione di curvatura di campo descrive il fatto che i raggi paralleli che raggiungono la lente da direzioni diverse non si focalizzano su un piano, ma piuttosto su una superficie curva. Ciò causa una sfocatura radiale, ovvero, per una data posizione del sensore, solo una corona circolare sarà a fuoco.

- <font color="#00b0f0">Distorsione</font>: si manifesta con cambiamenti nella forma di un'immagine piuttosto che nella nitidezza o nello spettro dei colori. È causata da diverse lunghezze d'onda della luce che subiscono vari gradi di rifrazione e vengono messe a fuoco in posizioni diverse mentre attraversano l'obiettivo.


Aberrazioni cromatiche:

<font color="#00b050">Aberrazione cromatica longitudinale (LCA)</font>: L'LCA si verifica se una lente non è in grado di mettere a fuoco colori diversi sullo stesso piano focale. È causata dalla luce incidente diritta. I fuochi dei diversi colori si trovano in punti diversi nella direzione longitudinale lungo l'asse ottico.

<font color="#00b050">Aberrazione cromatica trasversale (TCA)</font>: La TCA si verifica quando la dimensione dell'immagine cambia con la lunghezza d'onda. In altre parole, quando si utilizza la luce bianca, le lunghezze d'onda del rosso, del giallo e del blu si mettono a fuoco in punti separati su un piano verticale.


Una lente può essere vista come due prismi attaccati tramite le loro basi; il problema di questi due prismi e, successivamente, delle lenti è l’aberrazione cromatica: le varie lunghezze d’onda all’interno del prisma hanno indici di rifrazione diversi e, a causa della dispersione, viene a crearsi un arcobaleno. Ad oggi non è possibile costruire una lente priva di aberrazione cromatica. Nei telescopi è stato risolto utilizzando degli specchi che riflettono tutte le lunghezze d'onda: usano due lenti, una convergente e una divergente; in particolare si può risolvere questo problema usando per ogni lente dei materiali diversi.

Per prevenire alcune di queste aberrazioni, è possibile realizzare una fotocamera senza lenti; tale fotocamera è una **pinhole camera** e i suoi principali vantaggi sono che non presenta distorsioni, né curvatura di campo, né aberrazioni cromatiche, ma in cambio soffre di astigmatismo e non è otticamente nitida.

La soluzione a queste aberrazioni cromatiche è l'utilizzo di uno specchio perché riflette le onde in un unico raggio.

Una macchina fotografica di solito ha delle lenti. Le lenti sono un elemento ottico realizzato in vetro in grado di divergere o convergere i raggi luminosi in un punto. Il tipo più comune di lente è quella sferica, ma esistono molti tipi diversi di lenti.

![|500](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfU3p0RvTOgmSgTbJ42I9mRTLy06UAqjke7JL5AIxeqnZvt_Nj2NreCijEPg-9WsOoG_fC2RzcvCE_-NMN8QIaa4nn5_9G2CFIIetgUp5YgpL63_yIFQk5xjvKTCudmzq80vEIV66gk-FvPq9dBYhy3xjQ?key=IlMU9hpgd3pUHC0chA8ahA)

Esistono anche lenti asferiche più complesse, spesso utilizzate negli occhiali.

Altri tipi di lenti speciali sono:

<font color="#ffc000">Lente di Fresnel</font>, una successione di anelli concentrici, ciascuno costituito da un elemento di una lente semplice, assemblati in modo corretto su una superficie piana per fornire una lunghezza focale breve. La lente di Fresnel è utilizzata in particolare nei fari e nei proiettori per concentrare la luce in un fascio relativamente stretto.

<font color="#ffc000">Lenti telecentriche</font>, progettate per avere un ingrandimento costante indipendentemente dalla distanza o dalla posizione dell'oggetto nel campo visivo. Questa caratteristica è ideale per molte applicazioni di misurazione della visione artificiale, poiché le misurazioni delle dimensioni di un oggetto saranno indipendenti dalla sua posizione.

Le <font color="#ffc000">lenti anamorfiche</font> sono strumenti speciali che influenzano il modo in cui le immagini vengono proiettate sul sensore della fotocamera. Sono state create principalmente per consentire a una gamma più ampia di rapporti di aspetto di adattarsi a un fotogramma standard, ma da allora i cinematographers si sono abituati al loro aspetto unico.


Un'immagine si forma quando i raggi luminosi provenienti da un oggetto a distanza *p* attraversano la lente e convergono sull'altro lato a una distanza *q*, se $p>f$. 
Il punto oggetto e il punto immagine di un sistema di lenti sono detti punti coniugati.
Poiché tutti i percorsi della luce dall'oggetto all'immagine sono reversibili, ne consegue che se l'oggetto fosse posizionato dove si trova l'immagine, si formerebbe un'immagine nella posizione originale dell'oggetto. 
La formula per questo è 
$$\displaystyle \frac{1}{p} + \frac{1}{q} = \frac{1}{f}$$
L'immagine proiettata, sebbene simile, non può mai essere uguale alla realtà che vediamo a causa di una serie di fattori quali l'atmosfera, l'apertura dell'obiettivo, le aberrazioni, l'allineamento e gli ostacoli, l'assorbimento e la riflessione tra le lenti, la vignettatura, la sfocatura, i problemi agli occhi e gli errori di elaborazione mentale.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcfcSeYLe3U2J7xqTXrL2I6XESNBQNRZK6EHjVr-1yXHe14L1mPcAmTn1mgIW2c9NpzaTsXCgdQwMOIPPItTT6vrI-4e5c76oBWidkHL3sS08of4xlalJxh-2jV2rGvj7KjAXuQWGeihcpZPGf0VjES-wE?key=IlMU9hpgd3pUHC0chA8ahA)

Le lenti sono solo una parte di una fotocamera, l'altra metà è il sensore.

Un sensore rileva e trasmette le informazioni utilizzate per creare un'immagine. Lo fa convertendo l'attenuazione variabile delle onde luminose (quando attraversano o riflettono gli oggetti) in segnali, piccole scariche di corrente che trasmettono le informazioni.

Le due tecnologie più utilizzate per i sensori sono **CCD** e **CMOS**.

I <font color="#00b0f0">Charged Coupled Devices</font> (CCD) sono circuiti integrati incisi su una superficie di silicio che formano elementi sensibili alla luce chiamati pixel. I fotoni che colpiscono questa superficie generano una carica che può essere letta dall'elettronica e trasformata in una copia digitale dei modelli di luce che cadono sul dispositivo.

A differenza dei sensori CCD, i sensori CMOS (<font color="#00b0f0">Complementary Metal-Oxide Semiconductor</font>) convertono la carica in tensione direttamente nei pixel. L'amplificazione e la quantizzazione della tensione creano il valore digitale in uscita.

Il CCD ha una qualità di output migliore rispetto al CMOS, essendo meno sensibile al rumore, ma è molto più costoso in termini di energia.

Per rappresentare immagini con dynamic range più ampio, possiamo utilizzare immagini **HDR** (High Dynamic Range), che sono una composizione di immagini con esposizioni diverse
Di solito vengono scattate diverse immagini con esposizioni diverse (con fino a diversi f-stop tra le diverse immagini), le immagini sovraesposte vengono utilizzate per migliorare i dettagli delle ombre, mentre le immagini sottoesposte vengono utilizzate per migliorare i dettagli della luce piena.

L'**ISO** è la sensibilità alla luce della fotocamera in relazione alla pellicola o al sensore digitale. 
Un valore ISO più basso significa minore sensibilità alla luce, mentre un valore ISO più alto significa maggiore sensibilità. 
L'ISO NON modifica la luce che entra nella fotocamera, ma agisce come un amplificatore. In quanto tale, amplifica anche il rumore che entra nella fotocamera.

Alcuni sensori, tuttavia, sono più invarianti rispetto ad altri, il che significa che il rumore è meno aumentato dall'ISO.

L'**esposizione** è il numero di fotoni per un singolo pixel del sensore.

I sensori delle fotocamere utilizzano un **Bayern Pattern** per catturare il colore nelle immagini: ogni pixel assorbe la luce di un determinato colore (verde, rosso o blu) che, quando mescolati insieme, creano l'illusione del colore.

![|500](https://lh7-rt.googleusercontent.com/docsz/AD_4nXelPszCPrpC_CpXIUrNw_ETSPzgDQde51xX9rDeMEafSvt-1Ijm57x4ctJbz0nyjYzOQHb5ArbuXHf5NQPWEHWF9knxok-r_MZXIU-wAIyO9mGeNS5zgv_ZfGmPWGl5hHRVU25NAveKX5qvu3nEmvT0GPY?key=IlMU9hpgd3pUHC0chA8ahA)

Se la dimensione dei pixel del sensore cambia e viene ridotta, avremo meno fotoni per ogni pixel e quindi sarà necessaria una maggiore amplificazione dei fotoni esistenti. 
Una maggiore amplificazione comporta un aumento del rumore.
Questo è ciò che accade confrontando immagini scattate, ad esempio, con una reflex full frame e uno smartphone.
La scarsa qualità delle foto del cellulare deriva dal fatto che i pixel del cellulare sono troppo piccoli e quindi non riescono ad assorbire abbastanza luce per riprodurre l'immagine in modo efficace.

Una **3D camera** è un dispositivo di imaging che consente la percezione della profondità nelle immagini per replicare le tre dimensioni come sperimentate attraverso la visione binoculare umana. Alcune fotocamere 3D utilizzano due o più obiettivi per registrare più punti di vista, mentre altre utilizzano un unico obiettivo che cambia posizione. Le fotocamere 3D non sono fotocamere a scansione lineare, ma piuttosto a scansione di area.

Il **principio di Scheimpflug** viene utilizzato per risolvere i problemi relativi al controllo della profondità di campo. Questo principio consente sia di regolare il piano di messa a fuoco sia di mantenere un'apertura desiderabile imponendo che il piano dell'immagine (sensore), il piano dell'obiettivo e il piano del soggetto (noto anche come piano di messa a fuoco, che è la parte anteriore del soggetto) si intersecano tutti in un unico punto.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfMzD1LKBTqPW1Y_tm0FrEfDjJfrowkhioZi7cp-6PLh8QiZ8rNHnY1-Nnq8c6BIZ5KhbVR3mN1og_qPRtundaxoPHtbSAA6gtPp-zLX2zT9of5qN1u9uAsZl5MSIDYwNve8xKgPQvEDvfWhhMTF7zKNEM?key=IlMU9hpgd3pUHC0chA8ahA)

  

# Spectral and Spatial Transforms and Illumination

Lo **spettro visibile** è composto dalle frequenze che possono essere viste dall'occhio umano, che vanno da 380 nm a 780 nm.

Un **corpo nero** è un oggetto che assorbe tutte le radiazioni elettromagnetiche ed è una fonte di radiazione correlata solo alla sua temperatura T.

Per migliorare l'acquisizione di un'immagine, può essere utile applicare dei filtri alle luci o alla fotocamera. Ad esempio, i **filtri polarizzanti** possono essere utilizzati per eliminare i riflessi. Poiché il riflesso è tipicamente una luce orizzontale, le lenti polarizzate bloccano questa luce e lasciano passare solo quella verticale.

I **filtri UV** bloccano le frequenze di radiazione superiori a 780 nm, che non sono visibili ma interferiscono comunque con il modo in cui i sensori catturano un'immagine, mentre i **filtri a infrarossi** lasciano passare solo la luce infrarossa (<380 nm).

I **filtri di interferenza** sono filtri dicroici, il che significa che sono band-pass filters altamente selettivi che lasciano passare solo un colore di luce.

I **parametri di Stokes** sono un insieme di quattro valori che descrivono lo stato di polarizzazione della radiazione elettromagnetica, spesso combinati in un vettore noto come vettore di Stokes.
Sebbene un dato fascio di luce sia rappresentato da un vettore di Stokes unico, due fasci di luce con lo stesso vettore di Stokes non sono necessariamente uguali dal punto di vista ottico.

La **trasformata di Fourier** è uno strumento fondamentale nell'analisi delle immagini, poiché trasforma qualsiasi immagine dal dominio spaziale al dominio frequenziale. 
In particolare, l'immagine viene suddivisa nelle sue componenti seno e coseno.

La trasformata di Fourier di un'apertura circolare è un disco di diffrazione (disco di Airy) con anelli. Gli anelli di diffrazione potrebbero essere evitati (apodizzati) avendo un'apertura con una trasformata di Fourier uguale a se stessa: questo è il caso della gaussiana. Mettendo un filtro di gradazione basato sulla gaussiana (filtro apodizzante), possiamo far scomparire gli anelli.

La trasformata di Fourier è composta da ampiezza e fase. L'ampiezza può essere visualizzata con simmetria orizzontale e verticale con la frequenza 0 al centro. I low pass, high pass e band-pass filters possono essere progettati oscurando parte dell'ampiezza. 
L'ampiezza contiene principalmente informazioni sul colore, mentre la fase contiene informazioni sulla posizione. 
L'ampiezza di un'immagine costante è solo un punto, e l'ampiezza delle funzioni elementari sin/cos è solo 3 punti in una linea.
La traslazione non è significativa sulla grandezza perché le informazioni sulla posizione si trovano nell'immagine di fase, mentre la rotazione modifica anche la grandezza.
Una trasformazione di Fourier trasforma i segnali in somme di seni con ampiezze e fasi diverse → numeri complessi.

Al posto delle funzioni seno, possiamo usare le funzioni Mexican hat per ottenere le **wavelet**, un ibrido tra i metodi spaziali e quelli basati sulle trasformate di Fourier.
Le wavelet possono essere utilizzate per la nitidezza dell'immagine o la rimozione del rumore lavorando direttamente su determinate frequenze. Possiamo anche utilizzare le wavelet per il riconoscimento delle caratteristiche nelle immagini, poiché possono essere utilizzate per separare il rumore dal segnale: ad esempio, è possibile distinguere tra rumore casuale e stelle in una foto del cielo notturno.

La **deconvoluzione** cerca di recuperare l'immagine originale partendo dalla conoscenza del PSF. Sono possibili diverse soluzioni e il rumore può rendere impossibile il recupero di un'immagine, poiché piccoli errori nell'immagine rilevata producono grandi errori nell'originale restaurato.
L'idea di base della deconvoluzione è quella di eseguirla in modo iterativo partendo dall'immagine rumorosa e dalla conoscenza del PSF.
La deconvoluzione viene eseguita trovando l'inverso del PSF e convolvendolo con l'immagine originale. In pratica può essere impossibile trovare il vero PSF, ma è possibile approssimarlo.

# Image Segmentation 

Più importanti:

Morphology deals with the shape of 2D objects.

Morphological operators in image processing are used for noise filtering, shape simplification, skeletonizing, thinning and thickening, convex hull, object making, segmentation with respect to the background and describing objects by means of area, perimeter or other features. Morphological operators work with binary images.

Set operations like union, intersection, complement, difference, reflection and translation  are morphological operators as well.

Mixing fundamental set operations, we get more complex operators like dilation: Given sets A and B the dilation of A by B is obtained by reflecting B (called structure element) about its origin and then translating this reflection by x; dilation is then the set of all x displacements such as the reflection of B and A overlap by at least one nonzero element.

Dilation is useful to fix holes or cracks and to make contour lines smoother.

Dilation is commutative, associative, translation invariant, not invertible.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeyCGPjPwBlIEfbPptTIxwfcI2a9hhMEC12CMpELyrHlJpkHD6MnVSORwbk9jEm2q9oq8vqrqpp9IGMfZuEDXENzqEVmjyJarzi_jEMS0BA0NMm6l6WsAi9L_c44RMANdd1QVeuDgl0y91nkYnlhnjPIWY?key=IlMU9hpgd3pUHC0chA8ahA)

The Erosion operator works opposite to dilation: Given sets A and B the erosion of A by B is the set of all points x such that B, translated by x, is contained in A.

Erosion can be used to delete small spots (e.g. noise) or thin lines or small protrusions or to separate objects in order to count them.

It is translation invariant, not commutative, and cannot be inverted.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdk7RWoHNR-iFAHzdslMwjfuhp9yM3atjbM8-xpK4KxwOkRPazOZz0c_6qBPq2Js6xADvYx9f8E81a46CSteWq-PtBDq_cmpS-HlOWgkK5AAAef8ucQKHVPoScW1AnqclLtSwfpJrWi9ZkNqONp9ZRf_Ak?key=IlMU9hpgd3pUHC0chA8ahA)

In other words dilation adds pixels to the boundaries of objects in an image, while erosion removes pixels on object boundaries. The number of pixels added or removed from the objects in an image depends on the size and shape of the structuring element used to process the image.

Le operazioni di dilatazione e erosione sono irreversibili, quindi una volta eseguita la trasformazione non è possibile ottenere l’immagine di partenza.

  

Opening of A by B is the erosion of A by B, followed by a dilation of the result by B.

Since erosion deletes objects smaller than the mask it is possible to delete particular objects by using structures larger than them.

It can be used to smooth the contour of an image, break narrow links and delete thin protrusions.

Opening is idempotent: repeated applications maintain the same effect. 

  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcYmszI82734BigMctpuSNRo2wuKAT3RNUpYxBVGtN6lH3tmXZ0dirwudXnQ8dHnZJ2PwiUasT_D5T5lOVTYl-6SYHWyavP1qHWXKSgOeR4nJWmqhGPn0vOrpLiZ3t8pJvUTA3H8yA6ReQmWvqgJijyq58?key=IlMU9hpgd3pUHC0chA8ahA)

Closing is the dual of opening: of A by B is the dilation of A by B, followed by the erosion of the result by B.

Closing smooths contour of an image, fuses narrow breaks, eliminates small holes and fills gaps in the contour,

It is an idempotent operation.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfUaNl41nKq_h2HurnlEvhp6MbX5J2MfIU_NeVq4_5K6XdV5uU34hEN4uNyCLyp886gyfTqYVB9gvfPFlQoKSflxo5pimNSemiaLlYbAfye3338QoupKcFYebtnfs3aOdPsrA0o1MQP9TATmKzr1g6d1w?key=IlMU9hpgd3pUHC0chA8ahA)

  

Meno importanti:

The Hit-miss operator detects objects of a specific shape,  using the erosion operator and a pair of disjoint structuring elements. The first structuring element erodes with mask M1, the second erodes the background with mask M2, opposite to M1. The intersection of the two erosions gives all centre pixels of desired objects.

  

The boundary extraction operator is used to obtain the boundary of a set by first eroding A by the suitable structuring element B, and then doing set difference between A and its erosion.

Region filling is an iterative operator that applies dilation to a point inside a region until it fills it completely, to then do the union with the boundary.

The extraction of connected components is an operation that, using dilation iteratively, looks for all pixels connected to a starting pixel.

The convex hull of an arbitrary set S is the smallest convex set containing S. The set difference H-S is called the convex deficiency D of the set S. The algorithm that finds the convex hull of a set uses the union of four structural elements that iteratively apply the hit-miss operator to the original set.

Thinning of a set A by a structuring element B can be defined with hit-miss transforms:  the process is to thin A by one pass with B1 then thin the result with one pass of B2; after Bn the entire process is repeated until no further changes occur.

Thickening is the morphological dual of thinning and can be done via thinning the background.

  

In shape analysis, the skeleton of a shape is a thin version of that shape that is equidistant to its boundaries. The skeleton usually emphasises geometrical and topological properties of the shape, such as its connectivity, topology, length, direction, and width. The skeleton of a region is obtained by means of a thinning algorithm. A Skeleton S(A) can be expressed via erosions and openings.

  

Lastly, pruning is used as a complement to the skeleton and thinning algorithms to remove unwanted components.

  

# Object Recognition

Machine learning in computer vision is used to turn data into information by extracting rules or patterns.

Data is preprocessed into features that are arranged into patterns. Those patterns can be:

- Vectors, used for quantitative descriptions, like length, area, texture
    
- String Patterns, where each letter represents a primitive
    
- Tree Patterns, for structural description of the composition
    

  

Gli string pattern sono utilizzati per il riconoscimento di impronte digitali.

Features follow the rules of coverage, concision and directness: good features should ensure all relevant info is captured with their number minimised, and each feature should be independently useful for prediction.

  

The next step in machine learning is turning features into a model; either by classifying features with labels, or by clustering them into groups.

ML algorithms analyse our features and adjust weights, thresholds, and other parameters to maximise performance according to those goals. Parameter adjustments is what we mean by the term "learning".

To test how well a ML method works we break the data in a large training set and a smaller test set. After running the algorithm on the training set, we measure its performance on the test set.

  

Machine Learning Problems:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf_8HvvXbHbSrhPRFgzci_Sy1hMLi94-2cxEm_VeFtzP_TUX8VqaO4lIHnJi7BVNNj-NwDBi684IalFkOes79aMaq4_4keSMknzX3k6pX3bLh4WXXBAW_9zvkpGWPMl-L_sISNsDwApzJoKZJ6rqllhC54?key=IlMU9hpgd3pUHC0chA8ahA)

  

Clustering groups together similar points and represent them with a single token. This allows us to look at large amounts of data, apply patch-based compression or denoising, or represent a large continuous vector with the cluster number. Some usages of clustering are:

- Segmentation: separating the image into different coherent regions
    
- Counting histograms of texture, colour, SIFT vectors
    
- Prediction: Images in the same cluster may have the same labels
    

  

There are many different algorithms for clustering, one of the most used is K-means: a simple, fast heuristic algorithm that finds the natural cluster in the data by Iteratively re-assigning points to the nearest cluster centre. It doesn’t guarantee convergence to a global optimum. The k-means algorithm works by following these steps:  
1) Given a data set and a desired number of cluster k chosen by the user, randomly assign cluster centre locations (aka "means")  
2) Associate each data point with its nearest cluster centre, to create k cluster  
3) Move cluster centre to the centroid of their data points  
4) Step 2 and 3 are repeated until convergence has been reached  
  

Classification is the problem of labelling a new object based on a set containing already labelled instances. Training labels dictate if two examples are different, Classifiers try to learn weights or parameters for features and distance measures so that visual similarity predicts label similarity.

In general, it is better to find features that express some invariance in the objects.

  

Any decision rule divides the input space into decision regions separated by decision boundaries, the most used techniques for dividing the space are, for instance, K-nearest neighbour, Naïve Bayes, Decision trees, Haar/Viola-Jones classifier, Neural networks.

We define variance as how much models estimated from different training sets differ from each other, while bias is the difference between the correct value, which is what one would like to predict, and the average prediction.

A model is said to be underfitting when it is too "simple" to represent all the relevant class characteristics, while a model too “complex” that fits irrelevant characteristics (noise) in the data is said to be overfitting.

  

The No Free Lunch theorem states that no classifier is inherently better than any other, thus some assumptions are needed to generalise.

The errors in classification models are either inherent, and so unavoidable, due to oversimplification (bias) or due to the inability to perfectly estimate parameters from limited data (variance)

  

Classifiers fall in two categories: generative models and discriminative models:

Generative models give the distribution of the data given the label, and are good for giving more powerful representation of the data. 

Discriminative models give the probability of the label given the data, and are good for yearning predictions.

  

Bayes Classifiers follow a generative model. It is a probabilistic classifier, which means it predicts on the basis of the probability of an object and can be used to separate an image in different components.

  
  

Classifiers are also used for face recognition. For instance, the Haar Classifier is a tree-based technique suitable for "mostly rigid" objects like faces and the human body, but also cars, bikes etc.

Haar-like features are usually linked to edge features, line features or centre surround features, which are mostly specified by shape, position and scale.

The viola-jones classifier organises weak nodes of a rejection cascade using boosting: a classification scheme that combines weak learners into a more accurate ensemble classifier. Boosting is used to create binary classification nodes characterised by high detection and weak rejection.

In the first step of the boosting training procedure, each training example is weighted equally. In each boosting round we find the weak learner that achieves the lowest weighted training error and raise the weights of training examples misclassified by the current weak learner. The final classifier is computed as a linear combination of all weak learners, where the weight of each learner is directly proportional to its accuracy.

  

Face recognition and facial expression recognition have many different applications and as a consequence face different challenges, like illumination changes and white balancing, age, glasses, makeup, facial hair etc.

  

The Local Binary Pattern operator thresholds a 3x3 neighbourhood for each pixel, reading binary thresholded values as a label for each pixel. The histogram of the labels can then be used as a texture descriptor.

Principal Component Analysis is a method that uses eigenfaces: a reduced set of eigenfaces is computed, then a new face image is approximated by a weighted sum of the eigenfaces.

Linear Discriminant Analysis Tries to find a linear transformation by maximising the between-class variance and minimising within-class variance.

Elastic Bunch Graph Matching represents faces as graphs, where nodes are positioned at fiducial points and edges are labelled with distance vectors. Identification of a new face means determining among the constructed graphs the most similar in terms of graph similarity.

Bayesian Intra/extrapersonal Classifier uses Bayesian decision theory to divide the difference vectors between pairs of face images in intrapersonal differences and extrapersonal differences.

  

# Motion analysis and Object Reconstruction

Motion analysis can make use of spatial grey value changes to identify object or camera movements.

Unfortunately not all temporal grey value changes are due to motion, as some may depend for instance on illumination.

Local operators (like spatial and temporal derivatives) often can not determine the displacement unambiguously. This is known as the aperture problem. The correspondence problem is a generalisation of the aperture problem that states that physical correspondence can be not identical to visual correspondence.

Image sequence (video) analysis can be done considering it as a spatiotemporal image, where each pixel extends to a voxel with extensions x, y, t. This lets us reinterpret motion as orientation and study velocity analytically before a suitable discretization is applied. Furthermore, More than two images can be used to find orientation in space-time.

Having introduced the space-time domain, we can also analyse motion in the Fourier domain. In a sequence in which all the objects are moving with constant velocity it is possible to determine the slope of the plane on which the spectrum of the sequence is located.

Motion field is the real motion of the object in the 3D scene projected into the image plane, while Optical flow is the “flow” of grey values in the image plane. Motion field and optical flow are the same only if the objects do not change the irradiance on the image plane while moving in a scene.

Block Motion techniques are motion analysis methods used in video compression that aim to find the next position of an object and then compute the displacement vector. 

Motion analysis can also be done through camera calibration. To calibrate a camera we need to compute its intrinsic and extrinsic parameters. Extrinsic parameters are related to rotation-translation matrix and allow to describe motion of an object in front of a camera or motion of the camera.

In order to reconstruct large scenes it can be required to use a panoramic camera, in which the lens rotates around the rear node N2 with the film held static in a circular path centred in N2 and of radius f.

The angle of rotation however can’t be too large due to residual aberrations.

  

# Neural Networks

An Artificial Neural Network  is an adaptive system that learns by using interconnected nodes or neurons in a layered structure that resembles a human brain. A neural network can learn from data—so it can be trained to recognize patterns, classify data, and forecast future events.

Le reti neurali cercano di copiare il funzionamento del cervello umano.

Dentro ogni neurone c’è un componente che agisce come un trigger di Schmitt che agisce come soglia, sopra un certo valore viene considerato on e sotto viene considerato off. Durante il processo di apprendimento i recettori di polarità presenti all’interno dei neuroni vengono stimolati.

  

A Feed-Forward Neural Network, in its simplest form, is commonly seen as a single-layer Perceptron. It is composed of synapses and neurons. 

Synapses have a weighting and interconnection functions, while neurons make the linear combination between the inputs coming from the synapses. This linear combination can be described as follows: 

Y=(weights*inputs)+bias.

Weighs decide how much an input will affect the output. biases are constant and, alongside inputs, are fed into the next layer to ensure that even when all inputs are zero, there will still be activation in the neuron.

The output is then passed to an activation function, which is needed to decide if the neuron is activated or not.

Le reti neurali funzionano con dei pesi e con delle connessioni: tutto parte dai sensori che recepiscono le informazioni, seguito da un idle layer e seguito da un output layer.

  

Convolutional Neural Networks, unlike FeedForward ones, add the possibility of using three types of layers:

- Convolutional layers:  
    The fundamental element of a CNN that does most of the computation: it requires input data, a filter and a feature map. The filter or kernel moves through the input data (image) to see if a feature is present, calculating the scalar product and storing the result into an output array. The final output is known as a convolved feature, feature map or activation map. Convolutional layers have 3 parameters that can change their behaviour: stride is the distance the kernel moves across the input matrix (a larger stride produces a smaller output), The number of filters affects the depth of the output, and padding is a parameter that can be set in three states:  
    Valid padding, in which the last convolution is dropped if dimensions do not align.  
    Same padding, which guarantees that the output layer is the same size as the input layer  
    Full padding, which increments the output size by adding zeros to the edge of the input layer  
      
    
- Pooling layers:  
    also known as downsampling, reduces the number of parameters in the input. It works by scrolling an unweighted filter over the entire input. The two types of pooling are max pooling (chooses the pixel with the maximum value) and average pooling (computes the average).  
      
    
- Fully Connected Layers  
    In the fully connected layer, each node in the output layer connects straights to a node in the previous layer. It is usually used to perform the classification based on the features extracted in the previous layers and through different filters. 
    

  

During training,  the two metrics used to evaluate the performance of a model are accuracy, which indicates the fraction of predictions guessed by the model (with 1 = 100% accuracy) and loss, which indicates how bad the prediction of the model was on a single sample (with 0 = perfect prediction). Depending on the particular task we want to perform we can choose different loss functions, like binary cross-entropy for binary choices or categorical cross-entropy for multiple classes.

  

Backpropagation is a technique that attempts to minimise the loss function as much as possible in the learning process. It tells us how quickly the cost changes as weights and distortions change, then provides detailed information on how weight changes and distortions affect the overall behaviour of the network. It is calculated from the last layer of the network until the first.

  

For training a neural network, it is good practice to divide the input dataset into three parts:

- the training dataset, used to train the network
    
- the validation dataset, used to evaluate the network
    
- the test dataset, used to test the network in order to verify its capability to generalise on new, unseen data
    

  

Training a neural network requires a large amount of labelled data; the ground truth is the label that is assigned to a particular object by experts.

Fornendo troppi dati di uno stesso tipo causeremo un overfitting e, nel caso contrario, un underfitting, perché bisogna fornire un insieme di dati variegato per poter avere un buon riconoscimento. 

Hyperparameters are variables that must be set before the training and that determines how the network will be trained. The most important hyperparameters are:

- Learning rate:  
    Defines the update rate of the network; low values slow down the learning process but make it more fluid, while high values speed it up but increases the risk of not converging and overfitting 
    
- Samples:  
    Elements of the dataset
    
- Batch:  
    a set of N samples, processed independently and in parallel. During the training, a batch makes a single update of the network
    
- Epoch:  
    "a passage on the entire dataset", used to divide the training in more phases in order to make, for example, periodic evaluation and logging
    

  

Recurrent Neural Networks

A recurrent neural network (RNN) is a special type of an artificial neural network adapted to work for time series data or data that involves sequences. Ordinary feed forward neural networks are only meant for data points, which are independent of each other.

Recurrent neural networks add in their architecture backlinks and loops that allow information to persist. Neurons have an hidden state h, that carries information from previous events and overwrites itself uncontrollably at each step. An RNN cell is a recurring part of a network that maintains an internal state yt for each instant, and can be considered as a sort of network layer itself. 

Despite all the advantages, RNNs suffer from many problems.

  

Long-short term memory networks are a special kind of RNN that attempt to solve some of these problems by introducing the ability to learn long-term dependencies at the cost of adding additional gates. In particular, they control what information in the hidden cell is exported as output and to the next hidden state.

A basic LSTM unit is composed of:

- A memory cell, which has the purpose of recording additional information with respect to the hidden state
    
- A forget gate, which  decides when to remember and when to ignore inputs in the hidden state
    
- An input gate
    
- An output gate
    

  

A technology similar to that of the LSTM network is the Gated Recurrent Unit (GRU). Which with respect to the LSTM only has update and reset gates and no internal memory. It is usually best to use when modelling not-so-long distance relations, since using fewer training parameters makes it perform faster and use less memory.

  
Le reti neurali moderne hanno capacità sovrumane perché sono più precise degli umani nel riconoscimento delle persone.

  

Neural networks used to solve  classification problems all belong to the discriminative model. 

  

The neural networks we’ve studied until now are not able to synthesise new data in a realistic way. Generative models attempt to do this by learning the data distribution p(x) in order to generate new samples.

Generative models use a latent space, which means that similar samples from the outside world are placed next to each other in a multidimensional space for the internal representation of observed external events.

  

Generative Adversarial Networks (GANs) are based on the simultaneous use of generative and discriminative neural networks in competition.

In this competition, generative neural networks generate candidates, while the discriminative one  tries to distinguish the false candidates produced by the generator from the true data distribution.

  

# OpenCV

GIMP

Analisi istogramma con GIMP: Colors>Info>Histogram

Equalizzazione istogramma con GIMP: Colors>Auto>Equalize or Colors>Curves

Sogliatura con GIMP: Threshold or Curve

Contrasto e Luminosità con GIMP: Brightness-Contrast

  

COMANDI OPENCV

Apri immagine:

 `cv2.imread("esempio.jpg")`

Cambia Spazio Colori: 

`cv2.cvtColor(img, cv2.COLOR_BGR2RGB)`

Mostra l’immagine: 

`plt.imshow(rgb_img)`

`plt.show()`

Cambia un canale con un valore:

`img_[:, :, 0] = NewValue`

Calcola l’istogramma:  
`cv2.calcHist([img], [0], None, [256], [0, 256])`

Calcolo della trasformata di fourier:

```
cv2.dft(np.float32(img), flags=cv2.DFT_COMPLEX_OUTPUT) //dft
dft_shift = np.fft.fftshift(dft) //reorder the quadrants
cv2.magnitude(dft_shift[:, :, 0], dft_shift[:, :, 1]) //compute magnitude
np.log(img_magnitude) //move to log scale
```

Calcolo della trasformata inversa:
```
cv2.idft(dft) //idft
cv2.magnitude(img_back[:, :, 0], img_back[:, :, 1]) 
```

Creare una maschera
```
mask = np.zeros((rows, cols, 2), np.uint8)
```

Filtro di Sobel
```
cv2.Sobel(frame, cv2.CV_16S, 1, 0, ksize=3)
```

Erosione
```
cv2.erode(img, kernel, iterations=2)
```
Dilatazione
```
cv2.dilate(img, kernel, iterations=3)
```
  

- Riportare le operazioni principali, in OpenCV, per calcolare la trasformata di Fourier di un'immagine RGB rappresentante la funzione seno (4 cicli, orizzontale). Qual è l'immagine risultante dalla trasformazione e cosa rappresenta?
    
```
img = cv2.imread("images/sinFunction.png", cv2.IMREAD_GRAYSCALE)

# convert the image in float32 and transform

# DFT_COMPLEX_OUTPUT: the output is a complex matrix of the same #size as input

dft = cv2.dft(np.float32(img), flags=cv2.DFT_COMPLEX_OUTPUT)

 # re-order the quadrants (put the zero frequency component in the middle of #the image)

 dft_shift = np.fft.fftshift(dft)

img_magnitude = cv2.magnitude(dft_shift[:, :, 0], dft_shift[:, :, 1])

# if np.log() gives a warning and you want to get rid of it, just add 1 to avoid #log(0)

img_magnitude += 1

magnitude_log = 20*np.log(img_magnitude)
```
  

L’immagine risultante è una linea orizzontale con tre punti che rappresenta la magnitudine della funzione seno.

- Due oggetti, denominati "background" e "logo", contengono due immagini. Il "logo" è in scala di grigio mentre background è in BGR. Inoltre, "logo" è il triplo di "background". Si vuole aggiungere, tramite linear blending, "logo" nell'angolo in basso a sinistra di "background", in modo che occupi 1/4 di "background". Si descrivano i passi principali in OpenCV per eseguire questa operazione, riportando i metodi essenziali da utilizzare
    

  
Carichiamo il logo tramite logo = cv2.imread(“logo.png”); Come prima cosa bisogna trasformare logo da scala di grigi in RGB.  
Carichiamo anche il background. Per eseguire il linear blending si usa il metodo di cv2 addWeighted(src1, alpha, src2, beta, 0.0), con src1 il background, alpha=1, src2 il logo e beta = 1/12 (?) .

  
  

# Esempi domande e risposte esami

- Spiegare il significato dell’MTF e illustrare l’MTF dell’occhio umano:
    

La MTF è la modular transfer function, la quale rappresenta l’ampiezza (magnitudine) dell’OTF. E’ la misura di quanto bene un sistema può riprodurre l’input, si può misurare tramite un segnale sinusoidale o tramite uno slant target; in particolare rappresenta il rapporto tra la modulazione di output e quella di input. Per il principio di conservazione di energia l’MTF dovrebbe sempre essere minore di uno perché l’output non può essere più grande dell’input. Nel caso dell’occhio umano si ha un MTF maggiore se l’area dell’immagine è più grande.

  

- Indicare le modalità per la modifica del contrasto a livello di funzione di trasferimento. Qual è la differenza tra micro e macro contrasto?
    

Il macrocontrasto è definito sull’intera immagine, il microcontrasto è definito su una porzione ristretta.

  

- Quali sono i principali operatori morfologici? 
    

Gli operatori morfologici sono usati per il filtraggio del rumore, semplificazione delle forme, thinning, thickening, etc. I principali sono l’erosione il cui compito è quello di eliminare le piccole protrusioni nei bordi di una forma o di separare gli oggetti, la dilatazione che può essere considerato come l’opposto dell’erosione perché riempie i buchi nell’immagine per far apparire l’immagine più continua. Da questi due primi operatori dipendono le operazioni di opening, che applica prima un'erosione e poi una dilatazione per regolare i contorni di un’immagine, e di closing che applica prima una dilatazione e poi un’erosione con l’obiettivo di riempire i buchi nell’immagine e nel suo contorno. Altri operatori morfologici sono hit and miss, boundary extraction, region filling, convex hull, thinning, thickening, skeleton e pruning.

  

- In che modo si può contrastare l’aliasing nelle fotocamere a colori?
    

L'aliasing nelle fotocamere a colori si può risolvere tramite il Bayer drizzling, tramite cui vengono scattate più foto spostandosi ogni volta lungo una direzione diversa e, infine, vengono riunite in un'unica immagine.

  

- Spiegare il principio di Scheimpflug e la sua utilità a livello industriale.
    

Il principio di Scheimpflug è utilizzato per correggere i problemi riguardanti la profondità di campo in fotografia. Secondo Scheimpflug per poter avere una profondità di campo distribuita sulla scena bisogna far incontrare le rette su cui giacciono  la fotocamera (o meglio il suo sensore), il soggetto e il piano focale in un unico punto.

  

- Illustrare il classificatore di Bayes.
    

Il classificatore di Bayes può essere usato per determinare le probabilità delle classi alla luce di una serie di osservazioni. Grazie a questo classificatore è possibile, ad esempio, dividere un’immagine in più parti.

  

- Illustrare, anche mediante un esempio, l’effetto dell’operatore morfologico di opening
    

L’operatore morfologico di opening consiste nell’applicazione di un’erosione seguita da una dilatazione. Questo operatore può essere utilizzato, ad esempio, per aprire uno spazio fra gli oggetti collegati tramite un piccolo ponte; per fare questo l’erosione elimina i pixel corrispondenti al ponte tra i due oggetti e poi la dilatazione riporta gli oggetti sopravvissuti alla prima operazione alla dimensione originale. 

  

- Spiegare il concetto di ISO invarianza.  
    L’ISO invarianza è una proprietà del sensore della fotocamera grazie al quale il rumore non aumenta quando si cambia l’ISO. Nessuna fotocamera, però, è perfettamente ISO invariante.
    

  

- Definire il concetto di contrasto e spiegare come esso può influenzare la qualità delle immagini riprese.
    

 Il contrasto rappresenta la differenza relativa all’interno dell’immagine fra i livelli massimo e minimo di intensità. Più formalmente, detti Lmax ed Lmin il massimo ed il minimo rispettivamente, si definisce il contrasto C:

C = (Lmax-Lmin)/(Lmax+Lmin)

Quando viene calcolato sull’intera immagine si parla di macrocontrasto, altrimenti se calcolato su una porzione dell’immagine si parla di microcontrasto. Il contrasto è una misura di quanto è possibile distinguere variazioni di intensità luminosa. Immagini a basso contrasto avranno un aspetto maggiormente ‘slavato’, con una generale difficoltà a distinguere i dettagli, e presentano un istogramma concentrato su pochi livelli di luminosità. Al contrario, immagini ad alto contrasto presentano dettagli ben distinguibili a livello visivo e un istogramma ben distribuito lungo i diversi livelli di luminosità.

  

- Spiegare cos’è il disco di Airy e qual è il suo ruolo nella ripresa e gestione delle immagini.  
    Il disco di Airy è il pattern di diffrazione che si genera quando la luce attraversa un diaframma circolare uniformemente illuminato. Analogamente, esso può essere visto come la risposta all’impulso (PSF) di un sistema ottico a singola lente ideale. Il disco di Airy disperde la luce puntiforme lungo un disco la cui ampiezza va come sinc(r), e si può dimostrare che la quasi totalità dell’energia è concentrata entro un diametro di: 𝐷 = 2.44𝜆𝑓# Dove λ è la lunghezza d’onda incidente e f# l’apertura del diaframma (in f-stop). Essendo il più piccolo dettaglio risolvibile dall’ottica, è necessario che esso sia campionato opportunamente per evitare problemi di aliasing, con almeno 3-4 pixel che ne coprano il diametro.
    

  
  
  
  
  
  
**