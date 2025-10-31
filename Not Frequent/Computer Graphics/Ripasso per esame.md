
# CH1 - Intro

30% cervello dedicato ad elaborazione di info visive --> occhi = canale di comunicazione con banda maggiore per cervello

Informatica Grafica = tecniche e metodi per rappresentare sotto forma di stimoli sensoriali + elaborare e manipolare info digitali

Computer Graphics = aspetti di sintesi di informazioni visive (sensoriali) mediante calcolatore. tecniche di rendering. Immagine è il prodotto finale che parte dai dati

Computer Vision = analisi di immagini, immagine è punto di partenza e finisce in dati

![[Pasted image 20240924133143.png|500]]
![[Pasted image 20240924133206.png|500]]
![[Pasted image 20240924133223.png|500]]
## Rendering

Dato un cubo centrato nell'origine, di dimensione 2 per lato, spigoli allineati con assi x,y,z
Rappresentazione vertici:
	A: ( 1, 1, 1)      E: ( 1, 1, -1) 
	B: ( -1, 1, 1)     F: ( -1, 1, -1) 
	C: ( 1, -1, 1)     G: ( 1, -1, -1) 
	D: ( -1, -1, 1)   H: ( -1, -1, -1) 
Rappresentazione spigoli:
	AB, CD, EF, GH, 
	AC, BD, EG, FH, 
	AE, CG, BF, DH

Per passare da 3D a 2D:

- mappare vertici 3D in punti 2D --> proiezione prospettica, modello pinhole camera (+ lontano --> + piccolo)
  ![[Pasted image 20240924133828.png|400]]![[Pasted image 20240924134333.png]]

### Algoritmi di Rendering

scegliere posizione camera (es: C = (2,3,5) )
per ogni spigolo:
- convertire (x,y,z) in (u,v)
	- sottrai coordinate C dal vertice (x,y,z)
	- dividi (x,y) per z per ottenere (u,v)
- Disegnare segmento (u1,v1), (u2,v2)
![[Pasted image 20240924134926.png|400]]
![[Pasted image 20240924134940.png|400]]

Online Rendering
- Interattivo: 1-10 fps
- Real-Time: 10-100 fps
Offline Rendering

## Storia
Necessità di rappresentare semplicemente e flessibilmente dati elaborati dai calcolatori
larga diffusione ostacolata da costi hardware grafico, elevate risorse computazionali, difficoltà nello sviluppare programmi interattivi, software specifico per hardware grafico

anni 50: tubi catodici
Anni 60-80: grafica vettoriale
Anni 70: diffusione sistemi raster --> matrice rettangolare (raster) di elementi (pixel). Scansione raster: fascio di elettroni spazza il CRT in modo sequenziale tot volte al secondo (refresh rate)
## Grafica Raster

illuminare pixel in modo opportuno per formare immagine
modulare stimolazione degli elementi luminosi durante la scansione per ottenere colore
necessario memorizzare valori di intensità (colore) da assegnare ad ogni pixel

- area di memoria dedicata al display = frame buffer
- 1280x1024x24bit = 3,75MB

Pros: costi inferiori, possibilità di gestire aree "filled"
Cons: natura discreta dei pixel, conversione primitive grafiche in pixel nel frame buffer (rasterizzazione), approssimazione (aliasing)

Rasterizzazione = quali pixel illuminare per rappresentare un immagine tramite pixel?
![[Pasted image 20240924193252.png|500]]


1. illuminare i pixel intersecati dalla linea
![[Pasted image 20240924193323.png|500]]

2. regola (test) del diamante
![[Pasted image 20240924193358.png|500]]

### Approccio Incrementale

```C
v = v1
for(u=u1; u<=u2; u++){
	v+= s;
	draw(u, round(v));
}
```
![[Pasted image 20240924193802.png]]

---
# CH2 - Sistema Visivo Umano

influenzato da:
- percezione colore
- acutezza, capacità dell'occhio di risolvere e percepire dettagli fini
- percezione profondità
- livelli di luminosità
- variazioni temporali
- campo visivo (FOV)

Risoluzione spaziale = acutezza di risoluzione, fondamentale per progettazione di dispositivi output grafici. dipende da luminosità e contrasto
Inutile creare output grafici più fini dell'occhio umano, non viene percepito, costo inutile
Risoluzione occhio: 20"x24" = 6000x6000 pixel, gli schermi solitamente sono 70-100 dpi

Risoluzione Temporale = Frequenza critica del flicker (CFF), sopra la quale non si è in grado di osservare effetti di sfarfallio o flicker (solitamente 35-60Hz)
2 aspetti:
- Refresh rate --> frequenza di aggiornamento superiore a CFF per evitare flicker
- animazioni fluide --> frame rate, frequenza di aggiornamento superiore a CFF

Campo Visivo (FOV): occhio singolo = 150 gradi, combinati 180-200 con sovrapposizione centrale
![[Pasted image 20241003180556.png]]

luce visibile tra 430nm (violetto) a 790 nm (rosso), con picco a 559nm
![[Pasted image 20241003180905.png]]

livelli di luminosità percepibili: da pochi fotoni a tanto di più, nessuno schermo replica il range percepibile, occhio effettua un adattamento per operare su questo range
percezione dipende anche da luminosità sfondo

esperimenti: pochi livelli diversi possono essere riconosciuti in un immagine monocromatica, ma il movimento rapido dell'occhio --> intensità sfondo cambia --> percezione diversa di livelli

numero di livelli in immagine dipende da range dinamico del dispositivo di visualizzazione
![[Pasted image 20241003192058.png]]

Effetto Mach = strisce verticali con tono di grigio omogeneo, percezione diversa. sistema visivo tende a esagerare o smorzare percezione al confine tra regioni con intensità diverse. Rilevante per ombreggiatura di superfici poligonali
![[Pasted image 20241003212322.png|300]]

Profondità : approccio basato su cues (suggerimenti), info 2D che fanno percepire immagine in 3D tipo ombre, occlusioni, prospettiva, gradienti texture, dimensioni di oggetti comuni

Info oculomotorie = convergenza e accomodazione (fissazione e messa a fuoco)
Info binoculari = disparità binoculare, stereopsi (capacità percettiva di unire immagini proveniente dai 2 occhi)
Info dal movimento = parallasse

Fisiologia occhio umano: sfera diametro 20mm
![[Pasted image 20241003214151.png|450]]
- **Cornea** = tessuto trasparente, copre superficie occhio
- **Membrana sclerotica** = tessuto opaco, ricopre resto bulbo oculare
- **Membrana coroidea** = ricca rete di vasi sanguigni, fonte nutrimento
- corona ciliare 
- **Iride** = contrae o allarga a seconda della luce, 2-8mm, apertura = pupilla
- **Corpo vitreo** = nutrimento occhio e forma sferica sotto pressione
- **Cristallino** = assorbe 8% luce visibile, assorbimento aumenta al diminuire della lunghezza d'onda. Infrarossi e ultravioletti quasi tutti assorbiti, eccessivi = danni
- **Retina** = formazione immagine tramite recettori di luce: coni e bastoncelli
	- Coni (CONES) = 6-7mln, parte centrale retina, altamente sensibili a colore ed elevati livelli di illuminazione, ognuno connesso a nervo --> dettagli più fini
	- Bastoncelli (RODS) = 75-150mln, connessi a gruppi di nervi (meno dettagli), visione generale campo di vista, sensibili a bassi livelli illuminazione

Cristallino != lente normale perchè è flessibile --> raggi di curvatura vengono controllati dalla tensione delle fibre del corpo ciliare, accomodamento per focalizzare oggetti a distanze diverse

Distanza focale: 14-17mm, quindi possibile calcolare dimensioni immagine su retina
![[Pasted image 20241003215243.png|500]]
h=2.55mm

---
# CH3 - Luce e Colori

Isaac newton (1676) luce attraverso prisma --> intuisce che è insieme di colori con indici rifrazione diversi

- Teoria corpuscolare (newton, 1600)
- Teoria ondulatoria (Huygens 1600) ed elettromagnetica (maxwell 1800)
	- onde luminose come onde radio, campi elettrici e magnetici si propagano sia nella materia che nel vuoto
- Meccanica quantistica (Plank + Einstein 1900)
	- fotone sia proprietà ondulatorie che particellari

LUCE = radiazione elettromagnetica
COLORE = frequenza di oscillazione / lunghezza d'onda della radiazione

calore genera moto particelle --> campo elettromagnetico --> oscillazione particelle aumenta con aumentare temperatura --> colori rossi per caldo (es: visione termica)

frequenza visibile: 400 (violetto) - 700 (rosso) nm

Luce acromatica = assenza di colore, parametro: quantità di luce
- termini di energia --> intensità / luminanza (luminance)
- termini di intensità percepita --> brillanza / brillantezza (brightness)

quantità di energia per diverse lunghezze d'onda --> distribuzione di energia spettrale
![[Pasted image 20241004121552.png]]

spettro emissione = quanta luce viene prodotta, utile per descrivere il colore di una sorgente
	intensità di luce in funzione della frequenza
spettro di assorbimento = quanta luce viene assorbita, descrive colore di vernice, inchiostro etc...
	colore percepito

Stesso colore ma più efficiente:
![[Pasted image 20241004121741.png|600]]

Colore = quantità di luce generata o assorbita, in funzione della frequenza
- assorbimento
- riflessione
- trasmissione
- rifrazione
- diffusione
- diffrazione
- polarizzazione

