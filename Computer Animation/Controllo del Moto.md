determinare forma della curva è solo il primo dei passi per generare l'animazione
Bisogna controllare la velocità con cui la curva è tracciata in funzione del valore parametrico
Il primo passo è quello di riuscire a produrre passi di incremento uniformi lungo la curva

Si assuma che $u$ sia un parametro e $P(u)$ la funzione di interpolazione scelta

Lunghezza d'arco: rappresenta la distanza percorsa da p(u1) a p(u2) 
Per garantire velocità costante, la funzione deve essere parametrizzata mediante la lunghezza d'arco, ovvero la distanza lungo la curva di interpolazione
Deve essere un procedimento che aggiunge il minor overhead possibile

$$S = G(u)$$
dove S = lunghezza d'arco, e G è una funzione che scegliamo noi

!! DOMANDA DA ESAME !!
1. Traiettoria va campionata, più punti campione = più precisione = più lungo il processo (tradeoff)
2. calcolo distanza tra punto campione e precedente e si somma al punto
![[Screenshot 2025-03-11 at 5.11.26 PM.png|500]]

G(0.00) = 0
G(0.05) = distanza tra P(0.00) e P(0.05)
etc...

G è invertibile:
![[Screenshot 2025-03-11 at 5.13.49 PM.png|400]]
Look-Up Table, tabella pre calcolata

input: U = 0.22 --> stimato facendo un average lineare

Pro:
- facile da realizzare
- intuitivo
- veloce da calcolare
Contro:
- stime introducono errori nei calcoli
Riduzione degli errori:
- curva può essere super campionata
- si possono usare valori di interpolazione di ordine più elevato invece che lineari


manca ancora il tempo

## Controllo della Velocità
Concetto Ease-In Ease-Out

Oggetto che ha massa non può accelerare o decelerare istantaneamente. (come una macchina)

![[Screenshot 2025-03-11 at 5.23.58 PM.png|400]]

**Funzione tempo-distanza** $s(t)$ che per un dato tempo t, produce la distanza percorsa lungo la traiettoria

La funzione tempo-distanza può essere specificata in un modo grafico o in modo analitico
- La funzione dovrebbe essere monotona in T (no tornare indietro nel tempo)
- La funzione non può essere discontinua

Procedimento:
- fisso t, fotogramma
- t --> s(t) lunghezza d'arco tramite funzione tempo-distanza
- s --> u = Gˆ-1 (s) stima lunghezza d'arco
- calcolo x y z tramite f(u) parametrico

## Ease-In  / Ease-Out

tratto di curva in accelerazione, un tratto centrale con pendenza 45 gradi costante, un tratto finale di decelerazione, fermandosi parallela all'asse x

Controllando la pendenza --> si possono soddisfare le richieste come accelerazione e decelerazione del moto di un oggetto

### Interpolazione Seno
Un modo di realizzare la funzione ease è di approssimare tramite curva del seno tra -n/2 e n/2 e poi mappare il range della funzione seno \[-1, 1] in \[0, 1]

![[Screenshot 2025-03-11 at 6.05.24 PM.png|500]]
![[Screenshot 2025-03-11 at 6.05.58 PM.png|500]]

teoricamente non è mai costante, ma accelera e decelera sempre --> modifica
![[Screenshot 2025-03-11 at 6.07.25 PM.png|500]]

Ease-in/Ease-out parabolico
- per evitare la valutazione di trascendenti o creazione di tabelle di look-up equivalenti si possono fare assunzioni riguardo l'accelerazione
- L'utente può specificare dei parametri che permettono di identificare una curva velocità tempo che possa essere facilmente integrata per ottenere la funzione tempo-distanza

![[Screenshot 2025-03-11 at 6.16.44 PM.png|300]]

![[Screenshot 2025-03-11 at 6.17.16 PM.png|300]]

L'accelerazione non è per forza costante, quindi il metodo è limitato

Orientamento: altri 3 gradi di libertà
Parliamo di corpo rigido che si muove lungo una traiettoria: equazioni meccanica del volo
- w = view vector --> dove guarda l'aereo
- v = up vector --> testa in su o giù
- u = w X v --> prodotto dei primi due

