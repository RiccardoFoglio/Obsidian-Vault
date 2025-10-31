
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

La morfologia si occupa della forma degli oggetti 2D.

Gli operatori morfologici nell'elaborazione delle immagini vengono utilizzati per: noise filtering, shape simplification, skeletonizing, thinning and thickening, convex hull, object masking, segmentation wrt background e la descrizione degli oggetti mediante area, perimetro o altre caratteristiche. Gli operatori morfologici funzionano con immagini binarie.

Anche operazioni su insiemi come unione, intersezione, complemento, differenza, riflessione e traslazione sono operatori morfologici.

Mescolando operazioni fondamentali sugli insiemi, otteniamo operatori più complessi come la dilatazione: dati gli insiemi A e B, la dilatazione di A per B si ottiene riflettendo B (chiamato structure element) rispetto alla sua origine e poi traslando questa riflessione di x; la dilatazione è quindi l'insieme di tutti gli spostamenti x tali che la riflessione di B e A si sovrappongono per almeno un elemento diverso da zero.

La dilatazione è utile per correggere fori o crepe e per rendere più uniformi le linee di contorno.
La dilatazione è commutativa, associativa, invariante rispetto alla traslazione, non invertibile.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeyCGPjPwBlIEfbPptTIxwfcI2a9hhMEC12CMpELyrHlJpkHD6MnVSORwbk9jEm2q9oq8vqrqpp9IGMfZuEDXENzqEVmjyJarzi_jEMS0BA0NMm6l6WsAi9L_c44RMANdd1QVeuDgl0y91nkYnlhnjPIWY?key=IlMU9hpgd3pUHC0chA8ahA)

L'operatore Erosione funziona in modo opposto alla dilatazione: dati gli insiemi A e B, l'erosione di A da parte di B è l'insieme di tutti i punti x tali che B, traslato di x, è contenuto in A.

L'erosione può essere utilizzata per eliminare piccoli punti (ad esempio rumore), linee sottili o piccole sporgenze, oppure per separare oggetti al fine di contarli.
È invariante rispetto alla traslazione, non commutativa e non può essere invertita.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdk7RWoHNR-iFAHzdslMwjfuhp9yM3atjbM8-xpK4KxwOkRPazOZz0c_6qBPq2Js6xADvYx9f8E81a46CSteWq-PtBDq_cmpS-HlOWgkK5AAAef8ucQKHVPoScW1AnqclLtSwfpJrWi9ZkNqONp9ZRf_Ak?key=IlMU9hpgd3pUHC0chA8ahA)

In altre parole, la dilatazione aggiunge pixel ai contorni degli oggetti in un'immagine, mentre l'erosione rimuove pixel dai contorni degli oggetti. Il numero di pixel aggiunti o rimossi dagli oggetti in un'immagine dipende dalle dimensioni e dalla forma dell'elemento strutturante utilizzato per elaborare l'immagine.

Le operazioni di dilatazione ed erosione sono irreversibili, quindi una volta eseguita la trasformazione non è possibile ottenere l'immagine di partenza.


L'apertura di A da parte di B è l'erosione di A da parte di B, seguita da una dilatazione del risultato da parte di B.
Poiché l'erosione elimina gli oggetti più piccoli della maschera, è possibile eliminare oggetti particolari utilizzando strutture più grandi di essi.
Può essere utilizzata per smussare il contorno di un'immagine, rompere collegamenti stretti ed eliminare sporgenze sottili.
L'apertura è idempotent: applicazioni ripetute mantengono lo stesso effetto.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcYmszI82734BigMctpuSNRo2wuKAT3RNUpYxBVGtN6lH3tmXZ0dirwudXnQ8dHnZJ2PwiUasT_D5T5lOVTYl-6SYHWyavP1qHWXKSgOeR4nJWmqhGPn0vOrpLiZ3t8pJvUTA3H8yA6ReQmWvqgJijyq58?key=IlMU9hpgd3pUHC0chA8ahA)

