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

funzione tempo-distanza $s(t)$ che per un dato tempo t, produce la distanza percorsa lungo la traiettoria

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


Orientamento mediante centro di interesse
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