Frame di Frenet: sistema di coordinate (u, v, w) destrorso determinato dalla tangente e dalla curvatura della curva
- W = versore corrispondente alla derivata prima
- U = prodotto vettoriale della derivata prima e derivata seconda
- V = prodotto vettoriale di W e U
![[Screenshot 2025-03-11 at 6.27.17 PM.png|400]]

Problemi:
- come calcolare l'orientamento quando la seconda derivata è nulla?
- come calcolare la derivata seconda in presenza di discontinuità?
- spesso vettore tangente w non rappresenta la "naturale" direzione di vista


Orientamento mediante **centro di interesse**

metodo più semplice per ovviare ai problemi del frame di Frenet
COI = Center Of Interest
- W = COI - POS
- U = W X Asse-Y (W prodotto uno degli assi del moto, convenzione)
- V = U X W
Serve che l'utente specifichi un COI

Non è vietato che sia il COI che la POS sia in movimento
- w = C(s) - P(s)
- u = w x (U(s) - P(s))
- v = u x w


Raccordare una traiettoria

una traiettoria si può ottenere mediante una serie di punti, ottenuti da un processo di digitalizzazione
La curva risultante può essere troppo brusca (jerky) per formare una traiettoria realistica

raccordata per media di punti adiacenti:
![[Screenshot 2025-03-18 at 4.30.53 PM.png|500]]

Nel caso i dati siano visti come valori di una funzione, lo smoothing può essere effettuata mediante convoluzione.
Un kernel di smoothing può essere applicato ai punti immaginandoli come una funzione a gradini

![[Screenshot 2025-03-18 at 4.32.37 PM.png|400]]

Smoothing con kernel a convoluzione --> il kernel da l'importanza dei punti
attributi:
- centrato nell'origine
- è simmetrico
- ha supporto finito
- l'area sottesa dalla curva di kernel è unitaria

![[Screenshot 2025-03-18 at 4.33.50 PM.png|500]]

Centrando il filtro di convoluzione sulla posizione da calcolare
calcolando l'area ottenuta dalla moltiplicazione della funzione di kernel g(u) con il corrispondente segmento della funzione a gradini f(x)
si traduce in un integrale:

![[Screenshot 2025-03-18 at 4.36.16 PM.png|500]]

nel caso di un kernel box e tent, la convoluzione si traduce in una media pesata


Integrare equazioni differenziali ordinarie (ODE) significa, in generale, avere a disposizione la derivata prima di una funzione e voler approssimare numericamente la funzione f(x)
Il metodo più semplice per risolvere ODE è quello di Eulero:
$$
y_{n+1} = y_n + h \cdot f'(x_n, y_n)
$$
dove h è il passo temporale tale che 
$$
x_{n+1} = x_n + h
$$

Il metodo di Eulero non è simmetrico, usa info all'inizio del passo temporale per avanzare verso la fine del passo temporale stesso.
La derivata all'inizio del passo temporale produce un vettore tangente alla curva rappresentante la funzione in quel punto a quell'istante
La tangente all'inizio dell'intervallo è usata come un approx lineare del comportamento della funzione nell'intero intervallo

Eulero va bene se: variabile cambia poco, e quando l'intervallo di variazione è piccolo

![[Screenshot 2025-03-18 at 4.40.43 PM.png|400]]

Metodo di Runge-Kutta:
famiglia di metodi simmetrici di diverso ordine
più usato quello del quarto ordine
richiede più calcoli rispetto a Eulero, ma è più preciso e permette intervalli h più grandi richiedendo meno passi di integrazione

![[Screenshot 2025-03-18 at 4.43.07 PM.png|400]]

# Collisioni

Corpo che si muove in un ambiente popolato da altri oggetti, rischia di collidere con essi
Nella collision detection devono essere gestiti due problemi:
- Rilevare le collisioni --> problema tipico della cinematica che coinvolge posizione e orientamento degli oggetti 
- Determinare la risposta appropriata degli oggetti alla collisione --> dinamica e calcolo forze che si generano a seguito della collisioni

Collision Detection
- Intersezione spazio-temporale
gli algoritmi di intersezione spazio-temporale son bassati sull'operazione di estrusione

![[Screenshot 2025-03-18 at 4.51.22 PM.png|500]]

indica un criterio per determinare la collisione di due oggetti, ma non indica come calcolare le estrusioni. il calcolo delle estrusioni è un operazione estremamente "costosa" e spesso porta a considerare approssimazioni lineari delle traiettorie