La chiusura è il duale dell'apertura: di A per B è la dilatazione di A per B, seguita dall'erosione del risultato per B.
La chiusura leviga il contorno di un'immagine, fonde le interruzioni strette, elimina i piccoli fori e riempie gli spazi vuoti nel contorno.
È un'operazione idempotente.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfUaNl41nKq_h2HurnlEvhp6MbX5J2MfIU_NeVq4_5K6XdV5uU34hEN4uNyCLyp886gyfTqYVB9gvfPFlQoKSflxo5pimNSemiaLlYbAfye3338QoupKcFYebtnfs3aOdPsrA0o1MQP9TATmKzr1g6d1w?key=IlMU9hpgd3pUHC0chA8ahA)


Meno importanti:

L'**hit-miss operator** rileva oggetti di una forma specifica, utilizzando l'operatore di erosione e una coppia di elementi strutturanti disgiunti. 
Il primo elemento strutturante erode con la maschera M1, il secondo erode lo sfondo con la maschera M2, opposta a M1. 
L'intersezione delle due erosioni fornisce tutti i pixel centrali degli oggetti desiderati.


Il **boundary extraction operator** viene utilizzato per ottenere il confine di un insieme erodendo prima A con l'elemento strutturante B appropriato, quindi eseguendo la differenza tra A e la sua erosione.

**Region Filling** è un operatore iterativo che applica la dilatazione a un punto all'interno di una regione fino a riempirla completamente, per poi eseguire l'unione con il confine.

L'estrazione dei componenti connessi è un'operazione che, utilizzando la dilatazione in modo iterativo, cerca tutti i pixel connessi a un pixel di partenza.

Il **Convex Hull** di un insieme arbitrario S è il più piccolo insieme convesso che contiene S. La differenza tra gli insiemi H-S è chiamata deficienza convessa D dell'insieme S. L'algoritmo che trova l'involucro convesso di un insieme utilizza l'unione di quattro elementi strutturali che applicano in modo iterativo l'operatore hit-miss all'insieme originale.

Il **Thinning** di un insieme A mediante un elemento strutturante B può essere definito con trasformazioni hit-miss: il processo consiste nel diradare A con un passaggio di B1, quindi diradare il risultato con un passaggio di B2; dopo Bn l'intero processo viene ripetuto fino a quando non si verificano ulteriori cambiamenti.

Il **Thickening** è il duale morfologico del thinning e può essere effettuato tramite il diradamento dello sfondo.


Nella **shape analysis**, lo **scheletro** di una forma è una versione assottigliata di quella forma che è equidistante dai suoi confini. Lo scheletro di solito enfatizza le proprietà geometriche e topologiche della forma, come la sua connettività, topologia, lunghezza, direzione e larghezza. Lo scheletro di una regione si ottiene mediante un algoritmo di assottigliamento. 
Uno scheletro S(A) può essere espresso tramite erosioni e aperture.

Infine, il **pruning** (potatura) viene utilizzata come complemento agli algoritmi dello scheletro e del diradamento per rimuovere i componenti indesiderati.

# Object Recognition

Il **Machine Learning** nella visione artificiale viene utilizzato per trasformare i dati in informazioni estraendo regole o modelli.

I dati vengono pre-elaborati in features che vengono organizzate in patterns. 
Questi modelli possono essere:
- <font color="#00b0f0">Vettori</font>, utilizzati per descrizioni quantitative, come lunghezza, area, consistenza
- <font color="#00b0f0">String Patterns</font>, in cui ogni lettera rappresenta un elemento primitivo
- <font color="#00b0f0">Tree Patterns</font>, per la descrizione strutturale della composizione

Gli string pattern sono utilizzati per il riconoscimento delle impronte digitali.

Le caratteristiche seguono le regole di copertura, concisione e immediatezza: le caratteristiche ottimali dovrebbero garantire che tutte le informazioni rilevanti siano acquisite con il minor numero possibile di dati e che ciascuna caratteristica sia utile in modo indipendente per la previsione.

Il passo successivo nel machine learning consiste nel trasformare le caratteristiche in un modello, classificandole con etichette o raggruppandole in cluster.

Gli algoritmi di ML analizzano le nostre caratteristiche e regolano pesi, soglie e altri parametri per massimizzare le prestazioni in base a tali obiettivi. 

*La regolazione dei parametri è ciò che intendiamo con il termine “learning”*

