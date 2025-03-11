animazioni = sequenze di immagini statiche
flickering = mancata sensazione di continuità

frequenza massima e minima del nostro occhio
troppo bassa: motion blur

frequenza di visualizzazione bassa: flickering
minima frequenza: 10-12fps

Frequenze standard:
- Cinema: 24fps
- PAL: 25fps
- NTSC: 30fps

primo strumento: taumatropio --> due immagini si fondono se girato abbastanza velocemente
poi: zootropo (o ruota della vita) --> cilindro che ruota attorno al suo asse, serie di immagini prendono vita
alla fine dell'Ottocento: lanterna magica, shadow puppets
1891 Thomas Edison inventa motion picture projector. Primo apparecchio che utilizza pellicola, uso individuale
Fratelli Lumiere 1895

Fantasmagorie primo film di animazione
Ogni frame dell'animazione:
- disegno su carta
- ognuno leggermente diverso dal precedente
- copiati su fogli di acetato trasparente
- colorati
- fotografati per trasferimento su pellicola

Evoluzioni:
- divisione tra sfondo e personaggi, per fare meno lavoro --> separazione fondali - personaggi

Walt Disney: propone camera multi piano, ciascuno con un foglio di acetato
pro: muovere i piani --> pan e tilt, zoom in e zoom out senza ridisegnare nulla

Pipeline di produzione:
- idea preliminare --> script, cosa voglio rappresentare
- storyboard --> forma grafica di vignette per rappresentare i momenti salienti della storia
- soundtrack preliminare --> scratch track, pre-registrazione temporanea di voci e suoni, da un idea dei tempi delle scene
- animatic --> story reel, animazione approssimata, serve a revisionare e confermare storyboard, corrispondenza suoni scene e storyboard
- design --> definizione elementi, fondali, personaggi ecc...
- timing --> definizione dei keyframe, quanti frame necessari per i movimenti, espressioni...
- layout --> definizione finale di background, inquadrature, colori, pose...
- animazione --> processo di disegno di ogni frame
	- Key animator : disegna frame principali
	- Assistant animator : tweening, disegna frame intermedi tra i keyframe
	- Clean-up : ri-disega tutto per continuità di stile
- sfondi --> background artists
- inking & painting --> 
	- Tradizionale: disegni trasferiti su fogli di acetato trasparente, dipinti a mano, trasferiti su pellicola
	- Digitale: disegni scannerizzati, colorazione e composizione con gli sfondi via software, salvataggio in formato video digitale, stampa su pellicola
	- Walt Disney: inking e painting digitale del 1989 "La Sirenetta"


24fps = alto numero di disegni
Disney: 18 disegni al secondo, ma gli altri son poveri --> tecniche per aver meno disegni
Shooting on two = un singolo fotogramma è tenuto fermo per 2 frame, ma l'animazione è meno fluida

Animazione piena: molti dettagli, molti frame disegnati
Animazione limitata: produzioni a basso budget, produzione televisive, disegno di pochi frame, riutilizzo di parti o dettagli statici, utilizzo di animation loop

Animazione + Live Action --> Space Jam, Mary Poppins
mischiare il vero con il virtuale (ad oggi: green screen)


Stop Motion: tecnica degli anni '60
Spostamento e posizionamento manuale di oggetti/personaggi/modelli
Tutti fotogrammi chiave

## Principi della Computer Animation
Lasseter (animatore Disney passato a Pixar) legò i principi dell'animazione a tecniche comunemente usate in CA:
- Squash & Stretch
- Timing
- Secondary actions
- Slow in & Slow out
- Arcs
- Follow through overlapping action 
- Exaggeration 
- Appeal 
- Anticipation
- Staging
- Stright ahead vs. pose to pose


Produzione di un animazione: collezione di sequenze (un file diviso in scene o un file per scena)
Sequenza = collezione di shot, quello che inquadra la telecamera in quella posizione
Ogni shot = collezione di fotogrammi (24-25-30fps)

Pipeline di produzione
![[Screenshot 2025-03-04 at 6.36.01 PM.png|500]]

Editing:
- Lineare --> tradizionale, diverse sorgenti, durata ordine ecc... scelti prima
- Non Lineare --> digitale
![[Screenshot 2025-03-04 at 6.37.42 PM.png]]

## Interpolazione

