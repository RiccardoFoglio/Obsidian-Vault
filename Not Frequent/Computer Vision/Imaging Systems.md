
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

unsharp mask : può rendere dettagli più visibili
![[Screenshot 2025-05-14 at 6.30.53 PM.png]]
risalto anche il rumore con la mask

basso diaframma = immagine più sharp

ingrandimento lente:
![[Screenshot 2025-05-14 at 7.03.32 PM.png]]

oggetto: 50mm, voglio vederlo con precisione 1/10 mm (100 micron)
u = distanza oggetto - obbiettivo (25cm) --> $u = f(1+ \frac{1}{m})$
v = distanza obbiettivo - sensore (focale obbiettivo, es 25mm, 50mm ecc...) $u = f(1+m)$

$D = u+v = f(2+m+\frac{1}{m})$
ingrandimento: $m = \frac{v}{u}$

grandezza sensore: data dal datasheet della camera
grandezza del pixel: data dal datasheet della camera (1.25 micron)

calcola da grandezza sensore e grandezza pixel se l'oggetto ci sta:

- ingrandimento sistema lente: $m = v / u = 25mm / 25cm = 0.1$ 
- --> 50mm diventano 5mm su sensore
- controllo grandezza sensore per pixel (6.25mm, ci sta con un po' di margine)
- precisione richiesta: 100micron --> nel sistema lente diventano 10micron --> circa una decina di pixel bastano per rappresentarla

la formula non funziona del tutto perchè la focale cambia leggermente, l'unico modo per vedere quanto è grande su sensore l'oggetto è tramite il metodo sperimentale:
- conta quanti pixel occupa, e sai quanto è grosso 1 pixel

![[Screenshot 2025-07-06 at 3.14.29 PM.png]]

MTF_occhio = 1/CTF_occhio normalizzato a 1
CTF dipende dall'area della figura in gradi --> più grande è l'area più piccola è la CTF, l'occhio ha MTF più grande quando l'area della figura è grande

frequenza di risoluzione = intersezione tra MTF_system e CTF_eye --> dipende dalla grandezza del display e la distanza da esso

SQF = MTF del sistema calcolato tra le due frequenze f1 e f2
![[Screenshot 2025-07-06 at 3.30.11 PM.png]]
calcolato su tanti osservatori e si fa una media

per giudicare la qualità delle lenti: ci si basa sul MTF
- meglio avere MTF più basso sulle basse frequenze per dare senso di nitidezza (vista da occhio)
- altri calcoli basati sul MTF

stampante: MTF ridotto se l'inchiostro viene sovrapposto durante la stampa --> sharp mask per interpolare valori e ridurre punti vicini


IMATEST è il programma industriale che si usa per il calcolo dell'aberazione e altro
-  mettendo oggetto davanti a obbiettivo: poco effetto, mentre ridurre diaframma ha grossi effetti

come si forma l'immagine: raggi perpendicolari incontrano reticolo sinusoidale --> deviati a seconda dell'angolo con l'onda --> incontrano la lente e si centrano nel fuoco della lente
alla distanza focale della lente si forma la trasformata di fourier nel fuoco della lente davanti al sensore
sul piano di fuoco troviamo più ordini (più punti: ordine 0, ordine 1 sopra e sotto) e spostando lo schermo (sensore) di X cm dal piano (calcolabile con le formule) si ottiene un punto solo


Trasformata di Fourier di un apertura circolare --> disco di diffrazione (airy disk) ed anelli
gli anelli di diffrazione si possono evitare apotizando l'apertura (gaussian filter clear al centro e scuro agli angoli)
apertura grande --> trasformata compatta
apertura piccola --> pattern più grande
trasformata composta da: magnitudine e fase
- magnitudine = simmetrie verticali e orizzontali con la frequenza 0 al centro. Low pass, High pass, Band pass filters possono essere usati per oscurare la magnitudine.
  contiene info sul colore
- Fase = contiene info sulla posizione

traslazione non importante sulla magnitudine perchè le info di posizione sono sulla fase, mentre la rotazione influisce anche la magnitudine

La trasformazione di Fourier trasforma segnali in somme di seni con ampiezze e fasi diverse --> numeri complessi



disco di airy: luce passa da fessura e si creano cerchi concentrici di intensità, dati dagli angoli di incisione della luce con la fessura: centrale più forte, ai lati lunghezze diverse e si possono avere interferenze distruttive che diminuiscono l'intensità
visibile solo con la giusta esposizione e campionamento
diametro dato da 2.44 * lambda * f_number, qualsiasi punto più piccolo del airy disk non può essere messo a fuoco
la dimensione cambia in base all'apertura (diminuisce alla crescita) e aumenta con la lunghezza d'onda
non-mirror based systems soffrono di aberrazione cromatica --> alcune lunghezze d'onda non a fuoco

una lente perfetta soddisfa 3 condizioni:
- tutti i raggi si incontrano in un punto
- piano ortogonale (oggetto) è riprodotto come un piano ortogonale (immagine)
- oggetto e forma restano identici anche con magnification

se la lente è perfetta su due distanze lo è per tutte

ray tracing usato per fare design e verificare delle lenti
legge di snell: 
$$
n_1 \sin \theta_1 = n_2 \sin \theta_2
$$
seno si espande con Taylor -->
-  $\sin \theta = \theta$ approssimazione parassiale, primo ordine
-  $\sin \theta = \theta - \thetaˆ3/3$ terzo ordine

aberrazioni di terzo ordine: 5 monocromatiche e 2 cromatiche
- Spherical aberration (SA) : blur edge image, spherical lens on camera causes light near edge of lens to converge closer to lens. non correggibile
- Coma : light rays from a point that pass thru lens at an angle, with coma they dont refocus to a point but flare out from it, makes points of light look like a comet with blurred tails
- Astigmatism : lens fails to focus image lines running in different directions in the same plane (rail fence with vertical posts are sharp at a focus setting different from horizontal rails)
- Curvature of Field : parallel rays reaching lens from different directions don't focus on a plane but on a curved surface
- Distortion : changes in shape of the image, caused by different wavelengths of light and degrees of refraction

Chromatic Aberration: lunghezza focale dipende dal indice rifrattivo, ed è inversamente proporzionale alla lunghezza d'onda
Newton pensava non si potesse compensare

- Longitudinal Chromatic Aberration (LCA) : lens cannot focus different colors in the same focal plane
- Transverse Chromatic Aberration (TCA) : size of the image changes with wavelength, colors focus on separate points in a vertical plane

Aberration-limited lens: lente con bassa aberrazione.
SA, coma e astigmatismo non producono immagini stigmatiche
curvature of field da una curvatura alle immagini
distorsione produce immagini non identiche

una lente corretta per SA e coma: *aplanic*
ulteriore correzione per astigmatismo: *anastigmat*

per aggirare le aberrazione, si può usare la macchina foto senza obbiettivo: pinhole
- nessuna distorsione
- no curvatura di campo
- piccola aberrazione cromatica 
ma
- il risultato ha astigmatismo
- non otticamente sharp

soluzione per le aberrazioni cromatiche è l'uso di uno specchio perchè riflette le onde in un unico raggio

una camera usa lenti. una lente è un elemento ottico fatto di vetro che diverge o converge i raggi di luce in un punto. 
lenti convergenti convergono i raggi in un piano focale ad una distanza f dalla lente
lenti divergenti divergono i raggi

![[Screenshot 2025-07-07 at 5.35.33 PM.png]]