- Interferenza degli swept volume
volumi contenenti i punti occupati dagli oggetti durante il tempo di simulazione (time span)
![[Screenshot 2025-03-18 at 4.56.05 PM.png|400]]
Gli swept volume non sono sufficienti, ma indicano solo la possibilità di collidere (se non si intersecano allora non c'è collisione, se c'è bisogna verificare che il tempo sia uguale per entrambi)

Rilevamento di interferenze multiple
tecniche di rilevamento di interferenze multiple 
campionamento del tempo, per il test di collisione
![[Screenshot 2025-03-18 at 4.59.35 PM.png|400]]

tecniche di parametrizzazione delle traiettorie determinano esattamente l'istante della collisione esprimendo le traiettorie degli oggetti come funzioni del parametro tempo


tecniche di parametrizzazione delle traiettorie determinano esattamente l'istante della collisione esprimendo le traiettorie degli oggetti come funzioni del parametro tempo

L'efficienza dell'intero algoritmo di collision detection dipende dalla velocità del test di intersezione e da quante volte si deve applicare il test di intersezione

Le gerarchie di volumi limitanti si sono rivelate molto utili per ridurre il numero di test
- spesso possibile rilevare una non intersezione al primo livello della gerarchia
- è possibile ridurre la porzione di spazio da considerare per il test di intersezione
Se si rileva una collisione tra gerarchie limitanti si può procedere alla verifica della collisione tra oggetti incapsulati

Gerarchia con approccio bottom-up di volumi limitanti partendo dagli oggetti
si risparmia dal 70-90% di test, complessità da quadratica a logaritmica

- AABB Axis-Aligned Bounding Box
- OBB Oriented Bounding Box
- Sphere Tree (sfere non hanno problemi di orientamento)
- ...

Suddivisione dello spazio:
- Octree --> divisione in 8 se nel cubo ci sono n oggetti maggiori della soglia (iterativo)
- Griglie uniformi
- BSP

Andiamo a ridurre la difficoltà, complessità logaritmica


Collisione piano-particella e risposta cinematica:
considera una particella che si muove a v costante verso un piano stazionario con un certo angolo di incidenza
Il problema è quello di rilevare quando la particella collide col piano e rimbalza su di esso
tutti i punti del piano soddisfano l'equazione: $ax + by + cz + d = 0$
per punti di fronte al piano, l'equazione planare restituisce valori maggiori di zero

0 = 0 impossibile che venga trovato nella computazione dell'equazione (errori calcolatore)
controllare entro un certo intervallo

Metodo 1: Risposta cinematica
oggetto si muove con velocità media costante
ogni passo temporale $t_i$ controlla se $E(p(t_i))>0$ 
	la prima volta che  $E(p(t_i))<0$ significa che la particella ha colliso con il piano nell'istante tra $T_{i-1}$ e $T_i$
Nella risposta cinematica quando si rileva la collisione, la componente del vettore velocità della particella è invertita e sottratta due volte al vettore velocità per calcolare la velocità dopo il rimbalzo

![[Screenshot 2025-03-18 at 6.02.58 PM.png|400]]

Funziona quando l'urto è completamente elastico ($V_i = V_f$) quindi non è reale, c'è sempre dispersione di energia
Per correggere il problema:
![[Screenshot 2025-03-18 at 6.05.49 PM.png|600]]
K = coeff di smorzamento, tra 0 e 1.
- 0 = tutta la velocità viene assorbita nell'urto anaelastico, no rimbalzo
- 1 = tutta energia restituita nell'urto elastico

Metodo 2: Metodo della penalità
prevede che un sistema molla-smorzatore venga allocato tra il punto raggiunto dalla particella quando si è rilevata la collisione e il piano
La molla impartisce una forza, parallela alla normale alla superficie, di ampiezza pari a F = -k\*d (legge di Hooke).
![[Screenshot 2025-03-18 at 6.16.44 PM.png|500]]
Valore di massa m è attribuito al punto per determinare un accelerazione $a = F/m$ e quindi un vettore velocità uscente dal piano che deve combinarsi con il vettore velocità
Problemi:
- stabilire una massa arbitraria m
- stabilire un valore per la costante k della molla
Se i parametri sono errati, la collisione non apparirà realistico



PAUSE, BLOCCO 3 PER DINAMICA CORPO RIGIDO



Forze di impulso di collisione