oggetto riceve energia luminosa --> assorbimento + riemissione a frequenza minore
- fluorescenza = riemissione immediata
- fosforescenza = riemissione ritardata

Rappresentazione colore -->
- spazio di colore : insieme di colori (spazio RGB)
- modello di colore : modo in cui colore viene specificato (HEX)

colore associato a 3 quantità (leggi di Grassmann)
- Tinta (HUE) --> colore puro, rosso verde ecc...
- Saturazione (saturation) --> bianco/grigio mischiato a colore puro
- Luminosità (lightness) --> intensità percepita per oggetti riflettenti
- Brightness --> intensità percepita di un oggetto che emette luce

Artisti specificano colori di
- Tinta (tint) --> pigmento bianco + pigmento puro = diminuisce la saturazione
- Sfumatura (shade) --> pigmento nero + pigmento puro = diminuisce luminosità
- Tonalità (tone) --> pigmenti bianchi + neri al pigmento puro

occhio umano percepisce:
$$
128\ colori\ saturi\ diversi\ \times 
130\ tinte\ (bianco\ aggiunto)\ \times
23\ sfumaure\ (nero\ aggiunto)\ =
380.000
$$

Distribuzioni di energia spettrale possono descrivere in modo compatto i colori:
- Lunghezza d'onda dominante --> colore percepito quando si guarda la luce, picco della distribuzione
- Purezza di eccitazione --> saturazione del colore, proporzione luce pura e luce bianca $e_2 / e_1$
- Luminanza --> intensità di luce, proporzionale all'area sottesa della distribuzione
![[Pasted image 20241004130502.png|600]]


per rappresentare un colore non è necessario riprodurre la sua distribuzione del mondo reale, ci son tanti modi per ottenere la stessa percezione

Servono solo 3 colori per ottenerne tanti altri --> spettri diversi possono produrre stesso colore

Teoria del Tristimolo: 3 tipi di coni nella retina
![[Screenshot 2025-08-19 at 4.48.21 PM.png]]

occhio: maggior sensibilità attorno ai 550nm --> verde giallo

ottenere tutti colori visibili --> combo di colori spettrali puri (RGB) --> tramite color matching

