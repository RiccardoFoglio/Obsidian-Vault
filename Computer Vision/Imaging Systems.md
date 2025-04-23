
![[Screenshot 2025-04-04 at 3.07.31 PM.png]]

Macrocontrast has max and min on the whole image
Microcontrast has values on limited areas
When it's impossible to see the bars of bar tests, contrast is 0

l'occhio umano riesce a vedere le frequenze intermedie ma quelle alte e basse poco o niente.

USAF Test 1951: how many lines are visible on the test chart? is there aliasing?

cromo su cristallo con sbarrette rettangolari, serve per testare la precisione della telecamera.



ISO12233 : standard internazionale usato per testare dispositivi ottici
- serve una stampa il più preciso possibile
- effetti di aliasing e blending delle righe
I valori = 100 lines per picture height

Misura: pixel dal top bianco al bottom bianco --> misura max della camera / misura sui pixel --> moltiplica per il valore visto sulla mira

MTF misura quanto un sistema riproduca fedelmente l'input
(Modulation Transfer Function) --> rapporto tra modulazione d'uscita e d'ingresso
quanto è forte ed attenua il resistore
si basa sulla frequenza
	h(x,y) è la risposta del sistema ottico all'impulso e l'immagine risultante è il Point Spread Function (PSF)

Trasformata di Fourier porta h(x,y) in H(u,v)

OTF (optical transfer function) è uguale al' $H_{spatial}$ se ignoriamo gli effetti dell'elettronica
$$
OTF(u,v) = H_{spatial}(u,v) = MTF(u,v) eˆ{jPTF(u,v)}
$$
dove MTF è la magnitudine e jPTF è la fase

$H_{ELECTRONICS}(f_e) = MTF(f_e) eˆ{jPTF(f_e)}$

Molto spesso OTF = H = MTF perchè la sia parte PTF spesso è irrilevante
è possibile che OTF sia negativa quando ci sono shift di focus o aberrations

le frequenze elettroniche possono essere convertite

 l'MTF delle lenti solitamente non può essere calcolato solo con la moltiplicazione degli MTF singoli perchè dovrebbero essere indipendenti tra loro, ma se si va ad unire più lenti allo stesso sistema è quasi una certezza che le aberrazioni vadano ad interferire tra loro


misura dei diaframmi: lunghezza focale / numero di diaframma --> 50mm / F2 = 25mm di diaframma
indica quanta luce entra ed esce nella macchina fotografica

tempo di posa : quanto tempo resta aperto l'otturatore (1/200, 1/1000 ecc...)

pellicola più sensibile: permette di fotografare meglio al buio. nel caso della pellicola la sensibilità era un parametro 

adesso in digitale non lo è più, i pixel son sempre grandi uguali
la corrente viene misurata e amplificata attraverso gli ISO

I migliori sensori digitali (Sony) sono iso invarianti, cioè se si aumenta l'esposizione, il risultato è analogo ad averlo fatto in camera

Gli unici parametri sono quindi diaframma e tempo di posa

MTF Computation oggettivo:
- segnale sinusoidale : (usare una mira e vedere da camera fino a dove si distinguono le linee)
- slant edge target : 

$MTF = M_{OUTPUT} / M_{INPUT}$

campionare il passaggio da bianco a nero di un edge e calcolare la risposta di impulso

si ruota di 5,7 gradi l'edge per avere più pixel interessati nel calcolo del passaggio, il campionamento è più curato e il calcolo viene meglio

![[Screenshot 2025-04-23 at 3.57.17 PM.png|500]]

![[Screenshot 2025-04-23 at 3.58.41 PM.png|500]]

programma standard per calcolare l'MTF : Imatest, ma costa troppo