- come interpolare un parametro che varia nel tempo?
- parametrizzare in base alla distanza
- controllo della posizione interpolata nel tempo

"come generare al meglio i valori dei parametri per gli in-between frame?"
- trovare una tecnica di interpolazione appropriata
- applicare take tecnica

AVAR: Animation Variable, un qualunque parametro animabile
- posizione, orientamento, scala, forma, colore, velocità, trasparenza ecc...

Il tempo non è l'unico driver delle animazioni

Interpolazione (sx) vs Approssimazione (dx)
![[Screenshot 2025-03-11 at 4.14.24 PM.png]]

Complessità: complessità della funzione di interpolazione influenza il tempo di calcolo. Per contro, funzioni di interpolazioni troppo semplici possono dare origine a traiettorie non smooth.
--> funzioni polinomiali cubiche

Continuità: la continuità della traiettoria è una considerazione primaria: può influenzare il realismo dell'animazione
Matematicamente determinata da quante derivate dell'equazione della curva sono continue
![[Screenshot 2025-03-11 at 4.20.19 PM.png]]

Controllo di forma --> Globale vs Locale: 
- per controllare la forma della curva l'utente posiziona una serie di punti.
- la modifica ad uno di questi punti può influenzare la forma di tutta la curva (contr. Globale) o solo della zona vicina al punto di controllo (contr. Locale)
Preferite quelle locali

### Curve
Explicit: $y=f(x)$ può essere ambigua
Implicit: $0 = f(x,y)$ non vanno bene, l'uguaglianza non si verifica se x e y sono float, causa errori di approssimazione dei calcolatori

Parametric form: $x = f(u) \quad  y = g(u)$ --> al variare di un parametro, si ottengono dei valori x e y.

se vogliamo controllare nel tempo gli attributi animabili, bisogna ricondurre u a t (time)

solo variabili elevate a potenza --> Polinomiale
se potenza maggiore = 1 --> Lineare
seno, coseno, logaritmo --> Trascendente

**polinomiali cubiche** sono le più diffuse

Interpolazione lineare semplice
![[Screenshot 2025-03-11 at 4.33.07 PM.png|500]]

riscritta come $P(u) = (P1 - P0)\cdot u + P0$
ulteriormente riscritta come:
![[Screenshot 2025-03-11 at 4.36.41 PM.png|500]]

Esistono altre funzioni di interpolazione che permettono di generare traiettorie lineare che "vengono percorse" in modo non lineare

### Interpolazione di Hermite
![[Screenshot 2025-03-11 at 4.39.16 PM.png|500]]
![[Screenshot 2025-03-11 at 4.39.42 PM.png|500]]

La continuità è garantita assicurando che il vettore tangente alla fine di un segmento sia uguale al vettore tangente iniziale del segmento successivo

Problema: sono curve a livello globale: modificando un solo parametro può cambiare in modi diversi:
![[Screenshot 2025-03-11 at 4.42.37 PM.png|400]]/

Problema: calcolare la derivata prima di P nel fattore B è molto dispendioso
### Interpolazione Catmull-Rom Spline (Spline Cardinali)

unire due tratti di curve spline cardinali, ora significa congiungere $P_{i-1}$ e $P_{i+1}$ in modo geometrico
![[Screenshot 2025-03-11 at 4.48.49 PM.png|400]]
è un approssimazione che aiuta i calcoli, e se i punti non son troppo distanti tra loro, non è troppo diversa dal valore di derivata prima calcolato analiticamente

Cambiano M e B:
![[Screenshot 2025-03-11 at 4.50.04 PM.png|200]]

Anche queste sono curve a controllo globale

### Curve di Bezier

n+1 punti di controllo, miscelati (blended) per produrre un vettore di punti che costituisce la curva P(u) che descrive la traiettoria di una funzione di approssimazione tra gli estremi
![[Screenshot 2025-03-11 at 4.52.00 PM.png|500]]

curva definita mediante punto di partenza, punto di fine e due punti interni che controllano la forma della curva.
La continuità tra segmenti è controllata mediante la colinearità dei punti di controllo

### B-Spline
Le curve più flessibili: grado può essere definito entro certi limiti, indipendentemente dal numero di punti di controllo
Per contro, son più complesse delle altre curve
NURBS: NonUniform Rational B-Splines --> i punti di controllo sono distribuiti in modo non uniforme

