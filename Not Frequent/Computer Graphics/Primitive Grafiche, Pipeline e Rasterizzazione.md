Grafica Raster: primitive grafiche di output memorizzate in un buffer in termini di pixel.
Immagine sullo schermo formata a partire dal raster, insieme di linee di scansione (scan line) orizzontali composte da diversi pixel.
Raster memorizzato come una matrice di pixel che rappresenta l'intera area dello schermo.
Letto (scandito) in maniera sequenziale dal sottosistema per controllo dello schermo

Svantaggi: natura discreta della rappresentazione basata sui pixel: coordinate dei punti estremi devono essere convertiti in coordinate raster
anche la natura del raster causa problemi: sistema vettoriale può disegnare linee continue, il raster solo approssimazioni.
Aliasing = artefatto visivo frutto di un errore di campionamento / sotto-campionamento 

Due tecniche per preparare i contenuti per la visualizzazione:
- **Rasterizzazione** : per ogni primitiva = insieme di pixel accesi, colorati, illuminati, per disegnare. Veloce, complicato raggiungere risultati fotorealistici, perfetta per grafica vettoriale, font ecc..
- **Raytracing** : per ogni pixel = insieme di primitive, più facile raggiungere risultati fotorealistici, generalmente più lento

Pipeline: in ingresso, l'immagine si vuole disegnare nel formato opportuno. Stadi: sequenza di trasformazioni per passare dall'input all'output. In uscita, l'immagine disegnata.
![[Pasted image 20241024183637.png]]

I sistemi moderni di visualizzazione di immagini in tempo reale sono basati su rasterizzazione.
Ingresso: primitive 3D
Uscita: immagine raster, bitmap
## Librerie/API
Librerie/API (OpenGL/Direct3D) approssimano primitive matematiche (ideali) in input, descritte in termini di vertici in un sistema di coordinate cartesiane.
Con un insieme di pixel dell'intensità appropriata memorizzati in una bitmap o pixmap (nella memoria della CPU o nel frame buffer)
Algoritmi per **convertire le primitive** in un **pixel** ed effettuare il **clipping** rispetto al rettangolo di clip
![[Pasted image 20241022123943.png]]

Algoritmi di scan conversion e clipping sono indipendenti dalla tecnologia del dispositivo raster di output. Adattabili ad implementazioni sia software che hardware.
Per ogni primitiva di output incontrata le librerie effettuano la scan conversion.
Devono creare immagini della qualità appropriata ma anche essere il più veloci possibile.

## Sistemi di Visualizzazione

- libreria grafica come sistema in grado di mediare tra applicazione e hardware

- interfaccia verso l'hardware è indipendente dal dispositivo (pipeline I/O)

- frame buffer, senza controllore grafico: librerie effettuano scan conversion sia fuori dallo schermo che nel frame buffer. Organizzazione tipica in questo caso: memoria condivisa, comprende librerie, altro software e dati
![[Pasted image 20241024184352.png]]

