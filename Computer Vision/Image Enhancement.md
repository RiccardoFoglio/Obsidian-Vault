Spatial domain $g(x,y) = T[f(x,y)]$
Il vicinato può essere il punto stesso o più raffinata e comprendere anche dei valori confinanti al quadrato
![[Screenshot 2025-03-31 at 12.52.52 PM.png|400]]


Thresholding: funzione di trasferimento
![[Screenshot 2025-03-31 at 12.54.41 PM.png|400]]
scuro sotto k e chiaro sopra
Utile per separare le zone chiare da quelle scure

Funzioni di Trasformazione
$s = T(r)$
![[Screenshot 2025-03-31 at 1.03.09 PM.png|400]]

le parti luminose e molto scure non vanno subito ai loro massimi valori, per evitare di saturarle subito.


Log Transform:
![[Screenshot 2025-03-31 at 1.06.12 PM.png|500]]
$s = c \log(1+r)$ dove c è la costante e $r \ge 0$ 
un range piccolo di valori bassi viene mappato ad un range ampio alto
si vedono meglio i dettagli

Gamma Correction
![[Screenshot 2025-03-31 at 1.09.45 PM.png|500]]
L'occhio umano non è lineare, raddoppiare la luminosità non rende il doppio luminosa la percezione all'occhio, serve una gamma correction
CRT monitor ha valori gamma tra 1.8 e 2.5
(Monitor a tubi catodici) 
![[Screenshot 2025-03-31 at 1.11.13 PM.png|400]]
sul monitor si vede tutto più scuro
allora gamma correction --> quando si inscurisce diventa giusta

Utile anche per vedere più dettagli


 
Contrast Stretching
![[Screenshot 2025-03-31 at 2.27.19 PM.png|400]]
istogramma molto condensato, per migliorarlo si applica questa trasformazione per espanderlo un po'

si può esaltare anche specifici range
![[Screenshot 2025-03-31 at 2.29.58 PM.png|400]]


Bit-plane Splicing
![[Screenshot 2025-03-31 at 2.30.30 PM.png|400]]
scala di bit di un immagine
![[Screenshot 2025-03-31 at 2.30.51 PM.png|400]]
l'uso dei bit che sembrano non avere info è essenziale per ricostruire le immagini con il maggior numero di info possibili
![[Screenshot 2025-03-31 at 2.32.24 PM.png]]
certe tecniche usano questi bit poco significativi per inserire informazioni all'immagine


# Istogramma

funzione discreta, per ogni livello di grigio/canale sulle X, si ha un valore del livello in Y.
è normalizzato
![[Screenshot 2025-03-31 at 3.36.08 PM.png|400]]

Equalizzazione di un Istogramma: serve una funzione di trasformazione

$S_k = T(r_k) = \sum p_r(r_j) = \sum \frac{n_j}{n}$ 

è importante che la trasformazione si invertibile

Lo scopo dell'equalizzazione è passare dal istogramma di sx a quello di dx:
![[Screenshot 2025-03-31 at 3.41.33 PM.png]]

distribuzione e valori di un istogramma di 3-bit, 64x64 digital image
![[Screenshot 2025-03-31 at 3.41.59 PM.png|400]]

originale / funzione di trasformazione / istogramma equalizzato
![[Screenshot 2025-03-31 at 3.42.53 PM.png]]