Per testare l'efficacia di un metodo di ML, suddividiamo i dati in un ampio set di addestramento e un set di test più piccolo. Dopo aver eseguito l'algoritmo sul set di addestramento, ne misuriamo le prestazioni sul set di test.

Problemi di apprendimento automatico:
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf_8HvvXbHbSrhPRFgzci_Sy1hMLi94-2cxEm_VeFtzP_TUX8VqaO4lIHnJi7BVNNj-NwDBi684IalFkOes79aMaq4_4keSMknzX3k6pX3bLh4WXXBAW_9zvkpGWPMl-L_sISNsDwApzJoKZJ6rqllhC54?key=IlMU9hpgd3pUHC0chA8ahA)

Il **clustering** raggruppa punti simili e li rappresenta con un unico token. Questo ci permette di esaminare grandi quantità di dati, applicare la compressione o il denoising basati su patch, o rappresentare un grande vettore continuo con il numero del cluster. Alcuni utilizzi del clustering sono:
- <font color="#00b0f0">Segmentation</font>: separare l'immagine in diverse regioni coerenti
- <font color="#00b0f0">Counting</font> degli istogrammi di texture, colore, vettori SIF
- <font color="#00b0f0">Prediction</font>: le immagini nello stesso cluster possono avere le stesse etichette

Esistono molti algoritmi diversi per il clustering, uno dei più utilizzati è il **K-means**: un algoritmo euristico semplice e veloce che individua il cluster naturale nei dati riassegnando iterativamente i punti al centro del cluster più vicino. 
Non garantisce la convergenza verso un optimum globale. 
L'algoritmo k-means funziona seguendo questi passaggi:  
1) Dato un set di dati e un numero desiderato di cluster k scelto dall'utente, assegnare in modo casuale le posizioni dei centri dei cluster (noti anche come "means")  
2) Associare ogni punto dati al centro del cluster più vicino, per creare k cluster  
3) Spostare il centro del cluster al centroide dei propri punti dati  
4) I passaggi 2 e 3 vengono ripetuti fino al raggiungimento della convergenza

La **Classification** è il problema di etichettare un nuovo oggetto sulla base di un insieme contenente istanze già etichettate. 
Le Training Labels determinano se due esempi sono diversi, i classificatori cercano di apprendere pesi o parametri per le caratteristiche e le misure di distanza in modo che la somiglianza visiva predica la somiglianza delle etichette.
In generale, è meglio trovare caratteristiche che esprimano una certa invarianza negli oggetti.

Qualsiasi regola decisionale divide l'input space in regioni decisionali separate da confini decisionali. Le tecniche più utilizzate per dividere lo spazio sono: 
K-nearest neighbour, Naïve Bayes, Decision trees, Haar/Viola-Jones classifier, Neural networks.

Definiamo la **varianza** come la differenza tra i modelli stimati da diversi set di addestramento, mentre il **bias** è la differenza tra il valore corretto, che è quello che si vorrebbe prevedere, e la previsione media.

Si dice che un modello è sottodimensionato quando è troppo "semplice" per rappresentare tutte le caratteristiche rilevanti della classe, mentre un modello troppo "complesso" che si adatta a caratteristiche irrilevanti (rumore) nei dati è detto sovradimensionato.

  
Il teorema <font color="#ff0000">No Free Lunch</font> afferma che nessun classificatore è intrinsecamente migliore di un altro, pertanto sono necessarie alcune ipotesi per generalizzare.
Gli errori nei modelli di classificazione sono intrinseci, e quindi inevitabili, a causa di un'eccessiva semplificazione (distorsione) o dell'incapacità di stimare perfettamente i parametri da dati limitati (varianza).

I classificatori si dividono in due categorie: generative models e discriminative models:
- I <font color="#00b050">generative models</font> forniscono la distribuzione dei dati in base all'etichetta e sono utili per fornire una rappresentazione più potente dei dati. 
- I <font color="#00b050">discriminative models</font> forniscono la probabilità dell'etichetta in base ai dati e sono utili per le previsioni.

**I classificatori Bayes** seguono un modello generativo. 
Si tratta di un classificatore probabilistico, il che significa che effettua previsioni sulla base della probabilità di un oggetto e può essere utilizzato per separare un'immagine in diversi componenti.

