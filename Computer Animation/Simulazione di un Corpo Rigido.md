
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

