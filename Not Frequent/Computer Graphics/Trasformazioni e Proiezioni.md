# Trasformazioni Geometriche

corrispondenza biunivoca che associa ogni punto P con P' appartenente allo stesso spazio
	$$P'=f(P)$$

## Trasformazioni Lineari
preservano le operazioni di somma e moltiplicazione (combinazioni lineari)
preservano le line e l'origine del sistema di riferimento
forma matriciale

Esempi: rotazione, scalamento, scorrimento, riflessione

- traslazione: linee, distanze tra punti, angoli ed orientamento
- scalamento: origine, linee, linee per origine e angoli tra linee parallele
- rotazione: preservati origine, linee, distanze tra punti, orientamento
- scorrimento: origine, linee, distanze tra punti su linee non parallele alla direzione dello scorrimento e distanze relative tra punti collineari

Sono facili da calcolare per le GPU

trasformazioni affini: preservano linee e distanze, es: rotazione e traslazione.
### Rotazione
possibile ruotare un oggetto intero --> infiniti punti in un oggetto, quindi applica solo agli estremi

dato (x, y)
$$
x' = r \cdot \cos(\phi+\theta) = r \cdot\cos\phi\cos\theta - r \cdot\sin\phi\sin\theta = x \cdot\cos\theta-y\cdot\sin\theta
$$
$$
y' = r \cdot \sin(\phi+\theta) = r \cdot\cos\phi\sin\theta - r \cdot\sin\phi\cos\theta  = x \cdot\sin\theta+y\cdot\cos\theta
$$
in forma matriciale:
$$
\begin{bmatrix} x' \\ y' \end{bmatrix} =
\begin{bmatrix} \cos\theta &-\sin\theta \\ \sin\theta &\cos\theta \end{bmatrix} \cdot 
\begin{bmatrix} x\\ y\end{bmatrix} 
$$
Per la rotazione:
$$
\begin{bmatrix} \cos\theta &-\sin\theta \\ \sin\theta &\cos\theta \end{bmatrix} \begin{bmatrix} \cos\theta &\sin\theta \\ -\sin\theta &\cos\theta \end{bmatrix} =
R \cdot R^T =
\begin{bmatrix} 1 &0 \\ 0 &1 \end{bmatrix} = I
$$

#### Riflessione (asse X)
$$
Q = \begin{bmatrix} -1 &0 \\ 0 &1 \end{bmatrix}
$$
Non viene preservato l'orientamento (ribaltato) ma solo l'origine, linee e distanze
Non tutte le matrici per cui è valida la proprietà $Q Q^T = 1$ sono matrici di rotazione

#### Scalamento
$$
\begin{bmatrix} x' \\ y' \end{bmatrix}
= \begin{bmatrix} a &0 \\ 0 &a \end{bmatrix} \cdot
\begin{bmatrix} x \\ y \end{bmatrix}
$$
Scalare il punto P (x,y) di una quantità A lungo l'asse X e Y.
Vengono preservati: origine, linee, linee passanti per l'origine e angoli tra linee parallele
Per scalamento negativo (a = -1) può essere visto come una sequenza di due riflessioni
$$
Q = \begin{bmatrix} -1 &0 \\ 0 &1 \end{bmatrix} \quad 
Q = \begin{bmatrix} 1 &0 \\ 0 &-1 \end{bmatrix}
$$
Dal momento che ogni riflessione ribalta l'orientamento, in 2D l'orientamento è preservato (non necessariamente vero in tutti i casi)

Le proporzioni dello scalamento possono anche cambiare ($S_x$ e $S_y$)
#### Scorrimento
$$
\begin{bmatrix} x' \\ y' \end{bmatrix}
= \begin{bmatrix} 1 &T \\ 0 &1 \end{bmatrix} \cdot
\begin{bmatrix} x \\ y \end{bmatrix}
$$
Spostamento di ogni punto X lungo la direzione *u* in base alla distanza da un vettore fisso *v*
#### Traslazione
Consente di traslare un punto del piano in una nuova posizione
Sarebbe comodo poter definire le trasformazioni tramite prodotti di matrici --> trasformazione composta
$$
\begin{bmatrix} x' \\ y' \end{bmatrix}
= \begin{bmatrix} a &b \\ c &d \end{bmatrix} \cdot
\begin{bmatrix} x \\ y \end{bmatrix}
$$
![[Pasted image 20241015122906.png]]

