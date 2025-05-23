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

 dato un istogramma iniziale e uno desiderato, è possibile ricavare la funzione di trasformazione che si avvicina al risultato sperato.

1. mappa i livelli dell'immagine originale a partire dall'istogramma e ottengo la cumulata
2. calcola la funzione G dal istogramma desiderato, ottengo la cumulata
3. inverto G
4. applico G inverso all'immagine di partenza


Si può svolgere anche l'equalizzazione solo su una parte locale dell'immagine.
![[Screenshot 2025-03-31 at 4.27.21 PM.png|500]]


Mathematical Operations
- Image subtraction
	- used in segmentation and enhancement
- image addition
- image division
	- multiplication of one image by reciprocal of the other
- image averaging
	- used for contrasting noise

FILTRI
Linear Spatial Filtering
Data un operatore invariante di posizione lineare $h(x,y)$
1. Point Spread Function (PSF) $h(x,y)$ è la risposta del sistema all'impulso (punto di luce)
2. Fourier Transform di $h(x,y)$ è $H(u,v)$ ed è la optical transfer function

$h(x,y)$ si chiama anche spatial convolution matrix mask se è simmetrica

![[Screenshot 2025-03-31 at 4.42.43 PM.png|400]]

(il totale della somma dei valori dentro alla matrice deve essere 1)

in Photography: resulting image at the sensor level = convolution of the original image with the finite aperture of the lens.
- if the original image is a very little point = result is in focus
- if lens out of focus the result is much larger (bokeh)

Averaging Filter Masks
![[Screenshot 2025-04-04 at 11.48.20 AM.png|500]]

Non linear Filtering: Median Filter 
- median is the numerical value separating higher half from the lower half
- median filter computes the median under the mask and uses it as a resulting value
- it's very effective for some images
- there exist also max, min and mean filters

Sharpening spatial filters : Derivatives
![[Screenshot 2025-04-04 at 2.35.18 PM.png|500]]

Laplacian
![[Screenshot 2025-04-04 at 2.36.42 PM.png|500]]

![[Screenshot 2025-04-04 at 2.46.24 PM.png|300]]

In order to get the final image the laplacian is added or subtracted to the image


Unsharp masking and High Boost Filtering

unsharp mask is the equivalent of dark room technique based on printing together positive with a blurred negative

![[Screenshot 2025-04-04 at 2.49.08 PM.png|400]]

Amount : how much the contrast has to be incremented
Radius : how large is the area of modified pixels around starting pixel
Threshold : minimum difference between pixels for applying filter

First Derivative : Gradient
![[Screenshot 2025-04-04 at 2.50.51 PM.png]]


--------------------------------

In fotografia l'immagine risultante è la convoluzione dell'immagine originale con l'aperture della lente
- se l'originale è piccola (1 micron) il risultato è una figura a fuoco
- se la lente è fuori fuoco il risultato è più grosso (bokeh)


somma dei prodotti nella matrice va smorzata da una costante per renderla average
	Averaging filter masks --> smussa gli angoli
![[Screenshot 2025-05-14 at 6.09.30 PM.png]]
	Median Filter --> calcola mediana nell'area, il valore che divide metà alta da metà bassa (diversa da media), usata per correggere il rumore
![[Screenshot 2025-05-14 at 6.09.11 PM.png]]
	Sharpening filter --> derivata
![[Screenshot 2025-05-14 at 6.12.47 PM.png|400]]
Laplacian mask : aggiunta se il centro coeff è positivo, o sottratta se è negativo