I classificatori vengono utilizzati anche per il riconoscimento facciale. 
Ad esempio, il **classificatore Haar** è una tecnica basata su alberi adatta per oggetti "prevalentemente rigidi" come i volti e il corpo umano, ma anche automobili, biciclette ecc.
Le caratteristiche simili a Haar sono solitamente collegate a caratteristiche di bordo, caratteristiche di linea o caratteristiche di centro circostante, che sono per lo più specificate da forma, posizione e scala.

Il **classificatore Viola-Jones** organizza i nodi deboli di una cascata di rifiuto utilizzando il **boosting**: uno schema di classificazione che combina learner deboli in un classificatore ensemble più accurato. Il boosting viene utilizzato per creare nodi di classificazione binaria caratterizzati da un elevato rilevamento e un rifiuto debole.

Nella prima fase della procedura di addestramento del boosting, ogni esempio di addestramento viene ponderato in modo uguale. In ogni round di boosting troviamo il learner debole che ottiene l'errore di addestramento ponderato più basso e aumentiamo i pesi degli esempi di addestramento classificati erroneamente dal learner debole attuale. Il classificatore finale viene calcolato come una combinazione lineare di tutti i learner deboli, dove il peso di ciascun learner è direttamente proporzionale alla sua accuratezza.

  
Il **riconoscimento facciale** e il **riconoscimento delle espressioni facciali** hanno molte applicazioni diverse e, di conseguenza, devono affrontare diverse sfide, come i cambiamenti di illuminazione e il bilanciamento del bianco, l'età, gli occhiali, il trucco, i peli sul viso, ecc.
  

L'operatore **Local Binary Pattern** applica un threshold (soglia) ad un 3x3 vicino per ciascun pixel, leggendo i valori binari soglia come etichetta per ciascun pixel. 
L'istogramma delle etichette può quindi essere utilizzato come descrittore di texture.

La **Principal Component Analysis** è un metodo che utilizza gli eigenfaces: viene calcolato un insieme ridotto di eigenfaces, quindi una nuova immagine del volto viene approssimata mediante una somma ponderata degli eigenfaces.

La **Linear Discriminant Analysis** cerca di trovare una trasformazione lineare massimizzando la varianza tra le classi e minimizzando la varianza all'interno delle classi.

L'**Elastic Bunch Graph Matching** rappresenta i volti come grafici, in cui i nodi sono posizionati in punti fiduciali e i bordi sono etichettati con vettori di distanza. L'identificazione di un nuovo volto significa determinare, tra i grafici costruiti, quello più simile in termini di somiglianza grafica.

Il **Bayesian Intra/Extrapersonal Classifier** utilizza la teoria decisionale bayesiana per dividere i vettori di differenza tra coppie di immagini facciali in differenze intrapersonali e differenze extrapersonali.
# Motion analysis and Object Reconstruction

La **Motion Analysis** può utilizzare le variazioni spaziali del valore di grigio per identificare i movimenti di oggetti o telecamere.
Purtroppo, non tutte le variazioni temporali del valore di grigio sono dovute al movimento, poiché alcune possono dipendere, ad esempio, dall'illuminazione.

Gli operatori locali (come le derivate spaziali e temporali) spesso non sono in grado di determinare lo spostamento in modo univoco. Questo fenomeno è noto come **problema dell'apertura**. 
Il **problema della corrispondenza** è una generalizzazione del problema dell'apertura che afferma che la corrispondenza fisica può non essere identica alla corrispondenza visiva.

L'analisi della sequenza di immagini (video) può essere effettuata considerandola come un'immagine spazio-temporale, in cui ogni pixel si estende a un voxel con estensioni x, y, t. 
Ciò ci consente di reinterpretare il movimento come orientamento e di studiare analiticamente la velocità prima di applicare una discretizzazione adeguata. 
Inoltre, è possibile utilizzare più di due immagini per trovare l'orientamento nello spazio-tempo.