### Coordinate Omogenee

Rappresentazione matriciale:
- Traslazione : $P' = T + P$
- Scalamento : $P' = S \cdot P$
- Rotazione : $P' = R \cdot P$

coordinate omogenee: derivano dallo studio della prospettiva, introdotte da Mobius come modo naturale per assegnare coordinate alle linee
Essenziali nella grafica: trasformazioni 3D, proiezioni prospettiche, calcolo delle ombre etc...

Ogni piano 2D che non passa per l'oridine o in 3D : ogni linea L che passa per l'origine in 3D corrisponde ad un punto p sul piano 2D (intersezione della linea col piano)

Per i punti 2D, si aggiunge una terza coordinata : $\hat p = (x,y,W)$
due set di coordinate rappresentano lo stesso punto se e solo se uno è un **multiplo** dell'altro

Se W è diverso da 0, allora : $(x/W, y/W, 1)$

Usando le coordinate omogenee è possibile rappresentare la trasformazione 2D come una trasformazione lineare 3D (sotto forma di prodotti)
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix}
= \begin{bmatrix} 1 &0 & d_x\\ 0 &1 & d_y \\ 0 & 0 & 1 \end{bmatrix} \cdot
\begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$
Oppure : $P' = T(d_x, d_y) \cdot P$

### Composizione di Trasformazioni

Due Traslazioni: somma degli offset
![[Pasted image 20241015124003.png]]

Due Scalamenti: prodotto dei fattori di scalamento
![[Pasted image 20241015124048.png]]

Due rotazioni: somma angoli

Bisogna fare attenzione nell'uso della forma matriciale: il prodotto di matrici non è cumulativo, cambiando l'ordine delle trasformazioni si genera un risultato diverso, la trasformazione che deve essere eseguita per prima è posta in fondo alla composizione (destra)

- Di Corpo Rigido / Euclidea --> preservati distanze ed angoli
- Di similarità / similitudine --> preservati angoli
- Lineari --> scalamento, rotazione, scorrimento
- Affini --> preservati parallelismo delle linee e rapporti di distanze
- Proiettive --> preservate linee

![[Pasted image 20241015131459.png]]

Risultato del prodotto di una sequenza di trasformazioni affini (rotazioni, traslazioni, scalamenti) è ancora una trasformazione affine.
- preservato parallelismo delle linee
- non le distanze ed angoli

Le matrici omogenee possono essere moltiplicate tra loro per ottenere le trasformazioni composte volute. Obiettivo: migliorare efficienza calcoli applicando una singola matrice

Per scene complesse, uno **scene graph** può aiutare ad organizzare le trasformazioni. Memorizza trasformazioni relative in un grafo orientato.
Le trasformazioni sono mantenute in uno stack per evitare moltiplicazioni ridondanti (push, pop)
Altro utilizzo: gestione di diverse copie dello stesso oggetto in una scena (istanziazione)
## Trasformazioni 3D

Usando coordinate omogenee: 3D ha 4 coordinate (x,y,z,W)
sistema destroso (Z uscente), perpendicolare allo schermo, rotazione positiva è antioraria

![[Pasted image 20241015132137.png]]

Rotazione intorno ad un asse arbitrario?
	traslazione (T) e rotazione (R) per coincidere con l'asse, rotazione, rotazione (-R) e traslazione inversa (-T)

Rappresentazione angoli: necessari 3 gradi di libertà
- angoli di Eulero --> problema : gimbal lock, blocco cardanico
- alternativa : Quaternioni (numeri complessi, 4 coordinate)
### Cambio di Sistema di Riferimento

sistema di riferimento inalterato, oggetti trasformati rispetto all'origine.

modo alternativo, ma equivalente, di guardare una trasformazione è effettuare un cambio di sistema di riferimento.

![[Pasted image 20241015133555.png]]
### Camera Virtuale
Osservatore, o (tele)camera virtuale
Metafora utile per studiare la creazione di scene 3D
Telecamera ed oggetto hanno un proprio sistema di riferimento

