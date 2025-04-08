1. tecnica seed vertex : quando vertice si sposta proporzionalmente modifica i vertici vicini mediante un parametro (su blender : proportional editing)

2. Free form deformation : 
	- griglie 2D/3D (su blender : lattice, sistema di riferimento (u,v,w) != da (x,y,z) ) 
	- polyline : mantenere delle distanze (su blender : armature in envelope mode)

(recupera fino a 17/43)

"descrivere una deformazione globale"

si intende un procedimento per cui i vertici sono moltiplicati da una matrice 3x3 i cui coeff. dipendono dal vertice stesso.

Deformazioni globali: matrice di trasformazione applicata sul punto che deve essere trasformato
$$
P' = M(P)*P
$$
Twist, Tapering, su blender chiamate simple deform (modifier)
- tapering = assottigliamento
  ![[Screenshot 2025-04-08 at 4.24.22 PM.png|300]] 
- twist = avvitamento lungo un asse
- bending
- stretch

# FFD 3D

Sistema di riferimento locale (S, T, U)
più semplice agire su un numero limitato di vertici nella griglia che il numero illimitato di vertici dell'oggetto

Interpolazione tri-cubica : sommatorie = cicli for

Ogni tecnica di deformazione può essere applicata di serie ad altre
![[Screenshot 2025-04-08 at 4.38.21 PM.png|500]]

anche composte in modo gerarchico, permette di avere diversi livelli di dettaglio

Animazione mediante FFD : propagazione
![[Screenshot 2025-04-08 at 4.41.43 PM.png|500]]

è anche possibile animare i punti di controllo della griglia (shake key)
![[Screenshot 2025-04-08 at 5.02.11 PM.png|500]]

# Morphing

metamorfosi di immagini bidimensionali. Immagine sorgente ed immagine destinazione

- griglie di coordinate (definite da utente)
- linee caratteristiche (definite da utente)

### Griglie di coordinate

due griglie curvilinee che abbiano corrispondenza in numero di punti e zone da conformare
![[Screenshot 2025-04-08 at 5.06.56 PM.png|400]]

una mesh è poi generata usando punti di intersezione della griglia come punti di controllo per uno schema di interpolazione come quello prodotto mediante spline

Per generare un immagine intermedia tra quella sorgente e quella destinazione, i vertici sono interpolati performare una griglia intermedia

I pixel delle immagini sorgente e destinazione sono modificati per ottenere le immagini intermedie

![[Screenshot 2025-04-08 at 5.11.03 PM.png|300]]

Per calcolare un immagine intermedia si utilizza una griglia ausiliaria (rende più smooth la transizione)

La griglia ausiliaria è costruita utilizzando le coordinate x della griglia di partenza e le coordinate y della griglia intermedia

l'immagine di partenza e la griglia ausiliaria sono utilizzati per distorcere i pixel sorgenti lungo l'asse x

Vengono calcolate le intersezione tra le curve verticali delle griglie di partenza e ausiliarie con scanline orizzontali

viene stabilita una corrispondenza tra coordinate di griglia e coordinate dei pixel

ottenuti due grafici, si usa l'indice dei pixel del grafico sulla griglia ausiliaria per determinare l'indice di colonna sul grafico della griglia di partenza

L'indice di colonna mi dice quali sono i pixel dell'immagine di partenza che devono essere usati per calcolare quello intermedio

il procedimento è ripetuto prendendo la griglia ausiliaria e quella intermedia:
- queste griglie sono ottenute per distorcere l'immagine lungo y

procedura ripetuta due volte: 
- griglia di partenza e griglia intermedia
- griglia intermedia e griglia di arrivo

Alle due immagini ottenute si applica un effetto di dissolvenza incrociata ottenuto come un blend dei colori dei pixel corrispondenti:

$$
C[i][j] = \alpha C_1[i][j]+ (1-\alpha)\cdot C_2[i][j]
$$

### Linee caratteristiche

invece di griglie, utente può stabilire la corrispondenza tra due immagini usando line caratteristiche
Linee sono disegnate sulle due immagini (partenza e arrivo) per identificare caratteristiche corrispondenti
- linee caratteristiche sono interpolate per formare un insieme di linee caratteristiche intermedie
- le linee caratteristiche permettono di mappare il peso dei pixel

