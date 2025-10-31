
Ciclo di simulazione
![[Screenshot 2025-03-18 at 6.22.55 PM.png|600]]

un oggetto che subisce forze cambia il suo stato, con inerzia che dipende da massa, momento angolare ecc...
Quando le forze non sono in linea con il baricentro, allora l'oggetto ruota

### Moto di un punto nello spazio
- accelerazione nulla --> velocità costante --> eulero
  ![[Screenshot 2025-03-25 at 4.35.10 PM.png|400]]
- forze cumulate non nulle --> accelerazione costante --> 
  ![[Screenshot 2025-03-25 at 4.36.06 PM.png|400]]
	velocità media nel periodo temporale è la media delle velocità iniziale e finale
	![[Screenshot 2025-03-25 at 4.37.56 PM.png|400]]
L'importante è l'effetto visuale

### Moto Circolare
forza applicata ad un corpo rigido non direttamente in linea con il suo centro di massa = rotazione

l'orientamento di un oggetto può essere rappresentato tramite una matrice di rotazione

la velocità angolare è indipendente dalla velocità lineare

la direzione del vettore rappresenta l'asse attorno al quale l'oggetto sta ruotando
L'ampiezza del vettore rappresenta il numero di rivoluzioni attorno all'asse per unità di tempo

si consideri un punto A: ruota attorno ad un asse di rotazione con velocità angolare. 
A può essere descritto come: $b+r(t)$
Variazione di r(t) nel tempo (prima derivata): omega vettoriale r(t)
![[Screenshot 2025-03-25 at 4.49.25 PM.png|500]]

Si consideri un corpo rigido (non più un punto): adesso l'orientamento è dato dalla matrice di rotazione R(t).
Il cambiamento della matrice di rotazione può essere calcolato facendo il prodotto vettoriale di omega con ogni colonna di R(t)
![[Screenshot 2025-03-25 at 4.52.02 PM.png|500]]

![[Screenshot 2025-03-25 at 4.52.43 PM.png|400]]

 Centro di Massa

Il centro di massa di un oggetto è il punto nel quale l'oggetto è bilanciato in tutte le direzioni
In computer grafica la distribuzione di massa è modellata mediante punti individuali
Se le masse individuali sono $m_i$ la massa totale è la sommatoria: $M = \sum m_i$
Se il centro di massa è detonato mediante x(t) e le posizioni individuali mediante $q_i(t)$, il centro di massa si può esprimere come: $x(t) = \frac{\sum m_i q_i (t)}{M}$

 Forze

forza lineare applicata ad una massa da luogo ad una accelerazione $a = F/m$
Se più forze sono applicate, si fa la somma delle forze nel punto
Se il punto è parte di un corpo rigido, l'applicazione di una forza avrà un impatto sia sulla velocità lineare che quella angolare

 Momento di torsione

Equivalente rotazionale di una forza lineare: momento di torsione (torque)
deriva dall'applicazione di una forza ad un punto non coincidente con il centro di massa
momento di torsione : prodotto vettoriale tra il vettore dal centro di massa e il punto di applicazione della forza
$\tau = r \times F$

Momenti
in un sistema chiuso il momento totale è conservato
il momento di un oggetto è decomposto in una componente lineare ed una angolare

Momento lineare = somma massa per velocità su tutti gli i-esimi punti
![[Screenshot 2025-03-25 at 5.07.03 PM.png|400]]
![[Screenshot 2025-03-25 at 5.07.45 PM.png|300]]

Momento angolare: $L(t) = r \times p$
la derivata del momento angolare è uguale al momento torsale
![[Screenshot 2025-03-25 at 5.09.51 PM.png|400]]

Matrice dei tensori inerziali $I(t)$ = distribuzione di massa dell'oggetto nello spazio
descrive la resistenza di un oggetto a cambiare il suo momento angolare
$L(t)=I(t)\omega(t)$
Tensori descritti in una matrice 3x3
![[Screenshot 2025-03-25 at 5.13.01 PM.png|300]]

### Equazione di Stato

Lo stato di un oggetto è memorizzato in un vettore che contiene
- x(t) : posizione       
- R(t) : orientamento
- P(t) : momento lineare
- L(t) : momento angolare 
![[Screenshot 2025-03-25 at 5.15.05 PM.png|500]]

In qualunque istante di tempo è possibile calcolare le velocità dell'oggetto (angolare e lineare) e la variazione dei tensori inerziali come:
![[Screenshot 2025-03-25 at 5.15.27 PM.png|300]]

## Cinematica

Modello gerarchico: serie di oggetti uniti tra loro mediante vincoli di connettività
I giunti che permettono alle appendici ad esso connesse il modo in una sola dimensione si dicono a 1 grado di libertà (1 DOF)

![[Screenshot 2025-03-25 at 6.05.26 PM.png|400]]

Un arco contiene infomazioni che:
- permettono di ruotare-traslare l'oggetto relativamente al giunto di livello superiore nella gerarchia
- permettono di stabilire la posizione relativa rispetto al giunto corrente