PARAMETRI DI VISTA : specificano le condizioni sotto le quali si vuole vedere il mondo / modello

Occorre generare matrici di trasformazione: si può costruire vettori ortogonali a quello della camera, e traslare per raggiungere l'origine

Proiezioni: trasformano punti in un sistema di riferimento di dimensione n, in punti di un sistema di riferimento di dimensione \<n
Proiezione di un oggetto è definita dai raggi di proiezione o proiettori
- partono da un centro i proiezione
- passano attraverso ogni punto dell'oggetto
- intersecano una superficie di proiezione
- per formare la proiezione

Possono essere:
- prospettiche : centro di proiezione esplicito
- parallele : centro ad infinito, direzione di proiezione
![[Pasted image 20241015140209.png]]

![[Pasted image 20241015140233.png]]

Proiezioni prospettiche sono definite da piano e centro di proiezione.
scorcio prospettico: effetto di sistema fotografico o sistema visivo umano
la dimensione della proiezione prospettica di un oggetto varia in odo inv. prop. alla distanza dal centro di proiezione

Realistica, ma non particolarmente utile quando occorre rappresentare la forma esatta / dimensioni di un oggetto
- non si possono misurare lunghezze
- angoli preservati solo per facce parallele al piano di proiezione
- linee parallele convergono in **punti di fuga**

Alternativa: proiezione parallela, (in particolare ortografica) --> misure esatte, linee rimangono parallele, angoli come per prospettiva
### Punti di Fuga
le proiezioni prospettiche di qualunque insieme di linee non parallele al piano di proiezione convergono in un punto: punto di fuga (vanishing point)
In 3D, linee parallele si incontrano solo ad infinito
Punto di fuga interpretabile come la proiezione di un punto ad infinito
numero infinito di punti di fuga, uno per ogni infinita direzione in cui la linea può essere orientata

Se insieme di linee è parallel ad uno dei tre assi principali: fuga sull'asse

Proiezioni prospettiche possono avere 1, 2, 3 punti di fuga principali.

Proiezioni parallele
Definite da direzione verso il centro di proiezione, piano di proiezione.
Generalmente non sono adatte per immagini foto-realistiche, ma sono utili per comprendere le forme 3D degli oggetti.
Più veloci da calcolare

Classificate in due tipologie in base alla relazione tra direzione della proiezione e la normale al piano di proiezione
- ortografiche : normale al piano uguale alla direzione di proiezione (frontale, laterale, dall'alto)
- oblique : normale e direzione di proiezione diverse

### Volume di Vista

individua quella proporzione di mondo 2D su cui deve essere effettuato il clipping. Deve essere quindi proiettata sul piano di vista.

Piramide semi-infinita con vertice nel centro di proiezione e lati passanti per gli spigoli della finestra
Approssimazione del cono di vista degli occhi, ma più semplice dal pov matematico, e compatibile con finestre rettangolari

Per una proiezione parallela : parallelepipedo infinito, con lati paralleli alla dir. di proporzione

Spesso interesse per volumi finiti (piani di clipping)



![[Pasted image 20241016133544.png]]
![[Pasted image 20241016133556.png]]

Clipping : rimozione delle primitive non visibili dalla camera, per evitare sprechi di risorse nella rasterizzazione
![[Pasted image 20241016133654.png]]

Invece di proiettare direttamente il volume di vista, si considera cubo unitario nel sistema di riferimento delle coordinate di proiezione / del disposivo normalizzate (NPC)
volume di vista trasformato in un parallelepipedo in NPC che si estende di $min$ e $max$ in ogni direzione --> **3D viewpoint**
![[Pasted image 20241016133907.png]]

Per definire la viewpoint 3D
- piano di clipping anteriore = $Z_{max}$;  piano clipping posteriore = $Z_{min}$
- lato $U_{min}$ = $X_{min}$; lato $U_{max}$ = $X_{max}$
- lato $V_{min}$ = $Y_{min}$; lato $V_{max}$ = $Y_{max}$
- faccia $Z = 1$ del cubo unitario è mappata sul quadrato più grande che può essere iscritto nell'area di visualizzazione