Dopo aver introdotto il dominio spazio-temporale, possiamo anche analizzare il movimento nel dominio di Fourier. In una sequenza in cui tutti gli oggetti si muovono a velocità costante, è possibile determinare la pendenza del piano su cui si trova lo spettro della sequenza.

Il **Motion Field** è il movimento reale dell'oggetto nella scena 3D proiettato sul piano dell'immagine, mentre l'**Optical Flow** è il "flusso" dei valori di grigio nel piano dell'immagine. 
Il campo di movimento e il flusso ottico sono uguali solo se gli oggetti non modificano l'irradianza sul piano dell'immagine mentre si muovono in una scena.

Le tecniche **Block Motion** sono metodi di analisi del movimento utilizzati nella compressione video che mirano a trovare la posizione successiva di un oggetto e quindi a calcolare il vettore di spostamento. 

L'analisi del movimento può essere effettuata anche attraverso la **camera calibration**. 
Per calibrare una telecamera è necessario calcolarne i parametri intrinseci ed estrinseci. 
I parametri estrinseci sono correlati alla matrice di rotazione-traslazione e consentono di descrivere il movimento di un oggetto davanti a una telecamera o il movimento della telecamera stessa.

Per ricostruire scene di grandi dimensioni può essere necessario utilizzare una telecamera panoramica, in cui l'obiettivo ruota attorno al nodo posteriore N2 con la pellicola mantenuta statica in un percorso circolare centrato in N2 e di raggio f.
L'angolo di rotazione, tuttavia, non può essere troppo grande a causa delle aberrazioni residue.

# OpenCV

GIMP
Analisi istogramma con GIMP: Colors>Info>Histogram
Equalizzazione istogramma con GIMP: Colors>Auto>Equalize or Colors>Curves
Sogliatura con GIMP: Threshold or Curve
Contrasto e Luminosità con GIMP: Brightness-Contrast

COMANDI OPENCV

Apri immagine: `cv2.imread("esempio.jpg")`

Cambia Spazio Colori: `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)`

Mostra l’immagine:  `plt.imshow(rgb_img)` `plt.show()`

Cambia un canale con un valore: `img_[:, :, 0] = NewValue`

Calcola l’istogramma:   `cv2.calcHist([img], [0], None, [256], [0, 256])`

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

- Riportare le operazioni principali, in OpenCV, per calcolare la trasformata di Fourier di un'immagine RGB rappresentante la funzione seno (4 cicli, orizzontale). Qual è l'immagine risultante dalla trasformazione e cosa rappresenta?
  ```python
  img = cv2.imread("images/sinFunction.png", cv2.IMREAD_GRAYSCALE)
  # convert the image in float32 and transform
  # DFT_COMPLEX_OUTPUT: the output is a complex matrix of the same #size as input
  dft = cv2.dft(np.float32(img), flags=cv2.DFT_COMPLEX_OUTPUT)
  # re-order the quadrants (put the zero frequency component in the middle of
  #the image)
  dft_shift = np.fft.fftshift(dft)
  img_magnitude = cv2.magnitude(dft_shift[:, :, 0], dft_shift[:, :, 1])
  # if np.log() gives a warning and you want to get rid of it, just add 1 to
  # avoid #log(0)
  img_magnitude += 1
  magnitude_log = 20*np.log(img_magnitude)
  ```
  L’immagine risultante è una linea orizzontale con tre punti che rappresenta la magnitudine della funzione seno.


- Due oggetti, denominati "background" e "logo", contengono due immagini. Il "logo" è in scala di grigio mentre background è in BGR. Inoltre, "logo" è il triplo di "background". Si vuole aggiungere, tramite linear blending, "logo" nell'angolo in basso a sinistra di "background", in modo che occupi 1/4 di "background". Si descrivano i passi principali in OpenCV per eseguire questa operazione, riportando i metodi essenziali da utilizzare
  
  Carichiamo il logo tramite `logo = cv2.imread(“logo.png”);` 
  Come prima cosa bisogna trasformare logo da scala di grigi in RGB.  
  Carichiamo anche il background. 
  Per eseguire il linear blending si usa il metodo di cv2 `addWeighted(src1, alpha, src2, beta, 0.0)` 
  con src1 il background, alpha=1, src2 il logo e beta = 1/12 (?) .

  