Un nodo contiene:
- informazione relative all'oggetto
- le trasformazioni applicabili all'oggetto stesso

![[Screenshot 2025-03-25 at 6.11.31 PM.png|400]]

non sempre si parla solo di gerarchie binarie, ma anche con più figli

Cinematica Diretta: l'animatore definisce tutti gli angoli dei giunti
spesso difficile e noioso, trial-and-errorn

Cinematica Inversa: data una posizione finale, calcolare la configurazione dei giunti che porta a quella posizione. interpolare dalla posizione finale alla posizione obiettivo

![[Screenshot 2025-03-25 at 6.28.10 PM.png|400]]
è necessario verificare che la posizione sia raggiungibile dal sistema:
![[Screenshot 2025-03-25 at 6.39.53 PM.png|400]]
ci sono sempre soluzioni simmetriche rispetto all'asse, e non sempre la prima è quella che vogliamo noi.

## Figure Articolate

Modellare e animare figure articolate è un task complesso: il problema è il corpo umano virtuale
![[Screenshot 2025-05-01 at 2.02.04 PM.png]]
Si consideri il braccio come una componente isolata dal resto del corpo
![[Screenshot 2025-04-07 at 7.37.50 PM.png|400]]

problema dell'avambraccio: 
- rotazione associata al polso, ma la rotazione è distribuita lungo l'avambraccio stesso visto che radio e ulna ruotano l'uno attorno all'altro
	Soluzione: giunto a metà per distribuire la rotazione
- I giunti del braccio umano devono avere dei limiti (gomito: 160 gradi)
	catena cinematica per le ossa dei Bone Constraints, che son diversi dai vincoli di oggetto

7 DoF minimi + 1 per collegare il bracio al busto

Euristiche x il braccio:
- Dal più vicino al più lontano
- giunto lontano ha più effetto di quelli vicini

Modello della mano: polso palmo falange falangine e falangette per tutte le dita --> 27 DoF
![[Screenshot 2025-05-01 at 2.14.09 PM.png]]
![[Screenshot 2025-05-01 at 2.20.05 PM.png|400]]
movimento di una figura articolata in presenza di ostacoli: i numeri in figura son pesi che vengono usati per calcolare la probabilità di collisione con gli oggetti.

Camminata: modellare una camminata è uno dei compiti più difficili
è dinamicamente stabile ma non staticamente stabile
	moto di un umano non può essere bloccato in un determinato istante e analizzato staticamente per determinare quali forze lineari e torsioni determinano il moto

- lunghezza del passo
- rotazione dell'anca
- posizionamento dei piedi

usati per specificare come una particolare camminata dovrebbe apparire
Comprendere l'interazione tra i vari giunti coinvolti nel moto è il primo passo per modellare la camminata

Modello della Gamba
![[Screenshot 2025-05-01 at 2.30.19 PM.png|400]]

Ciclo camminata può essere diviso in diverse fasi:
![[Screenshot 2025-05-01 at 2.33.52 PM.png]]
![[Screenshot 2025-05-01 at 2.34.13 PM.png|500]]

Ciclo di corsa: diverso da camminata
- piedi non sono mai entrambi sul pavimento contemporaneamente
- istante in cui entrambi i piedi sono in aria
![[Screenshot 2025-05-01 at 2.35.45 PM.png]]

## Animazione facciale

Creazione del modello è il primo problema
Fattori importanti
- metodo di acquisizione dei dati geometrici
- controllo del moto
- qualità di rendering
- qualità del moto

2 tecniche per la costruzione del modello
- digitalizzazione (mediante scanner 3D o da fotografie)
- modifica di un modello esistente

metodo più semplice per l'animazione: identificare delle pose (espressioni) chiave
Animazione facciale ottenuta da interpolazione tra pose chiave
approccio restrittivo perchè non individualmente controllabile dall'animatore

Facial Action Coding System (FACS) ha l'obiettivo di decomporre tutte le espressioni facciali in un insieme di movimenti facciali basici
I movimenti sono detti Actiun Unit (AU)
son stati identificate 46 AU
Un animazione facciale può essere costruita dando accesso all'utente ad un insieme di variabili che corrispondono 1-1 con le AU
non è time-based e non incorpora la descrizione per il parlato

Modellazione muscoli:
possono essere
- lineari --> muscolo contraendosi tiri un punto (punto di inserzione) verso l'altro (punto di attacco)
- laminari --> array di muscoli lineari
- radiali --> contraggono radialmente verso un centro immaginario

possono essere posizionati:
- sulla superficie del volto (attaccati solo alla pelle)
- attaccati da una parte alla pelle e dall'altra alle ossa (anatomicamente più accurato ma più complesso)

reazione muscolare : muscolo attivato --> punto di inserzione si muove di un certo ammontare verso il punto di attacco
il movimento del muscolo crea una deformazione che si propaga sulla pelle e determina l'espressione facciale
il modello più semplice è quello di considerare la distanza dal punto di inserzione
![[Screenshot 2025-05-01 at 2.51.41 PM.png|600]]