CIE (Commission Internationale de l'Eclairage) in 1931 definisce colori primari X Y Z invece che R G B, non sono colori reali, sono sempre positivi, Y = luminanza e corrisponde a funzione di efficienza luminosa dell'occhio umano

![[Screenshot 2025-08-19 at 4.56.14 PM.png]]
$$x = \frac{X}{X+Y+Z}\quad y = \frac{Y}{X+Y+Z}\quad z = \frac{Z}{X+Y+Z}$$

## Modelli di Colore
specifica sistema di coordinate di colori 3D, diversi modelli:
- XYZ
- CIELab
- RGB
- CMY(K)
- HSV/HSB/HLS
- YIQ
- YCbCr / Y'CbCr
### RGB
colore = terna di valori RGB
linea di grigi unisce nero e bianco
solo 3 elementi da controllare, facile per elettronica
![[Pasted image 20241008121733.png]]
Utente: difficile scegliere colore desiderato

 Sintesi Additiva dei Colori: mescolanza di stimoli di colore che entrano simultaneamente o in rapida successione nell'occhio
 ![[Pasted image 20241008122225.png]]
 Primari additivi: Rosso Verde Blu
 Complementari: Magenta Ciano Giallo

Media spaziale: piccoli punti colorati vicini non distinguibili dall'occhio
Media temporale: rapida successione

Halftoning: migliora aspetto immagine se limiti del sistema grafico non la rendono buona. ampie sfumature di uno stesso colore su sistema che non permette riproduzione di molti livelli

Sintesi Sottrattiva:
- luce emessa R+G+B = Bianco
- luce riflessa R+G+B = Non bianco (pigmenti)
sintesi sottrattiva di stimoli ha cause esclusivamente fisiche: colore 1 + colore 2 assorbe più luce, vediamo nero

Additiva vs Sottrattiva
![[Pasted image 20241008123554.png]]
### Modello CMY(K)
Stampa elettrostatica o getto d'inchiostro, Ciano Magenta Giallo (complementari di RGB)
![[Screenshot 2025-08-19 at 5.30.30 PM.png]]
Ulteriore pigmento nero (K) per evitare di sprecare altri CMY
### Modello HSV
Hue Saturation Value (brightness)
User-oriented, facile per l'utente, sistema coordinate cilindrico, cono 6 facce
trasformazione non lineare da RGB
![[Pasted image 20241008124059.png]]
### Modello HLS
Hue Lightness Saturation
Modello simile a HSV, doppio cono, più complicato
![[Pasted image 20241009111230.png]]
Limiti:
- ci si può muovere lungo coordinata senza ottenere effetti visibili
- coordinata luminosa dovrebbe essere indipendente dal colore
### Modello YIQ
Ricodifica RGB per trasmissione efficiente e retro-compabilità della TV a colori con TV in bianco e nero.
Y = luminanza (su schermi BW solo questo viene mostrato, retro compatibile)
IQ codificano cromaticità
![[Screenshot 2025-08-19 at 5.38.08 PM.png]]
### Modello YCbCr
Codifica video e fotografia digitale
Y = luma / Y' = luminanza
Cb = differenza dal verde nel campo del blu
Cr = differenza dal verde nel campo del rosso
![[Screenshot 2025-08-19 at 5.40.07 PM.png]]
## Codifica Colori
tipica codifica 8 bit per canale = 256 valori, cifre esadecimali : #ff6600
codifiche diverse:
- comodità = utente può scegliere velocemente il colore
- efficienza = possibile codificare in maniera diversa in base a quanto siano rilevanti dal punto di vista percettivo (compressione)
- elaborazione = rilevante lo spazio di colori in cui si effettuano operazioni di interpolazione o miscellazione (blending)
- riproducibilità = modelli diversi rappresentano insiemi di colori diversi

Spazio dei colori CIE --> spazio di riferimento, include tutti i colori visibili
proiezione sul piano x,y = diagramma di cromaticità
![[Pasted image 20241009112752.png]]
![[Pasted image 20241009112810.png]]
mostra tutti i valori di cromaticità visibili, i colori puri sono lungo il bordo
triangolo che si ottiene unendo RGB rappresenta i colori rappresentabili con RGB, fuori non si vedono.
due colori combinati = risultato si trova sul segmento che li unisce

GAMUT = gamma di colori rappresentabili(triangolo nel diagramma di cromaticità), può dipendere sia da hardware che software
![[Pasted image 20241009113701.png|550]]

**Ellissi di MacAdam**: utilizzate per valutare la acuità di colore, colori dentro ellissi non sono distinguibili da un umano
![[Pasted image 20241009113757.png]]

## Radiometria
misura delle grandezze fisiche relative ad una sorgente di radiazioni elettromagnetiche

- fotoni si propagano in linea retta, lunghezza d'onda della luce molto più piccola degli oggetti, ignorati fenomeni di diffrazione interferenza ecc...

fotone trasporta energia, info di luce

- Energia Radiante (# totale di impatti) = Joules (J)
- Flussio Radiante (# impatti / secondo) = Watts (W)
- Irradianza (# impatti / secondo per unità di superficie) = W / Mˆ2

luce direzionale : sorgente di luce infinitamente luminosa a distanza infinita, raggiunge superficie in ugual modo

Luce puntiforme, isotropica, in tutte direzioni

Irradianza: decresce in maniera quadratica con la distanza

Legge di Lambert: relazione tra irradianza e l'angolo tra la direzione della luce / orientamento superficie
	Irradianza è proporzionale al coseno dell'angolo tra direzione della luce e la normale della superficie

Grandezze radiometriche
- energia radiante = energia trasportata da un qualunque campo di radiazione magnetica
- potenza / flusso radiante = energia per unità di tempo rilasciata da una sorgente di radiazioni
- emettenza radiante = flusso radiante emesso da una sorgente estesa per unità di area
- irradianza = irradiamento/irraggiamento, flusso radiante incidente su superficie per unità di area
- intensità di radiazione = flusso radiante emesso da sorgente puntiforme in una direzione per unità di angolo solido
- radianza = flusso radiante emesso da una sorgente estesa per unità di angolo solido e per unità di area proiettata su piano normale

## Fotometria
studio dell'effetto delle radiazioni elettromagnetiche sul sistema visivo umano

ad ogni grandezza radiometrica corrisponde una grandezza fotometrica valutata secondo la risposta del sistema visivo umano

grandezze fotometriche:
- energia luminosa: corrisponde a energia radiante
- flusso luminoso: quantità di energia luminosa emessa da una sorgente nell'unità di tempo
- emittenza luminosa
- illuminamento
- intensità luminosa
- luminanza

---

# CH4 - Trasformazioni e Proiezioni

trasformazione geometrica = corrispondenza biunivoca che associa ogni punto P con P' appartenente allo stesso spazio
$$P'=f(P)$$
Trasformazioni lineari: preservano combinazioni lineari (somma, moltiplicazione) + preservano le linee e l'origine
- riflessione
- rotazione
- scalamento
- scorrimemento
facili da calcolare, composizione lineare, possibile rappresentare prodotto di più matrici come unica matrice
trasformazioni affini: preservano linee e distanze, es: rotazione e traslazione.

- **<font color="#00b050">Rotazione</font>** = ruotare un intero oggetto, si applica solo agli estremi perchè infiniti punti nel oggetto
centro di rotazione è importante
matriciale:
$$\begin{bmatrix} x' \\ y' \end{bmatrix} =
\begin{bmatrix} \cos\theta &-\sin\theta \\ \sin\theta &\cos\theta \end{bmatrix} \cdot 
\begin{bmatrix} x\\ y\end{bmatrix} $$
Proprietà:
$$\begin{bmatrix} \cos\theta &-\sin\theta \\ \sin\theta &\cos\theta \end{bmatrix} \begin{bmatrix} \cos\theta &\sin\theta \\ -\sin\theta &\cos\theta \end{bmatrix} =
R \cdot R^T =
\begin{bmatrix} 1 &0 \\ 0 &1 \end{bmatrix} = I$$

- **<font color="#00b050">Riflessione</font>**:
$$Q = \begin{bmatrix} -1 &0 \\ 0 &1 \end{bmatrix}$$
non viene preservato orientamento, ma ribaltato, solo origine
non tutte matrici per cui $Q Q^T = 1$ sono matrici di rotazione

- **<font color="#00b050">Scalamento</font>** = scalare di `a` lungo gli assi
$$\begin{bmatrix} x' \\ y' \end{bmatrix}
= \begin{bmatrix} a &0 \\ 0 &a \end{bmatrix} \cdot
\begin{bmatrix} x \\ y \end{bmatrix}$$
produce anche una traslazione se l'oggetto non è nell'origine del sistema di riferimento

- **<font color="#00b050">Scorrimento</font>** = spostamento di ogni punto x lungo la direzione u in base alla distanza da un vettore fisso v
$$\begin{bmatrix} x' \\ y' \end{bmatrix}
= \begin{bmatrix} 1 &T \\ 0 &1 \end{bmatrix} \cdot
\begin{bmatrix} x \\ y \end{bmatrix}$$

- **<font color="#92d050">Traslazione</font>** = spostare un punto (offset) del piano in un altra posizione
trasformazione affine ma non lineare, non rappresentabile in forma matriciale in coordinate cartesiane

Definita come prodotti di matrici
$$
\begin{bmatrix} x' \\ y' \end{bmatrix}
= \begin{bmatrix} a &b \\ c &d \end{bmatrix} \cdot
\begin{bmatrix} x \\ y \end{bmatrix}
$$
## Coordinate omogenee

- Traslazione = $P' = T + P$
- Scalamento = $P' = S \cdot P$
- Rotazione = $P' = R \cdot P$

coordinate omogenee derivano dallo studio della prospettiva, modo naturale per assegnare coordinate alle linee, essenziali nella grafica

considera ogni piano 2D che non passa per origine OPPURE in 3D
- ogni linea che passa per l'origine in 3D corrisponde ad un punto sul piano 2D (intersezione col piano)
  ![[Screenshot 2025-08-20 at 5.22.58 PM.png]]
- per i punti 2D si aggiunge una terza coordinata: $\hat p=(x,y,W)$ 
  due set di coordinate rappresentano lo stesso punto se sono multipli
  (0,0,0) non ammesso, almeno uno deve essere diverso da zero
  se W diverso da 0 --> $(x/W, y/W, 1)$

Usando queste coordinate omogenee è possibile rappresentare trasformazione 2D in una lineare 3D 
$$\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix}
= \begin{bmatrix} 1 &0 & d_x\\ 0 &1 & d_y \\ 0 & 0 & 1 \end{bmatrix} \cdot
\begin{bmatrix} x \\ y \\ 1 \end{bmatrix}$$
![[Screenshot 2025-08-20 at 5.27.53 PM.png|500]]

## Composizione di Trasformazioni

- 2 traslazioni = somma degli offset
- 2 scalamenti = prodotto dei fattori di scalamento
- 2 rotazioni = somma angoli

cambiando ordine delle trasformazioni si genera un risultato diverso, la prima è posta al fondo della composizione (destra)

Trasformazioni:
- Di Corpo Rigido / Euclidea --> preservati distanze ed angoli
- Di similarità / similitudine --> preservati angoli
- Lineari --> scalamento, rotazione, scorrimento
- Affini --> preservati parallelismo delle linee e rapporti di distanze
- Proiettive --> preservate linee
![[Pasted image 20241015131459.png]]

Risultato del prodotto di una sequenza di trasformazioni affini (rotazioni, traslazioni, scalamenti) è ancora una trasformazione affine.
- preservato parallelismo delle linee
- non le distanze ed angoli

Le matrici omogenee possono essere moltiplicate tra loro per ottenere le trasformazioni composte --> migliorare efficienza calcoli applicando una singola matrice

## Trasformazioni 3D

coordinate omogenee 3D --> 4 coordinate (x,y,z,W)
sistema destroso (Z uscente), perpendicolare allo schermo, rotazione positiva è antioraria
![[Pasted image 20241015132137.png]]

Rappresentazione angoli: necessari 3 gradi di libertà
- angoli di Eulero --> problema : gimbal lock, blocco cardanico
- alternativa : Quaternioni (numeri complessi, 4 coordinate)

## Cambio di Sistema di Riferimento

modo alternativo ma equivalente di guardare una trasformazione
![[Pasted image 20241015133555.png]]

## Camera Virtuale

Osservatore / camera virtuale
Metafora utile per studiare la creazione di scene 3D

Parametri di vista: specificano le condizioni sotto le quali si vuole vedere il mondo/modello

Proiezioni: trasformano punti in un sistema di riferimento di N dimensioni in un altro con meno dimensioni.
Proiezione definita dai raggi di proiezione o proiettori
Possono essere 
- prospettiche : centro di proiezione esplicito
- parallele : centro ad infinito, direzione di proiezione
![[Pasted image 20241015140209.png]]
![[Pasted image 20241015140233.png]]

proiezioni prospettiche sono definite da piano e centro di proiezione
scorcio prospettico: effetto di sistema fotografico / sistema visivo umano
non particolarmente utile per rappresentare la forma o dimensione degli oggetti

proiezione parallela = misure esatte, linee parallele, angoli restano intatti
### Punti di Fuga

proiezioni prospettiche convergono in un punto di fuga (vanishing point)
in 3D, le linee parallele si incontrano ad infinito
Punto di fuga interpretabile come proiezione di un punto ad infinito

Proiezioni parallele definite da direzione verso il centro di proiezione, piano di proiezione.
Generalmente non sono adatte per immagini foto-realistiche, ma utili per comprendere forme 3D
### Volume di Vista

Individua proporzione di mondo 2D su cui deve essere effettuato il clipping.
Proiezione sul piano di vista
Piramide semi-infinita con vertice nel centro di proiezione e lati passanti per gli spigoli della finestra
Approssimazione del cono di vista degli occhi

proiezione parallela : parallelepipedo infinito, con lati paralleli alla dir. di proporzione
Spesso interesse per volumi finiti (piani di clipping)

![[Pasted image 20241016133544.png]]

Clipping = rimozione delle primitive non visibili dalla camera, per evitare sprechi di risorse nella rasterizzazione
![[Pasted image 20241016133654.png]]

Invece di proiettare direttamente il volume di vista, si considera cubo unitario nel sistema di riferimento delle coordinate di proiezione / del disposivo normalizzate (NPC)
volume di vista trasformato in un parallelepipedo in NPC che si estende di $min$ e $max$ in ogni direzione --> **3D viewpoint**
![[Pasted image 20241016133907.png]]

Per definire la viewpoint 3D
- piano di clipping anteriore = $Z_{max}$;  piano clipping posteriore = $Z_{min}$
- lato $U_{min}$ = $X_{min}$; lato $U_{max}$ = $X_{max}$
- lato $V_{min}$ = $Y_{min}$; lato $V_{max}$ = $Y_{max}$
- faccia $Z = 1$ del cubo unitario è mappata sul quadrato più grande che può essere iscritto nell'area di visualizzazione

---

# CH5 - Primitive Grafiche, Pipeline e Rasterizzazione

Grafica Raster = primitive grafiche memorizzate in buffer in termini di pixel
immagine schermo formata a partire dal raster, insieme di scan lines orizzontali composte da diversi pixel
raster = matrice di pixel che rappresenta l'area dello schermo
letto in maniera squenziale (sx dx, alto basso)

Svantaggi: 
- natura discreta della rappresentazione pixel --> coordinate dei punti vanno convertiti in coordinate pixel
- natura raster --> sistema vettoriale può disegnare linee continue, raster deve approssimare
- aliasing = artefatto visivo frutto di errore di campionamento / sotto-campionamento

2 tecniche per la visualizzazione:
- Rasterizzazione : ogni primitiva = insieme di pixel accesi, veloce ma complicato raggiungere risultati fotorealistici, perfetta per grafica vettoriale, font ecc...
- Raytracing : ogni pixel = insieme di primitive, più facile raggiungere fotorealismo ma più lento
![[Screenshot 2025-08-22 at 3.13.45 PM.png]]

Pipeline:
- ingresso = immagine che si vuole disegnare
- stadi = sequenza di trasformazioni per passare da input ad output
- uscita = immagine disegnata
![[Pasted image 20241024183637.png]]

visualizzazione di immagini in tempo reale --> rasterizzazione (in: primitive 3D, out: immagine raster)

Librerie/API approssimano primitive matematiche, descritte in termini di vertici in un sistema di coordinate cartesiane.
Algoritmi per convertire primitive in pixel (scan conversion) + clipping, indipendenti dal dispositivo raster output. Adattabili ad implementazioni software e hardware
Invocati ogni volta che un immagine viene creata o modificata, devono creare immagini della qualità appropriata ma il più veloci possibili
- metodi incrementali nella scan conversion per minimizzare numero di operazioni
- aritmetica intera e non float
- esecuzione parallela

## Sistemi di Visualizzazione

libreria grafica = media tra hardware e applicazione
interfaccia verso l'hardware indipendente dal dispositivo
- output pipeline: app estrae descrizione oggetti in primitive e passa a libreria che farà scan conversion + clipping
- input pipeline: interazione utente lato hardware, usata per modificare immagine su schermo

Frame buffer - Controllore grafico : 
librerie effettuano scan conversion sia fuori dallo schermo che nel frame buffer
memoria condivisa tra librerie software e dati

Frame buffer + Controllore grafico : 
controllore implementa scan conversion, minimo sforzo per librerie

Sistemi di sola visualizzazione: semplici, accettano una scan line alla volta (esempio stampante su carta)
Dispositivi più intelligenti accettano un intero fotogramma / pagina

## Clipping

- clip prima della scan conversion, calcola intersezioni coi confini della regione di clipping
- forza bruta : scissoring dopo la scan conversion, scritti solo pixel visibili (verifica tutte le coordinate dei pixel con i confini del rettangolo di clip)
- generare intera collezione di primitive e usare copyPixel sul rettangolo di clip

## Rasterizzazione

### Linee

algoritmo che calcoli coordinate dei pixel che giacciono su una linea retta su matrice raster 2D
sequenza pixel vicina alla linea ideale e + dritta possibile e + veloce possibile

data una linea di spessore unitario, monitor CRT con sezione circolare e spaziatura variabile
![[Screenshot 2025-08-22 at 3.50.35 PM.png|400]]
#### Algoritmo Base:
- $m = \Delta y / \Delta x$
- incrementa x di 1 a partire dal punto più a sx
- calcola la retta $y_i = m \cdot x_i + B$ per ogni $x_i$
- illumina pixel $(x_i, round(y_i))$

Selezionato il pixel più vicino alla linea (distanza tra pixel e linea ideale è minima)

ottimizzato rimuovendo la moltiplicazione : $y_{i+1} =m\cdot x_{i+1}+B = m(x_i+\Delta x)+B = y_i + m$ perché $\Delta x = 1$
allora si ha che $y_{i+1} = y_i + m$
#### Algoritmo Incrementale:
- inizia con coordinate intere di un estremo $(x_0, y_0)$
- no parametro B esplicito
![[Pasted image 20241024190539.png|400]]

Se $|m| > 1$, incrementando x si ottiene incremento su y maggiore di 1 
Algoritmo Digital DIfferential Analyzer (DDA) : strumento per risolvere sistemi di equazioni usando metodi numerici, ma variabili reali e precisione limitata
#### Algoritmo di Bresenham
basato su aritmetica intera, evita round()
permette di calcolare prossimo pixel da quello precedente
![[Pasted image 20241024191544.png|400]]
Scelto P bisogna decidere tra
- incremento destra (E)
- incremento destra + alto (NE)
Q = intersezione linea ideale con $x = x_p + 1$
scegliere NE o E in base alla distanza minore con Q
#### Formulazione Midpoint
si osserva da che lato giace M (midpoint tra NE ed E) rispetto alla linea
- se M sotto linea è vicina a NE
- se M sopra linea è vicina a E
errore è sempre 1/2 in questa formulazione

sufficiente calcolare $F(M) = F(x_p+1, y_p+1/2)$ e testare il segno
### Cerchi
per disegnare circonferenza si può risolvere equazione implicita $y = f(x) = \pm \sqrt{R^2-x^2}$ ma è inefficiente per via delle moltiplicazioni e radici quadrate

algoritmo midpoint: circonferenza, punti generati e scelti in base a midpoint tra due pixel più vicini alla linea
![[Screenshot 2025-08-22 at 4.28.45 PM.png]]
sfruttando simmetria, dato un punto si possono tracciare gli altri 7 specchiati sugli assi, basta calcolare un arco di 45 gradi
### Poligoni
Algoritmo utilizzabile sia per convessi che concavi, determina come trattare gruppi/intervalli di pixel (span) compresi tra i lati sx e dx del poligono

pixel sul contorno dello span calcolati con approccio che determina intersezione tra lato poligono e scanline, per mantenere coerenza

![[Screenshot 2025-08-22 at 4.32.58 PM.png|400]]

estremi calcolati con 
- algoritmo midpoint su ogni lato
- tabella di estremi per ogni scan line
- aggiornando tabella quando viene prodotto un nuovo pixel per lato

errori: prodotti estremi fuori dal poligono (non appartenenti o di colore diverso)

![[Screenshot 2025-08-22 at 4.55.14 PM.png|300]]![[Screenshot 2025-08-22 at 4.55.58 PM.png|300]]

1. individuare intersezioni della scan line con tutti i lati del poligono
2. ordinare intersezioni per X crescente
3. Filing del poligono segue la regola di parità:
	- parità pari ad inizio, ogni intersezione incontrata inverte la parità
	- si disegna pixel se parità è dispari (odd-parity)
	- "entra in poligono --> diventa dispari --> esce --> diventa pari"

4 elaborazioni per il filling :

1. come si determina quale dei pixel sui due lati è interno?
	- se dentro al poligono: approssima per difetto, a sinistra
	- se fuori dal poligono: arrotonda per eccesso, a destra

2. gestire caso speciale di intersezioni per pixel con coordinate intere?
	- se intersezione è sul lato sinistro dell'intervallo (span): considerato interno (span inizia a x=12 --> 12 interno)
	- se intersezione è sul lato destro dell'intervallo (span): considerato esterno (span finisce a x=20 --> 20 esterno)

3. vertici condivisi tra diversi poligoni?
	- bisogna decidere se quel vertice viene contato per il filling o no
	- per ogni lato del poligono considera $y_{min}$ e $y_{max}$
	- nel calcolo della parità (filling) considera vertice min e non il max
	- vertice $y_{max}$ viene disegnato solo se è il vertice minimo di un lato adiacente

4. vertici definiscono un lato orizzontale?
	- non influiscono sulla parità (filling), non c'è $y_{min}$ e $y_{max}$ 
	- viene disegnato solo se parità dispari, altrimenti viene saltato

## Algoritmo Scan-Line

1. individuare intersezioni della scan line con tutti i lati del poligono
   molti lati intersecati dalla scan line intersecano anche alla scan line y+1 (edge coherence)
2. intersezioni ordinate per coordinata X crescente
3.  regola di parità per decidere quali pixel sono interni e vanno colorati

Gestione semplificata usando 2 tipi di tabelle
- **Global Edge Table** (GET) : memorizza info su tutti i lati del poligono, viene usata per aggiornare l'AET, organizzati per scan line a cui iniziano
- **Active Edge Table** (AET) : memorizza info su tutti i lati attivi per la scan line corrente, intersezioni aggiornate e ordinate per X. Durante scansione si aggiorna aggiungendo i nuovi lati dalla GET e rimuovendo quelli terminati

![[Screenshot 2025-08-22 at 9.36.13 PM.png|400]]
![[Screenshot 2025-08-22 at 9.36.40 PM.png|400]]
![[Screenshot 2025-08-22 at 9.46.57 PM.png|400]]


## Campionamento in Triangoli

Pipeline di rasterizzazione moderna converte primitive in triangoli
Triangoli:
- possono approssimare qualsiasi forma
- sempre planari con normale definita
- facile interpolare info a partire dai vertici
- si possono creare pipeline ottimizzate basate sui triangoli

Due problemi principali:
- Copertura : stabilire quali pixel dello schermo sono coperti dal triangolo
- Visibilità : determinare quale triangolo è il più vicino alla telecamera per ogni pixel

Posizione dei vertici del triangolo viene proiettata sul piano del display (camera oscura o sensore virtuale) poi si decide per ogni pixel se è dentro o fuori dal triangolo

Algoritmo classico:
```
per ogni tirangolo
	proietta vertici
	calcola bounding box contentente il triangolo
	per ogni pixel del bounding box
		valuta se è dentro usando equzioni dei lati
		se è dentro assengna il colore nel framebuffer
```

Casi Particolari:
- pixel considerato coperto dal triangolo se si trova su un top edge o su un left edge
(per evitare doppioni)

rasterizzazione: per elaborare triangolo basta disporre info triangolo + info immagine e profondità per pixel del raster

Limiti: 
- applicabile solo a primitive per cui possibile effettuare scan conversion
- gestione ombre riflessioni trasparenze non unificata
- potenziali problemi nel disegnare la primitiva (pixel potrebbe essere considerato più volte)

Vantaggi:
- con triangoli: rasterizzazione ottimizzata, facile da parallelizzare, memoria gestita in modo efficiente
- GPU moderne progettate per gestire triangoli


Visibilità = in caso di sovrapposizione, controlla buffer di profondità (z-buffer)

Copertura = in situazioni complesse può essere difficile da determinare copertura --> % di area coperta da triangoli può essere usata per illuminare il pixel di conseguenza
![[Screenshot 2025-08-23 at 3.28.58 PM.png|400]]
Copertura precisa: teoricamente bisognerebbe calcolare con precisione la copertura esatta, ma per semplicità si considera solo il centro del pixel (un insieme discreto, sample): se il centro è dentro si illumina il pixel
si verifica un insieme di punto a "campione" --> si può ottenere una buona stima se scelte intelligentemente
### Brutal Force Approach
interno del triangolo = insieme di punti interni ai 3 semipiani definiti dalle 3 linee dei lati
$$E_i(x,y) = a_ix+b_iy+c_i$$
```
Calcolo coefficienti E_1, E_2, E_3 dai vertici proiettati
per ogni pixel
	valuta funzione nel centro del pixel
	se positivo/0:
		pixel è interno al triangolo
```
### Bounding Box Approach
molti calcoli inutili per triangoli piccoli (verifica tutti i pixel il brute force)
Ottimizzazione:
- considera solo i pixel all'interno del bounding box 2D del triangolo (min e max di x e y)
![[Screenshot 2025-08-23 at 3.39.11 PM.png]]
```
Per ogni triangolo 
	Calcola la proiezione dei vertici 
	Calcola Ei 
	Calcola il bounding box (sullo schermo) 
	Per ogni pixel nel bounding box 
		Valuta le funzioni Ei Se tutte maggiori o uguali a zero 
		Framebuffer[x,y] = colore del triangolo
```
### Incremental Approach
si risparmia due moltiplicazioni e due somme per pixel, importante riduzione per triangoli grossi
```
Per ogni triangolo 
	Calcola la proiezione dei vertici 
	Calcola il bounding box (sullo schermo) 
	Per ogni scan line y nel bounding box 
		Valuta le funzioni Ei=aix0+biy+ci in (x0,y) 
		Per ogni pixel nel bounding box 
			Se tutte maggiori o uguali a zero 
				Framebuffer[x,y] = colore del triang. 
			Incrementa l’eq. della linea Ei+=ai
```
![[Pasted image 20241104175942.png]]
### Approccio Gerarchico
si verificano blocchi di pixel per escludere quelli che non intersecano il triangolo
![[Pasted image 20241104180111.png]]

## Ricostruzione e Campionamento

ogni campione usato per controllare piccolo elemento luminoso sullo schermo per fargli assumere colore/intensità
cosa viene ricostruito da questi campioni?

teorema di nyquist-shannon: segnale a banda limitata può essere perfettamente ricostruito se campionato con una frequenza pari al doppio della frequenza massima utilizzando filtro ideale

In grafica i segnali non sono a banda limitata --> non si riesce a ricostruire con filtri ideali
ciò comporta artefatti (aliasing)
- Jaggies in imamgini statiche
- Shimmering in immagini animate
- Effetto moirè in aree delle immagini ad alta frequenza

![[Screenshot 2025-08-23 at 3.48.56 PM.png|500]]

Antialiasing: aumento risoluzione (riduce problema, non lo elimina)
grazie al campionamento ci sono soluzioni più efficaci
- intensità pixel in base a copertura area

- unweighted area sampling : intensità pixel intersecato dalla linea decresce man mano che la distanza tra centro del pixel e linea cresce
- weighted area sampling : aree di dimensione uguale contribuiscono in maniera diversa in base alla distanza dal centro del pixel
---
# CH6 - Illuminazione e Texture

calcolo colore di una porzione di superficie in una scena 3D / oggetto
- modelli di illuminazione (lighting) --> riguarda pixel
- modelli di ombreggiatura (shading) --> riguarda geometria

Modelli di Illuminazione:
- Globali = considerano scambio di luce tra tutte le superfici, più realistici e complessi
- Locali = considerano solo luce che arriva direttamente dalla sorgente, semplici ma non gestiscono ombre, riflessioni e rifrazioni
## Equazione Fondamentale di Rendering

Rendering Equation unifica diversi algoritmi descrivendo la radianza osservata in un punto come somma di luce emessa e luce riflessa/scambiata tra tutte le superfici

La radianza osservata $Lo$ si ottiene sommando Luce Emessa (Le) e Luce Riflessa, che dipende dalla luce incidente (Li), dal coefficiente di riflessione e dal coseno tra direzione incidente e normale

BRDF (Bidirectional Reflectance Distribution Function) : funzione che descrive come la superficie riflette la luce in tutte le direzioni e determina l'aspetto del materiale
## Tipi di Riflessione

- **Riflessione speculare ideale** : specchio perfetto
- **Riflessione diffusa ideale** : uniforme in tutte direzioni
- **Riflessione speculare non-ideale** : distribuita nell'intorno della direzione di riflessione
- **Retro-riflessione** : verso sorgente
## Componenti di Luce

- <font color="#00b050">Luce Ambiente</font> = semplificazione, assume tutta la scena sia colpita da una luce diffusa proveniente da tutte le direzioni
  viene modellata con fattore $k_a$ e un intensità costante $I_a$ $$I = I_a k_a$$
- <font color="#00b050">Riflessione Diffusa Ideale (Lambertiana)</font> = luminosità uniforme indipendente dall'angolo di vista. Riflessione uguale in tutte le direzioni.
  Calcolata usando legge del coseno, dipende dal coefficiente del materiale, intensità della sorgente e dall'angolo di incidenza. $$I = I_p k_d\cos\theta \longrightarrow I = I_p k_d(\bar{N}\cdot \bar{L})$$Può essere attenuata rispetto alla distanza.
  
- <font color="#00b050">Riflessione Speculare</font> = su superfici lucide, la riflessione è più intensa vicino alla direzione ideale di riflessione.
  Se si sposta il POV si sposta anche l'alone (highlight)
  Nel **modello Phong** l'intensità decresce con la potenza del coseno alla n (esponente di riflessione speculare) tra la direzione ideale e quella di osservazione.
  Il **modello Blinn** usa il vettore "halfway", semplifica il Phong. 
## Colore e Attenuazione

Il colore delle superfici e della luce viene gestito separatamente per ciascuna componente RGB

L'attenuazione atmosferica simula la variazione di intensità e colore della luce con la profondità: oggetti lontani appaiono deboli e tendenti ad un colore di fondo
## Ombreggiatura

è sempre possibile calcolare l'ombreggiatura di una superficie calcolando la normale per ogni punto visibile e applicando un modello di illuminazione al punto

- Flat / Faceted / Constant Shading : un solo colore per l'intera faccia del poligono, calcolato da un unica normale
- Ombreggiatura Interpolata : si interpolano linearmente i valori di illuminazione calcolati ai vertici, generando gradazioni di colore sui poligoni

Coordinate baricentriche: possono essere usate per interpolare qualunque attributo associato ai vertici. Le 3 funzioni sono ottenute nella fase di verifica dei 3 semipiani nella rasterizzazione --> no ricalcolo

(più poligoni insieme)
Servono modelli per geometrie poligonali che sfruttino le info relative ai poligoni adiacenti per simulare una superficie smooth:
- Gouraud Shading : interpola il colore calcolato nei vertici
- Phong Shading : interpola le normali tra i vertici, calcola l'illuminazione per ogn pixel, migliore per specular highlights

Problemi: discontinuità tra poligoni, Mach band effect, distorsioni prospettiche e dipendenza dall'orientamento
## Textures

Immagine (texture map) digitalizzata o sintetizzata, composta da texel (mappa rettangolare col suo spazio di coordinate)

Coordinate di texture : definiscono mapping da coordinate della superficie a punti nello spazio della texture, spesso definite interpolando linearmente le coordinate di texture associate ai vertici dei triangoli
![[Screenshot 2025-08-23 at 6.31.47 PM.png|500]]

Campionamento Texture:
```
per ogni pixel nell'immagine rasterizzata
	interpola coordinate (u,v) sul triangolo
	campiona texture in (u,v)
	utilizza valore per colorare il pixel
```

causa proiezione, pixel nello spazio dello schermo hanno posizioni diverse nello spazio texture
![[Pasted image 20241105141052.png|500]]

- Ingrandimento (magnification) : camera vicino ad oggetto, singolo pixel da mappare su regioni piccole, basta interpolare valore nel centro del pixel, semplice

- Rimpicciolimento (minification) : singolo pixel da mappare su regioni più grandi della texture, occorre calcolare valore medio della texture sul pixel, più complesso

Aliasing --> singolo pixel sullo schermo copre più pixel, idealmente si calcola valore medio ma è costoso
Si possono calcolare i valori medi una volta e usare quelle info a run-time quando necessario
MIPmap : memorizza immagini pre-filtrate ad ogni scala

per capire quale livello della MIPmap usare: campionamento da livelli diversi anche all'interno dello stesso triangolo, calcola differenza tra coordinate di texture per campioni vicini

Arrotondamento : produrre artefatti ai passaggi di livello (da texture nitida a sfocata)

Campionamento Texture:
- calcolo (u,v) dai campioni (x,y) attraverso interpolazione baricentrica
- calcolo differenze coordinate di texture per campioni vicini, per ottenere L
- calcolo livello MIPmap a partire da L
- conversione coordinate di texture normalizzate a coordinate nello spazio della texture
- individuare texel necessari per filtraggio
### Ombre

ci sono algoritmi per determinare quali parti di superficie sono viste dalla sorgente di luce

è possibile generare ombre fittizie senza eseguire verifica di visibilità: trasforma ogni oggetto nella proiezione poligonale dalla sorgente di luce (puntiforme) sul piano
### Trasparenze

superfici che trasmettono luci:
- trasparenti
- traslucide --> trasmissione diffusa, raggi mescolati dalla superficie. Oggetti visti attraverso sono "confusi"

Trasparenza
- rifrattiva --> linee di vista ottica e geometrica cambiano, indice di rifrazione
- non rifrattiva --> modo semplice, non realistico, linea visiva non alterata
---
# CH7 - Profondità Visibilità Trasparenze

![[Pasted image 20241114133441.png]]
ultimo passaggio della pipeline di rasterizzazione: profondità e trasparenze

Problema di visibilità: stabilire quali superfici sono visibili e la direzione di proiezione eliminando le superfici non visibili

approcci:
- **Object - precision** : confronto tra oggetti per stabilire quali sono nascosti, compenetrazioni (complessità quadratica, poco pratico) 

- **Image - precision** : per ogni pixel stabilire quale oggetto è visibile, risolvendo la visibilità punto per punto (tipico delle pipeline raster)
## Profondità

Prima della proiezione 2D (quindi in spazio 3D) le coordinate includono profondità (z). Il confronto tra punti sullo stesso proiettore determina qual è più vicino all’osservatore.

La profondità (z) per ciascun frammento/pixel si ottiene interpolando le coordinate dei vertici usando le coordinate baricentriche.

Algoritmo di depth sorting ("del pittore") : ordinare poligoni in base a distanza dal POV e disegnare in ordine
non funziona sempre, orientamento e profondità variabile possono produrre risultati diversi
![[Pasted image 20241114140706.png]]
## Algoritmo Z-Buffer

soluzione pratica e diffusa per la gestione della visibilità:

- frame buffer + depth buffer (z-buffer) : memorizza profondità più vicina incontrata finora per ogni pixel
- si inizializza lo z-buffer con valori massimi (inf) e frame buffer con colore di sfondo
- per ogni frammento rasterizzato, si effettua depth test : se profondità minore (vicino) aggiorna z-buffer e anche frame buffer col colore

algoritmo gestisce le compenetrazioni e funziona perfettamente anche con antialiasing tramite sovracampionamento (ogni campione, non ogni pixel)
## Gestione Trasparenze

La trasparenza è rappresentata dal valore di alpha ($\alpha$) tra 0 e 1
Il canale alpha permette di combinare (compositing/blending) immagini una sopra l'altra
Il colore risultante dei pixel si ottiene pesando i colori dei frammenti sovrapposti secondo alpha, con formule esplicite di blending

La pre-moltiplicazione dei canali colore per alpha è una tecnica importante che evita problemi come il "fringing" (bordo nero attorno agli oggetti semitrasparenti) soprattutto nelle operazioni ripetute di compositing

senza pre-moltiplicazione: 	0.5(1,0,0)+(1-0.5)0.5(1,0,0) = (0.75,0,0) --> scuro
con pre-moltiplicazione: (0.5,0,0.5)+(1-0.5)(0.5,0,0.5) = (0.75,0,0.75) --> corretto

vantaggi:
- tratta problema nello stesso modo per tutti canali (colore e alpha)
- riduzione numero operazioni aritmetiche richieste
- corretta gestione sovrapposizioni multiple
- corretta gestione del ridimensionamento di texture con trasparenze
- rappresentazione ideale per pipeline di rasterizzazione

Per immagini trasparenti multiple: si disegna dal più lontano al più vicino (back to front) e si effettua il blending senza aggiornare il depth buffer

1. rendering di tutte le primitive opache usando verifica della visibilità con z-buffer
2. disabilita aggiornamento depth buffer, render primitive semi-trasparenti in ordine back-front
## Pipeline OpenGL

- Input: descrizione scena (triangoli, attributi, texture). 
- Trasformazione dei vertici dei triangoli nel sistema di coordinate della camera 3D
- Proiezione e trasformazione dei vertici nel sistema di coordinate normalizzate (3D)
- Esecuzione clipping 3D scartando triangoli esterni al cubo unitario, gestendo intersezioni con esso ecc..
- Divisione omogenea e trasformazione delle coordinate normalizzate dei vertici in coordinate schermo (2D)
- Pre-elaborazione dei triangoli, calcolo di parametri che servono per passaggi successivi
- Campionamento della copertura: calcolo attributi per campioni coperti dal triangolo
- Calcolo colore del campione : usando modello di illuminazione o campionando texture
- Aggiornamento Depth Buffer
- Aggiornamento Frame Buffer

### Pipeline OpenGL 2.X

![[Pasted image 20241114143457.png]]

**Vertex Transformation** : riceve attributi dei singoli vertici e ed esegue trasformazioni di posizione, calcoli di illuminazione per vertice, generazione e trasformazione di coordinate di texture

**Primitive Assembly and Rasterization** : riceve vertici trasformati + info di connettività e assembla le primitive. Effettua clipping e back face culling. La rasterizzazione crea i frammenti (fragment) uno per campione
Output: posizione fragment nel frame buffer + valori interpolati degli attributi calcolati nello stadio di trasformazione vertici (pixel positions)

**Fragment Texturing and Coloring** : riceve info interpolate dei fragment e produce info di colore e profondità combinando le info calcolate in precedenza (come i texel dalle coord. di texture)

**Raster Operations** : riceve posizione dei pixel e info di colore e profondità dei ragment.
Esegue una serie di test: Alpha, Depth, Scissor, Stencil
se test passati, info usate per aggiornare valore pixel in base alla gestione delle trasparenze (fino ad ora no accesso al frame buffer)

Fragment != Pixel --> necessarie fasi di elaborazione e magari antialiasing

### Pipeline OpenGL 4.X

Funzionalità aggiuntive:
- Tessellation --> gruppi di vertici (e dati) divisi in primitive più piccole
- Uso di Geometry Shaders --> controlla l'elaborazione delle primitive (In: 1 prim, Out: 0-N prim)
- Buffer usati per memorizzare dati prodotti dalla fase di trasformazione vertici

Schede Grafiche moderne possono programmare le funzionalità degli stadi della pipeline
- Vertex Shader : rimpiazza stadio di trasformazione dei vertici
- Fragment Shader : rende controllabile lo stadio di texture e colorizzazione dei fragment
#### Vertex Processor

Esegue vertex shader, input = info relative ai vertici: posizione, colore, vettore normale

Istruzioni per eseguire:
- trasformazioni posizione vertici
- trasformazione normali
- generazione coordinate di texture
- determina colore

vertex processor lavora con un vertice alla volta, non conosce info di connettività, quindi non può fare back face culling

non ha accesso al frame buffer, quindi i pixel

#### Fragment Processor

Esegue il fragment shader

Responsabile per:
- determinazione colori e coordinate di texture per pixel
- applicazione texture
- calcolo normali se illuminazione per pixel

Input = valori interpolati di posizione colore e normali calcolati nello stadio precedente

istruzioni nel fragment shader rimpiazzano stadio della pipeline
Opera su singoli grammenti, non ha info sui fragment adiacenti

Non può cambiare coordinate del pixel calcolate precedentemente, può accedere alle info di singoli pixel data la loro posizione ma non ha accesso al frame buffer
## Hardware Grafico

GPU moderne gestiscono scene con milioni di triangoli e calcoli su ogni pixel a risoluzioni elevate, esegue calcoli complessi nei vertex e fragment shaders, opera a framerate elevati (30-120fps)

La pipeline è implementata tramite processori specializzate e architettura multi-core sia su GPU discrete che integrate

Recentemente l'hardware include anche stadi dedicati al ray tracing
## Ray Tracing

Approccio alternativo alla rasterizzazione, basato sul ray casting:
- Per ogni pixel spara raggio che attraversa scena e si calcolano le intersezioni con superfici

Determina visibilità e colore pixel in modo ricorsivo (riflessioni, rifrazioni, ombre)

l'algoritmo z-buffer usa i vertici proiettati e verifica copertura pixel
l'algoritmo ray casting verifica le intersezioni
![[Pasted image 20241114155743.png|500]]

- determina intersezione di un raggio con qualsiasi superficie
- può essere lento e complicato
- solo intersezioni raggio-oggetto, non tra oggetti
- ottimizzazioni bounding volume per intersezioni più semplici
---
# CH8 - Curve Superfici e Geometrie Poligonali

Curve e superfici sono fondamentali in grafica per descrivere geometrie di oggetti, tracciare percorsi e animazioni, definire font e rappresentare dati grafici complessi

Geometria = studio dello spazio e delle forme, dalla misurazione empirica alla formalizzazione matematica delle superfici

Superficie = parte esterna di un oggetto. Diverse codifiche digitali:
- esplicite : rappresentata direttamente tramite coordinate
	   nuvole di punti, mesh poligonali
- implicite : insieme dei punti che soddisfano un equazione
	   superfici algebriche, CSG, insiemi di livello, frattali

## Rappresentazioni Implicite

Superficie specificata da una relazione (punti in un equazione)

PROs: 
- verificare se un punto è interno/esterno alla superficie è semplice
- descrizione compatta
- rappresentazione esatta di forme semplici

CONs: 
- Estrazione di tutti i punti sulla superficie è difficile, soprattutto per forme complesse

Tipi di rappresentazioni implicite:

- <font color="#ffff00">Constructive Solid Geometry (CSG)</font> = Creazione di forme complesse da primitive tramite unioni, intersezioni, differenze strutturate ad albero

- <font color="#ffff00">Metasuperfici / Blobby</font> = Somma di funzioni (tipo Gaussiante) per descrivere forme che si fondono gradualmente

- <font color="#ffff00">Insiemi di Livello</font> = superficie rappresentata come griglia di valori e valutazione della soglia

- <font color="#ffff00">Frattali</font> = forme auto-simili, utilizzate per fenomeni naturali, difficili da controllare in dettaglio
## Rappresentazioni Esplicite

- <font color="#ffff00">Point Could</font> = lista di coordinate (spesso normali), rappresenta facilmente qualunque geometria, semplici da visualizzare. Però sono difficili da interpolare/analizzare/gestire in modo topologicamente consistente.

- <font color="#ffff00">Polygon Mesh</font> = collezioni di vertici collegati in poligoni (triangoli o quadrilateri) e informazioni sulla connettività. 
  Facile elaborazione, adattabilità di campionamento, gestibilità in simulazioni e rendering.
  Struttura dati complessa, superfici curve approssimate attraverso poligoni.

## Rappresentazioni Parametriche di Curve

Curve Esplicite : $y=f(x)$ limitate per curve complesse, rotazioni e tangenti infinite

Curve Parametriche : $(x(t), y(t), z(t))$ con $t$ parametro. Flessibile, gestisce tangenti e giunzioni tra segmenti.

in generale curve n=3 (cubiche), perchè:
- grado 1: retta/segmento, i due coefficienti determinati dagli estremi
- grado 2: non adatta, 3 coeff, due estremi + una condizione / 3 punti
- grado +3: costi di elaborazione elevati

- grado 3: 4 coefficienti/vincoli : 2 estremi + 2 valori della derivata negli estremi

**Continuità parametrica** 
- $C^0$ se le due curve **coincidono** in un punto (di giunzione) 
- $C^1$ se i due vettori tangenti alla curva **sono uguali** (**direzione** e **modulo**) nel punto di giunzione 
- $C^2$ se la derivata seconda **è uguale** in quel punto

**Continuità geometrica** 
- $G^0$ se le due curve **coincidono** in un punto (di giunzione) 
- $G^1$ se i due vettori tangenti alla curva sono **proporzionali** nel punto di giunzione (hanno la stessa direzione, ma non necessariamente lo stesso modulo) 
	- Pendenza uguale, uno multiplo dell’altro (TV1=k·TV2, k>0) 
	- Spesso richiesto, es. nel CAD
- $G^2$ se la derivata seconda è **proporzionale** in quel punto

$C^1$ implica $G^1$ 

- **Hermite** : definite da 2 estremi e 2 vettori tangenti negli estremi. Usano vincoli diretti sui punti-tangenti. Continuità facile da regolare

- **Bezier** : Definite da 4 punti di controllo. Curve passano solo per i punti estremi, i punti intermedi regolano le direzioni tangenti. Semplici grazie ai polinomi di Bernstein, sempre contenute nel convex hull dei punti di controllo.
  Svantaggi: punto di controllo modifica tutta la curva, pesanti restrizioni sulla forma della curva per continuità G2 (C2)

- **Spline/B-Spline** : interpolano determinati punti di controllo, uniscono segmenti di curve garantendo diversi livelli di continuità (C0, C1, C2) --> un grado di continuità in più rispetto a Hermite e Bezier, controllo locale

	- **B-Spline uniformi** : nodi equispaziati, ogni segmento di curva influenza solo pochi punti di controllo. 
	  $n$ nodi e $n-1$ punti di controllo

	- **B-Spline non uniformi** : nodi non equispaziati, maggiore flessibilità nella forma e nella gestione delle interpolazioni

- **Curve Razionali NURBS** : aggiungono una quarta coordinata (peso). permettono manipolazioni geometriche e rappresentazione esatta di coniche

![[Pasted image 20241124224445.png]]

## Superfici Parametriche

- Superfici bi-cubiche : generalizzazione delle curve cubiche. Definite da griglie di punti di controllo

- Superfici di Bezier : Patch derivati dai prodotti tensoriali delle curve. Semplici da disegnare ma non sempre facili da connettere mantenendo continuità

- Superfici NURBS : Reale generalizzazione, permettono rappresentazione di superfici complesse e coniche con continuità elevata, ma difficili da manipolare

## Superfici di Suddivisione

Partire da mesh di controllo "grezza" e raffinarla ripetutamente calcolando (con opportune regole di media) nuove posizioni dei vertici

Algoritmi Catmull-Clask (quadrilateri) e Loop (triangoli)

PROs : Facili da manipolare per il modellatore, generando superfici smooth, usate in CGI professionale
CONs : Difficile valutare punti singoli sulla superficie, controllo della continuità nei vertici
## Topologia: Varietà e Mesh Poligonali

Varietà (manifold) : condizione per cui una superficie, localmente, "assomiglia" ad un piano
In termini semplificati, in geometria una superficie è una varietà se, avvicinandosi molto, è possibile disegnare su di essa una griglia piana

Condizioni per una mesh poligonale per essere varietà:
- ogni lato appartiene ad al più 2 poligoni
- per ogni vertice, i poligoni che lo contengono formano un unico anello

PROs : algoritmi e strutture dati più semplici, gestione topologica robusta e storage efficiente (analogia con bitmap a griglia regolare)

## Strutture Dati per Mesh Poligonali

- **Array / List di triangoli** : solo coordinate, massima semplicità ma difficile da gestire vicinanza/connettività
- **Elenco di adiacenze / Matrici di incidenza** : Vertici memorizzati come triplette di coordinate / triangoli come triplette di indici.
  Relazioni tra vertici-lati-facce rendono rapide alcune operazioni, ma memoria elevata
- **Struttura Half Edge** : mix tra lista ma memorizza alcune info di vicinanza tramite puntatori.
  Gestione efficiente della connettività, facilitata traversine e manipolazione della mesh (inserimento, cancellazione, split, collapse edge)

Confronto tra gli approcci:

|                                       | Array/Lista | Matrice Incidenza | Half-Edge |
| ------------------------------------- | ----------- | ----------------- | --------- |
| tempo di accesso agli elementi vicini | NO          | SI                | SI        |
| Facilità di rimozione di elementi     | NO          | NO                | SI        |
| Supporto superfici non varietà        | SI          | SI                | NO        |
## Elaborazione delle Geometrie (Geometry Processing)

Operazioni principali:

- Ricostruzione : da punti, punti+normali, immagini, si cerca di ricostruire una superficie coerente (tramite visual hull, power crust)

- Filtraggio : Rimozione di rumore, enfasi delle caratteristiche geometriche importanti
  (tramite curvature, edge detection, filtri bilaterali/spettrali)

- Sovra/Sotto-campionamento : Aumentare o ridurre il numero di elementi geometrici
	- algoritmo greedy di edge collapse e metriche di errore
	- attenzione alla degradazione della qualità con ricampionamenti ripetuti

- Ricampionamento : modificare la distriuzione dei campioni per migliorare la qualità della mesh in base a obiettivi diversi


Qualità della mesh: 
- approssimazione della forma originale, presenza di dettagli dove la curvatura è alta
- regolarità degli angoli (ideale 60 gradi in pesi triangolari)
- Condizione di Delaunay : nessun altro vertice nella circonferenza di un triangolo
- Regolarità del grado dei vertici (6 per triangoli, 4 per quadrilateri)
- Tecniche per migliorare la qualità: flip degli edge, ricentramento dei vertici, ricampionamento isotropico
---
# CH9 - Rendering Fotorealistico

Lo scopo è simulare la generazione di immagini che siano visivamente realistiche, a partire da una scena descritta da geometrie, materiali, luci e una camera virtuale.

## Rasterizzazione vs Ray Tracing

Rasterizzazione:
- Opera per primitive: per ogni primitiva si verifica la copertura sui pixel, calcolando visibilità e colore tramite z-buffer e modelli di illuminazione locali
- Modello di illuminazione locale: difficoltà nel gestire eventi globali come ombre, riflessioni, rifrazioni
- Non richiede memoria dell'intera scena per ogni pixel

Ray Tracing:
- Opera per campione/pixel: per ogni pixel della camera si traccia un raggio verso la scena per individuare l'oggetto visibile, gestendo visibilità e colore attraverso calcoli fisici.
- Modello di illuminazione globale: facile la simulazione di ombre, riflessioni, rifrazioni e fenomeni di luce indiretta
- Necessita accesso a tutta la scena per calcoli complessi

## Ray Tracing

Forward Ray Tracing : tracciamento dei raggi dalla sorgente, usato raramente perchè inefficiente
Backward Ray Tracing : tracciamento dei raggi dal punto di vista della camera verso la scena, più usato perchè simula solo ciò che viene visualizzato nell'immagine

Illuminazione Globale: il raytracing applica ricorsivamente i modelli di illuminazione locale per gestire riflessioni e rifrazioni multiple, consentendo rendering realistico

Algoritmo base:
1. per ogni pixel traccia raggio dalla camera
2. trova prima intersezione oggetto-raggio
3. calcola colore al punto di intersezione tramite modelli di illuminazione (considerando sorgenti dirette)
4. traccia raggi secondari per riflessi, rifrazioni, ombre: ricorsione nei calcoli secondo proprietà dei materiali
5. calcola contributo di ogni sorgente

Limiti: simulazioni di luce indiretta e globale approssimate, problemi di precisione numerica e immagini innaturali, complessità computazionale elevata

- **Distributed Ray Tracing** : tracciamento multiplo di raggi in direzioni distribuite per simulare effetti complessi (riflessioni sfumate, penombre, traslucenza, depth of field, motion blur, antialiasing)

- **Monte Carlo Ray Tracing / Path Tracing** : approccio che usa il campionamento stocastico e integrazione numerica per calcolare radianza, risolvendo equazione di rendering per tutti i contributi di luce

![[Pasted image 20241126124518.png]]

- **Campionamento per importanza** : ottimizza i campioni considerando il materiale e direzioni più rilevanti

- **Campionamento per importanza multiplo** : info dai materiali + sorgenti di luce: combina le due strategie per migliorare efficienza e qualità

- approccio bi-direzionale: si collega percorso backward e quello forward, aiuta a scegliere i percorsi da considerare

Radianza osservata in un punto $p$ nella direzione $\omega _o$ è data dalla somma della luce emessa e della luce riflessa (integrata in tutte le direzioni incidenti, pesata dal coefficiente di riflessione e coseno tra direzione e normale)

Ad ogni punto di intersezione, vengono tracciati nuovi raggi (riflessi, rifrazioni, ombre)
Ricorsione viene fermata in base a soglie di contriuto, profondità massima o tecniche probabilistiche
Potenziale introduzione di bias se non si considerano tutti i precorsi significativi, strategie come Metropolis Light Transport (MLT) mirano a trovare e ottimizzare i percorsi buoni
## Approcci Alternativi

Radiosity:
- studio dell'energia luminosa totale scambiata tra superfici, assunzione di ambienti chiusi e riflessione diffusa.
- suddivisione in regioni emittenti/riflettenti, calcolo dei fattori di forma e solving di sistemi di equazioni
- Scene trattate in modo indipendente dal POV, costoso in memoria, efficace per luce diffusa e ombre morbide

Photon Mapping:
- 2 fasi: tracciamento dei fotoni dalle sorgenti + memorizzazione info in una photon map e rendering finale raccogliendo i contributi dei fotoni 
- gathering dei fotoni per ogni pixel durante il rendering, simulando scattering riflessione e trasmissione
- buona flessibilità per gestire effetti complessi e la possibilità di riutilizzare i dati per diversi POV

Confronto:
- Ray/Path Tracing -->
	  adatto per riflessioni speculari, fenomeni che dipendono dal POV
	  costoso, non sempre real-tim
	  
- Radiosity -->
	  Efficiente per diffusione
	  Non ottimale per riflessione speculare, più adatto per ambienti statici

- Photon Mapping -->
	  Buon compromesso, sfrutta vantaggi dei due approcci, permette riutilizzo dei dati

## Blender

Blender offre 3 motori di rendering integrati:
- **Workbench**: rasterizzazione, ottimizzato per visualizzazione rapida in fase di modellazione/animazione
- **Eevee**: basato su OpenGL, codice simile ad Unreal Engine, Rasterizzazione avanzata per interattività e tempo reale
- **Cycles**: basato sul path tracing, per rendering finale fotorealistico

## Ottimizzazioni e Strutture Dati per Intersezioni

Intersezione base raggio-triangolo: complessità O(N), poco efficiente per scene grandi

Bounding Volume Hierarchy (BVH): gerarchia che usa bounding boxes per suddividere le primitive, riducendo il numero di test necessari

Binary Space Partitioning (BSP): suddivide lo spazio in piani arbitrari, più adatta a oggetti solidi

K-D Tree: suddivisione spaziale usando piani allineati agli assi, efficienza nell'intersezione

Griglie uniformi e octree: suddivisione dello spazio in celle di uguale dimensione (griglie) o octani (octree) adatte a scene con distribuzione regolare

Scaletta della struttura: dipende dalla scena, distribuzione primitive e dagli obiettivi di efficienza e scalabilità

---
# CH10 - Hardware

## Schermi e Tecnologie di Visualizzazione

CRT (Cathode Ray Tube)
![[Pasted image 20241210121633.png|500]]
- Fascio di elettroni accelera verso uno schermo rivestito di fosfori eccitati e capaci di emettere luce per fluorescenza o fosforescenza
- tempo di decadimento della luce --> immagine fa rinfrescata spesso per evitare sfarfallio
- frequenza di refresh (Hz), banda video (MHz) determinano qualità e nitidezza
- fosfori RGB, tecniche come 
	  shadow-mask = sottile lamina metallica allineata in modo tale che ciascun fascio di elettroni possa colpire un solo tipo di fosforo 
	  aperture grille = griglia di sottili fili metallici tra i cannoni e lo strato di fosfori. Fasci di elettroni allineati solo dalla griglia
	  slot mask = Incrocio tra le due tecniche precedenti. Griglia di linee verticali suddivise in celle ellittiche
- limiti: dot pitch (distanza fosfori), risoluzione limitata dal diametro del fascio, grandezza e consumo energetico

LCD (Liquid Crystal Display)
![[Pasted image 20241210123615.png]]
- cellule di cristalli liquidi ruotano la polarizzazione della luce tra due filtri, ondulando la trasparenza con campi elettrici
- Tipi trasmissivi (retroilluminazione) o riflessivi (luce ambientale)
- filtri posizionati per segmento RGB
- matrice attiva: transistor per pixel/sottopixel, memoria dello stato
- basso peso, basso consumo, produzione complessa, rischio bad pixel

LED : retroilluminazione a LED, migliore contrasto/luminosità, minor consumo
Plasma : capsule di gas eccitate, adatti a grandi dimensioni, costosi e soggetti a burn-in
OLED : film organici, autofosforescenti, sottili, pieghevoli, bassi consumi, aia gamma angolare
DLP : micro-specchi per proiezione, alta risoluzione
ePaper : basati su elettroforesi, basso consumo, lento refresh
## Sistemi di Visualizzazione Raster

2 macro categorie:
- presenza o meno di hardware specializzato (GPU) per rendering
- relazione tra pixmap e spazio di indirizzamento della memoria general purpose

Schema semplice:
![[Pasted image 20241210142732.png]]
No processore grafico, pixmap nella memoria general purpose

sistema grafico può visualizzare max 16M colori, quindi max 24 piani di colore (True Color)
in sistemi avanzati: 32 o 40 piani
piani colore aggiuntivi usati per immagazzinare info come overlay di testo, doppio buffer ecc.. per non sovrascrivere memoria dedicata all'immagine vera e propria

True Color = ci si può spostare da un colore all'altro con continuità (no salti)
LUT (Look Up Table) (Color Palette) = tante celle quanti i possibili valori dei pixel, valore usato come indice

## Evoluzione Pipeline Grafica

Schema con Processore Grafico e Memoria Dedicata:
![[Pasted image 20241210143654.png]]

Svantaggi: gestione onerosa per OS se usato come GPU considerata come periferico, scan conversion complessa, trasferimenti blocchi raster tra memoria e frame buffer attraverso il bus di sistema è troppo lento

Schema alternativo:
![[Pasted image 20241210144459.png]]

Vertex Shader : elaborazione proprietà dei vertici = posizione colore normali
Fragment Shader : calcolo del colore a pixel (shading)

Parallelismo: GPU moderne gestiscono centinaia di migliaia di thread dedicati al throughput piuttosto che alla latenza come nelle CPU
## GPU Moderne

Esecuzione parallela e indipendente di unità di lavoro, uso efficiente della memoria condivisa
Scheduling avanzato, cache condivise, unità SIMD, hardware per ray tracing
Rasterizzazione per visibilità primaria, ray tracing per effetti avanzati
## Bus e Trasferimento Dati

Velocità globale di elaborazione dipende soprattutto da velocità con cui CPU riesce a colloquiare con interfaccia grafica, grafica 3D elabora una quantità di dati molto più alta di quelle in 2D (luci, animazioni, trasformazioni, texture...)

Bus ISA: parallelo, bassa frequenza, 8MHz, 16bit larghezza
Bus PCI: parallelo, aumentata frequenza (33-66MHz) 32-64bit 

Bus AGP: dedicato alla grafica, larghezza oltre i 2GBps, accesso diretto a memoria di sistema per texture evitando pre-fetch nel frame buffer del sistema video
Incremento rispetto al bus PCI (per le periferiche) è significativo

Bus PCI Express (PCI-E) è seriale, ha sostituito il bus AGP per schede grafiche, costruito da una serie di canali che possono essere aggregati per aumentare banda passante disponibile
## Memoria Video

Incremento prestazioni nei sistemi di visualizzazione (raddoppio velocità ogni 6 mesi) richiede crescita proporzionale degli altri componenti del sistema, come la memoria video

SDR (Single Data Rate), banda limitata

DDR, DDR2, DDR3... Double data Rate, dati trasmessi sia sul fronte di salita che di discesa, bus con clock multiplo rispetto alle celle. Sensibile aumento di banda, di frame rate con risoluzioni alte
## Dispositivi Periferici

Sistemi di puntamento: mouse, joystick, tavolette grafiche

Output (hardcopy): stampanti
- ad impatto = aghi su nastro inchiostrato, basso costo bassa qualità
- termiche = calore per marcare carta, inchiostro a cera
- laser = tamburo fotosensibile, laser, toner polverizzato e fissato, alta qualità e velocità
- inkjet = piezoelettrico, bubble jet, molte migliaia di punti per pollice, qualità elevata, costi minori
- plotter = alta precisione per CAD, grande formato, diverse tecnol, carta e stampa.

Input: 
- scanner = lettura linea per linea tramite CCD/CMOS/CIS, risoluzione e densità ottica determinano qualità
- fotocamere digitali