- con frame buffer e controllore grafico: controllore implementa scan conversion e si occupa delle primitive. minimo sforzo delle librerie (solo conversione dalla rappresentazione interna ai formati accettati dall'hardware)
![[Pasted image 20241024184502.png]]

- Sistemi di sola visualizzazione: più semplici, accettano una scanline per volta ed utilizzano approcci opportuni per fornire la scanline quando serve.
  Dispositivi più intelligenti: accettano un intero fotogramma/pagina
### Pipeline di Output
modi diversi di effettuare il clipping
- clip prima della scan conversion calcolando le intersezioni con i confini della regione di clipping
- "forza bruta": scissoring dopo la scan conversion scritti solo i pixel visibili
- generare intera collezione di primitive e usare copyPixel sul rettangolo di clip

## Rasterizzazione
### Linee
Si necessita un algoritmo che calcoli le coordinate dei pixel che giacciono su una linea retta ideal, infinitamente sottile, sovrapposta ad una matrice raster 2D
Si vorrebbe che: la sequenza di pixel fosse vicina alla linea ideale e la più dritta possibile e che il processo fosse il più veloce possibile
![[Pasted image 20241022125333.png|400]]

- Gestione linee di spessore superiore ad 1 pixel centrate sulla linea ideale
- gestione attributi per cambiare lo stile / effetti
- minimizzare artefatti dovuti all'approssimazione discreta --> tecniche di antialiasing basate sulla possibilità degli schermi a *n* bit per pixel di impostare l'intensità dei singoli pixel

Assunzioni:
- linee di spessore unitario
- pixel binari, uno per colonna
- rappresentazione di un pixel come punto circolare centrato in (x, y) sulla griglia degli interi
- $|m| \le 1$
#### Algoritmo base
- calcola $m = \Delta y / \Delta x$
- incrementa x di 1 a partire dal punto più a sinistra
- calcola $y_i = m \cdot x_i + B$ per ogni $x_i$
- Illuminare il pixel $(x_i, round(y_i))$ 
Viene selezionato il pixel più vicino alla linea, ovvero la cui distanza dalla linea ideale è la minima
Strategia forza bruta: inefficiente --> ogni iterazione richiede moltiplicazione e arrotondamento

si elimina la moltiplicazione : $y_{i+1} =m\cdot x_{i+1}+B = m(x_i+\Delta x)+B = y_i + m$ perché $\Delta x = 1$

Ciò implica che x e y sono definiti a partire da valori assunti precedentemente

#### Algoritmo incrementale
- inizializzato con $(x_0, y_0)$ coordinate intere di un estremo
- evita la necessità di gestire esplicitamente il parametro $B$
![[Pasted image 20241024190539.png|400]]

Se $|m| > 1$, incrementando x si ottiene incremento su y maggiore di 1 
--> Algoritmo Digital Differential Analyzer (DDA) : strumento per risolvere sistemi di equazioni usando metodi numerici
Problemi: variabili reali con precisione limitata
Alternativa:
#### Algoritmo di Bresenham
Incrementale e basato su aritmetica intera, evita round()
![[Pasted image 20241024191544.png|400]]
Permette di calcolare $(x_{i+1}, y_{i+1})$ da $(x_{i}, y_{i})$
Versione floating point applicabile a linee con estremi in coordinate reali
Può essere applicato anche per la scan conversion dei cerchi
Miglior approssimazione, minimizzando l'errore dalla linea/cerchio reali
Difficile dea generalizzare a coniche arbitrarie

Scelto P $(x_p, y_p)$ occorre scegliere tra pixel ad incremento a destra (E) o incremento a destra + alto (NE)
Q intersezione della linea per cui è in corso la scan conversion con la griglia al $x=x_p+1$

Nella formulazione di Bresenham: calcolare la differenza tra le distanze verticali E-Q/NE-Q, segno della differenza utilizzato per selezionare il pixel la cui distanza da Q è minore (miglior approssimazione)

#### Algoritmo Midpoint
Formulazione midpoint: si osserva da che lato giace M: se sopra la linea E è più vicino, altrimenti NE
Errore sempre minore o uguale a 1/2

Ad ogni passo, sceglie tra due pixel in base al segno della variabile di decisione calcolata all'iterazione precedente
Aggiorna la variabile di decisione aggiungendo $\Delta_E$ o $\Delta_{NE}$ al valore precedente
### Cerchi
Circonferenze, $x^2+y^2=R^2$ . Modi efficienti per applicare la rasterizzazione: risolvere equazione implicita $y = f(x) = \pm \sqrt{R^2-x^2}$ ma è inefficiente per via delle moltiplicazioni e radici quadrate.

Algoritmo midpoint per circonferenze: genera punti su una circonf. centrata nell'origine incrementando in tutte le direzioni lungo la circonferenza. La strategia è selezionare quale tra due pixel è più vicino alla circonferenza valutando una funzione nel midpoint tra di essi.

Dato $F(x,y) = x^2+y^2-R^2$ , se il midpoint tra E ed SE è fuori dalla circonf. allora SE è più vicino alla circonf., altrimenti E
L'algoritmo è ottimizzabile sfruttando le simmetrie
### Poligoni
Utilizzabile per poligoni concavi e convessi, determina come trattare gruppi/intervalli di pixel (span) compresi tra i lati sx e dx del poligono

I pixel estremi dello span (sul contorno) sono calcolati con un approccio che determina l’intersezione tra il lato del poligono e la scan line in modo incrementale a partire dall’intersezione con la precedente scan line (senza calcolare analiticamente l’intersezione con ogni lato, coerenza)

Gli estremi possono essere calcolati:
- utilizzando l'algoritmo midpoint su ogni lato
- mantenendo una tabella di estremi per ogni scan line
- aggiornando la tabella quando viene prodotto un nuovo pixel per un lato

Possono essere prodotti estremi che giacciono fuori dal poligono. Selezionati dall'algoritmo di scan conversion perchè vicini ad un lato, senza considerare tuttavia se fossero interni o esterni.
Da evitare, anche quando il pixel esterno sarebbe più vicino al lato, per non invadere i pixel di altre primitive

- Intersezione con un valore X arbitrario e frazionario, come determinare quale dei pixel su due lati sia interno?
--> se ci si avvicina ad una intersezione frazionaria a destra e si è all'interno del poligono, si arrotonda all'intero inferiore la coordinata X dell'intersezione per definire il pixel come interno. Altrimenti verso l'intero superiore

- Come gestire il caso speciale delle intersezioni per pixel con coordinate intere?
--> se il pixel più a sx in uno span ha una coordinata X intera lo si definisce come interno, altrimenti esterno

- Come gestire il secondo caso per vertici condivisi tra diversi poligoni?
--> valori minimi e massimi di Y. nel calcolo della parità si considera $y_{min}$ di un lato ma non il $y_{max}$ per il lato adiacente. Un vertice $y_{max}$ viene disegnato solo se è il vertice $y_{min}$ per il lato adiacente

- Come gestire il secondo caso nella situazione in cui però i vertici definiscono un lato orizzontale?
--> non si contano i vertici di quei lati
## Algoritmo Scan-Line
Primo passo della procedura: individuare intersezioni della scan line con tutti i lati del poligono
Molti lati intersecati dalla scan line sono anche intersecati dalla scan line successiva (edge coherence)

Per ogni lato, algoritmo memorizza ($Y_{max}, X_{@min}, 1/m$) ovvero le info necessarie per mappare

Active Edge Table (AET) : lista di tutti i lati ordinati con: Y max, X in cui lato interseca la scan line corrente, valore di incremento della X (1/m)

Global Edge Table (GET) : memorizza info su tutti i lati del poligono, viene utilizzata per aggiornare l'AET
### Triangoli

Una pipeline di rasterizzazione moderna converte tutte le primitive in triangoli, anche punti e linee.
I triangoli: 
- possono essere utilizzati per approssimare qualsiasi forma 
- sono sempre planari, con una normal definita
- risulta facile interpolare info a partire dai vertici
- se ogni primitiva è ricondotta ad un triangolo, ci si può dedicare a progettare pipeline più ottimizzate

Adesso le domande sono:
- Quali pixel sono coperti dal triangolo (copertura)
- Quale triangolo è più vicino alla camera per quei pixel (visibilità)

Ci si può ridurre ad un problema analizzabile facendo riferimento al modello della camera oscura:
![[Pasted image 20241022142006.png|500]]

Spesso analizzato usando un modello basato su un cosiddetto "sensore virtuale": proiezione su un piano dell'immagine che, per comodità, viene considerato come esterno alla camera
![[Pasted image 20241104165351.png|500]]

L'algoritmo sarebbe:
```
Per ogni triangolo 
	Per ogni pixel 
		Il triangolo copre il pixel? 
			Prendi il primo triangolo incontrato
```

Vantaggi: con la rasterizzazione
- elaborazione triangolo = info triangolo + profondità per i pixel del raster
- raytracing richiede anche id disporre dell'intera scena in memoria

Limiti: applicabile solo a primitive per cui sia possibile effettuare scan conversion. Gestione non unificata di ombre, riflessioni e trasparenze

Visibilità: per determinare quale sia il primo triangolo incontrato in caso di sovrapposizione, si usa buffer di profondità (z-buffer)

Copertura: scene reali sono complesse, determinare copertura esatta non è fattibile.
Considerare come un problema di campionamento (sampling), non si cerca soluzione esatta, ma si verifica un insieme di punti a "campione", con un numero sufficiente e una scelta intelligente delle posizioni, si può ottenere una buona stima

## Approccio "Forza Bruta"
Interno del triangolo = insieme di punti interni ai 3 semipiani definiti dalle 3 linee dei lati.
Algoritmo:
```
Calcolo coefficienti E_1, E_2, E_3 dai vertici proiettati
per ogni pixel
	valuta funzione nel centro del pixel
	se negativo:
		pixel è interno al triangolo
```

### Approccio Bounding Box
considerare solo i pixel dentro al bounding box 2D del triangolo (valori max e min in X e Y)
```
Per ogni triangolo 
	Calcola la proiezione dei vertici 
	Calcola Ei 
	Calcola il bounding box (sullo schermo) 
	Per ogni pixel nel bounding box 
		Valuta le funzioni Ei Se tutte maggiori o uguali a zero 
		Framebuffer[x,y] = colore del triangolo
```

### Approccio Incrementale
```
Per ogni triangolo 
	Calcola la proiezione dei vertici 
	Calcola il bounding box (sullo schermo) 
	Per ogni scan line y nel bounding box 
		Valuta le funzioni Ei=aix0+biy+ci in (x0,y) 
		Per ogni pixel nel bounding box 
			Se tutte maggiori o uguali a zero 
				Framebuffer[x,y] = colore del triang. 
			Incrementa l’eq. della linea Ei+=ai
```
![[Pasted image 20241104175942.png]]

### Approccio Gerarchico
si verificano in maniera "conservativa" blocchi di pixel prima di andare ad effettuare valutazioni pixel per pixel. Se i blocchi non intersecano il triangolo vengono scartati
![[Pasted image 20241104180111.png]]

## Aliasing
Il campionamento e la ricostruzione imperfetti portano ad artefatti (aliasing)
- Jaggies in immagini statiche
- shimmering in immagini animate
- effetto Moirè in aree delle immagini ad alta frequenza
## Antialiasing
Soluzione: aumento della risoluzione (riduzione del problema)
grazie al campionamento, ci sono soluzioni più efficaci
- intensità pixel in base alla copertura dell'area