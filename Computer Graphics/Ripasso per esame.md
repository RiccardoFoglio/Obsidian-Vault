
# CH1 - Intro

30% cervello dedicato ad elaborazione di info visive --> occhi = canale di comunicazione con banda maggiore per cervello

Informatica Grafica = tecniche e metodi per rappresentare sotto forma di stimuli sensoriali + elaborare e manipolare info digitali

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
- efficienza = possibile codificare in maniera diversa in base a quanto siano rilevanti dal unto di vista percettivo (compressione)
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
- gestione obre riflessioni trasparenze non unificata
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

- Ingrandimento (magnification) : camera vicino ad oggetto, singolo pixel da mappare su regioni piccole, vasta interpolare valore nel centro del pixel, semplice

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













---
# CH9 - Rendering Fotorealistico

---
# CH10 - Hardware

### Schermi CRT

Grafica interattiva, necessità di dispositivi che consentano di cambiare/"refreshare velocemente le immagini visualizzate
CRT (Cathode Ray Tube) monocromatici
![[Pasted image 20241210121633.png]]

Quando il fascio colpisce il rivestimento, gli elettroni del fosforo eccitati si muovono verso livelli di energia superiore

Nel ritornare ai livelli precedenti, rilasciano energia sotto forma di intensità luminosa (e calore)
- **Fluorescenza**: luce emessa in modo sostanzialmente istantaneo quando il fosforo viene colpito dal fascio 
- **Fosforescenza**: luce emessa quando l’eccitazione cessa

Rilevanza del fenomeno detto di **persistenza** : Tempo che trascorre da quando l’eccitazione cessa a quando l’intensità luminosa dovuta alla fosforescenza è decaduta esponenzialmente al 10% del valore iniziale

L’immagine deve essere rinfrescata diverse volte al secondo per evitare fenomeni di sfarfallio (l’occhio non riesce ad integrare impulsi di luce)
Frequenza di fusione critica, decresce al crescere della persistenza
Frequenza di refresh (refresh rate) o di quadro/di scansione verticale
Frequenza di scansione orizzontale

Banda del segnale video è legata alla velocità con cui il cannone può essere acceso e spento
Per ottenere una risoluzione orizzontale di $n$ pixel per linea deve essere possibile accenderlo almeno $n/2$ volte e spegnerlo almeno $n/2$ volte (alternanza on-off)

CRT a colori = Superficie interna dello schermo coperta con gruppi di fosfori rossi, verdi e blu ravvicinati
Tecnica **shadow-mask**: sottile lamina metallica allineata in modo tale che ciascun fascio di elettroni possa colpire un solo tipo di fosforo: colori diversi ottenibili eccitando in modo diverso (selettivamente) ciascun fosforo
![[Pasted image 20241210122421.png]]

Soluzioni alternative:
- Schema "aperture grille" : Griglia di sottili fili metallici tra i cannoni e lo strato di fosfori. Fasci di elettroni allineati solo dalla griglia
- Schema "slot mask" : Incrocio tra le due tecniche precedenti. Griglia di linee verticali suddivise in celle ellittiche (migliorano la saturazione, il contrasto e la messa a fuoco)
![[Pasted image 20241210122606.png]] 

Limite alla risoluzione dei CRT a colori
- Distanza tra i centri (dot pitch, o stripe pitch) 
- Considerato che è difficile garantire che il fascio di elettroni sia esattamente allineato con il foro, il suo diametro (intensità 50% ) è in genere di dimensione pari a 7/4 volte il dot pitch
![[Pasted image 20241210123403.png]]

## Schermi a cristalli liquidi LCD

![[Pasted image 20241210123615.png]]
Tra un solido cristallino ed un liquido (dis-ordine) 
- Lunghe molecole (nematiche), generalmente organizzate in una struttura a spirale tale per cui la polarizzazione della luce (polarizzata) che le attraversa viene ruotata di 90°
- In un campo elettrico, allineate nella stessa direzione, senza alcun effetto polarizzante

In assenza di campo elettrico:
- Luce incidente polarizzata verticalmente
- Polarizzazione della luce trasmessa ruotata di 90°
- Attraversato il polarizzatore orizzontale
- Luce riflessa (riattraversati i due polarizzatori e lo strato di cristalli liquidi)

Campo elettrico: 
- Applicando una tensione in un determinato punto della matrice (griglia orizzontale +, griglia verticale -)
- Polarizzazione della luce trasmessa non ruotata (resta verticale)
- Polarizzatore posteriore non attraversato, luce assorbita
- Osservatore: punto nero sullo schermo

Non c’è emissione di luce (interruttore)
Necessità di una sorgente di luce esterna:
- Laptop: retro-illuminazione, schermi trasmissivi
- Console, esempio Game Boy: schermi riflettivi, luce esterna riflessa da uno strato riflettente posteriore
Colori : Filtri a frequenze diverse posizionati davanti allo schermo (sotto-pixel)

I cristalli liquidi devono essere costantemente rinfrescati, o ritornano al loro stato cristallino
- Schermi LCD passivi: ciclo di refresh veloce, seguito da una lento ritorno allo stato precedente
- Schermi LCD a matrice attiva: un transistor per ogni punto sulla griglia
	- Utilizzato per cambiare velocemente lo stato dei cristalli 
	- Memoria dello stato, mantenuta fino al refresh successivo

Vantaggi : Basso costo, peso, dimensioni e consumi ridotti
Svantaggi : produzione complessa

## Schermi LED

Retro-illuminazione a LED anziché con tubi fluorescenti. Pannelli più sottili, minori consumi, migliore dissipazione termica, schermi più luminosi, miglior controllo dei livelli di contrasto
Tecnologie
- LED sul bordo, speciale pannello per diffondere in modo uniforme
- Matrice di LED posizionati dietro lo schermo
	- Luminosità controllata in maniera non selettiva 
	- Luminosità modulata per singoli LED o gruppi di LED

## Schermi al Plasma
Piccole capsule riempite con gas (neon, ecc.). Per singoli pixel/sotto-pixel, eccitate mediante campi elettrici emettono luce UV in grado di eccitare i fosfori che, terminata l’eccitazione, rilasciano energia a diverse lunghezze d’onda 

Vantaggi
- Adatti per schermi di grandi dimensioni 
- Ampi angoli di visione 

Svantaggi 
- Molto costosi 
- Pixel di dimensione elevata (1 mm contro frazioni di mm)
- Soggetti ad esaurimento
- Meno luminosi dei CRT (più potenza)
## Schermi OLED

Organic LED
- Sottili film organici tra due conduttori (elettro-fosforesc.)
- Meno potenza degli LCD (non c’è retro-illuminazione)
- Più luminosi 
- Minor tempo di risposta 
- Intervallo di temperature di funzionamento più ampio
- Angolo di vista fino a 160° o più
- Pieghevoli, arrotolabili

## Digital Light Processing (DLP)

Generalmente utilizzati per sistemi di proiezione. Alta risoluzione e luminosità
![[Pasted image 20241210142406.png]]

## ePaper

I più comuni basati su elettroforesi (es. eInk ) 
- Micro -capsule riempite con delle particelle bianche cariche elettricamente sospese in una soluzione oleosa con colorante nero
- Applicando una tensione, le particelle si spostano (e rimangono in quella posizione anche quando il campo elettrico viene rimosso)
- Anche con particelle nere, e filtri per colori
- Bassi consumi, refresh lento
![[Pasted image 20241210142447.png]]

## Sistemi di Visualizzazione Raster

Due macro-categorie, in base alla
- Presenza o meno di un hardware specializzato (Graphics Processing Unit (GPU) per il rendering)
- Relazione tra la pixmap e lo spazio di indirizzamento della memoria general purpose

Scheda/interfaccia grafica, circuiteria che normalmente comprende
- Controllore/(co-)processore grafico 
- Memoria video e vari dispositivi accessori (quarzi, DAC, ecc.)
- Controllore dello schermo ed almeno un connettore per il collegamento dello schermo 
- Loro connettori al resto dell’architettura, attraverso un bus più o meno dedicato
## Schema semplice
Nessun processore grafico, Pixmap nella memoria general purpose
![[Pasted image 20241210142732.png]]

Organizzazione per piani di colore
- sistema grafico può visualizzare max 16 milioni di colori, quindi un massimo di 24 piani di colore (True Color)
- in sistemi grafici avanzati è possibile gestire 32 o 40 piani colore
- I piani colore aggiuntivi vengono sfruttati dal sistema per immagazzinare informazioni come gli overlay di testo, doppio buffer o altri effetti, per non dover sovrascrivere la parte di memoria dedicata all’immagine vera e propria, avvantaggiando la velocità di elaborazione e qualità della grafica prodotta

True color = Ci si può spostare tra un colore e quello successivo con continuità (non si percepiscono salti di colore)
Look-up table (color palette) = Tante celle quanti sono i possibili valori dei pixel, Valore utilizzato come indice (non direttamente per controllare il pennello)
![[Pasted image 20241210143503.png]]

## Schema con Processore Grafico e Memoria Dedicata
![[Pasted image 20241210143654.png]]

Svantaggi 
- Se il processore grafico è acceduto dalla CPU come un dispositivo periferico la sua gestione può essere particolarmente onerosa per il sistema operativo
- Es., scan conversion piuttosto complessa 
	- Quattro possibili coppie sorgente-destinazione da gestire in maniera diversa (memoria di sistema-memoria di sistema può non esistere) 
	- Asimmetrie difficili da gestire dal punto di vista del programmatore, limitano la flessibilità
- Trasferimenti di blocchi raster tra la memoria di sistema ed il frame buffer attraverso il bus di sistema, potenzialmente troppo lenti animazioni, trascinamento, ecc. 
	- Parzialmente risolvibile aumentando la memoria del processore grafico (per memorizzare pixmap non mostrate)

Schema alternativo dove parte della memoria di sistema per il frame buffer 
- Accessibile dal processore grafico 
- Singolo spazio di indirizzamento

![[Pasted image 20241210144459.png]]

## GPU moderne

Non solo pipelining, Latenza (CPU) vs throughput (GPU)
Elevato numero di unità di lavoro (pixel) indipendente : Sincronizzazione non necessaria, parallelizzazione 
Località dei dati 
- Informazioni interne (vertici, attributi, ecc.), informazioni esterne (texture e frame buffer) con elevata coerenza 
- Accesso “intelligente” alla memoria, e memoria condivisa tra elementi di calcolo
Scheduling dedicato: Limitate situazioni di ridotta attività
Hardware dedicato per operazioni comuni costose

### Bus
In qualsiasi elaborazione informatica vengono gestite delle informazioni
La velocità globale di elaborazione di queste informazioni dipende da diversi fattori 
Nel caso specifico dell’elaborazione grafica, la velocità con la quale la CPU riesce a colloquiare con l’interfaccia grafica o, più generalmente con una qualunque interfaccia, è un parametro di notevole importanza
![[Pasted image 20241210155930.png]]

Le applicazioni che utilizzano grafica 3D devono elaborare una quantità di dati molto più alta che non quelle che lavorano in 2D 
- Animazioni o applicazioni grafiche/sistemi di simulazione interattivi, fanno crescere significativamente il carico computazionale legato alla simulazione in tempo reale dell’ambiente tridimensionale 
- Il calcolo delle luci, le trasformazioni geometriche, l’applicazione di texture nonché l’intelligenza/la logica presente nelle applicazioni grafiche/simulazioni interattive, necessitano di sotto-sistemi grafici sempre più potenti

### Bus AGP

AGP (Accelerated Graphics Port)
Benefici offerti dall’architettura AGP riguardano 
- quantità di dati trasferibili nell’unità di tempo
- strada che questi percorrono all’interno del sistema
L’AGP ottimizza uno dei “colli di bottiglia” dell’architettura dei calcolatori
Permette di utilizzare direttamente la memoria di sistema per immagazzinare le texture con un meccanismo di allocazione dinamica da parte del sistema operativo nei confronti del sistema grafico 
Le texture possono essere lette direttamente dalla memoria di sistema senza alcuna operazione di pre-fetch nel frame buffer del sistema video

![[Pasted image 20241210170500.png]]

In generale l’incremento di prestazioni rispetto al bus PCI è significativo
- Il sistema dispone di un ampio canale dedicato in grado di trasferire una grande quantità di dati tra memoria di sistema e interfaccia grafica senza “incontrare” traffico di altri sottosistemi
- I benefici di tale architettura sono più evidenti man mano che si lavora con risoluzioni più alte, cioè con un maggior numero di dati
## Memoria video

Incremento di prestazioni nei sistemi di visualizzazione: raddoppio della velocità dei processori grafici ogni sei mesi 
Ciò richiede che anche gli altri componenti del sistema crescano proporzionalmente al fine di evitare colli di bottiglia 
Potenza offerta dai processori grafici e dalle capacità di trasferimento del bus 
- Induce l’utente, e gli applicativi, ad utilizzare ambienti 3D sempre più dettagliati, con risoluzioni sempre più elevate e con un alto frame rate per rendere le scene fluide e più realistiche
![[Pasted image 20241210171906.png]]

Queste caratteristiche producono un aumento del consumo della capacità di banda della memoria video
Tutto ciò ha portato la tecnologia delle memorie SDR (Single Data Rate) al limite della loro capacità 

Esempio, banda richiesta da un’applicazione 3D con 
- Filtraggio delle texture bi-lineare 
- Due texture per pixel
- Frequenza di refresh pari a 72 Hz
- Complessità di profondità pari a due

All’aumentare della risoluzione di lavoro, la banda offerta dalle memorie SDR risulta insufficiente

Memorie DDR (Double Data Rate)
- Dati trasmessi sia sul fronte di salita che di discesa del clock 
- Stessa latenza
- Il doppio dei dati trasferiti rispetto alle SDR, senza aumentare la frequenza del bus di sistema

È possibile valutare l’incremento di prestazioni grafiche indotto dall’utilizzo di memorie DDR
- Intorno al 40% nel caso in cui la banda passante della memoria video rappresenti un collo di bottiglia

Evoluzioni
- DDR2 bus con un clock raddoppiato rispetto al clock della singola cella di memoria (es. 200 MHz), possibilità di trasferire il doppio dei dati rispetto a DDR(1) senza modificare la velocità delle celle di memoria (stessa latenza, 3.2 GB/s), oppure stessa banda ma clock dimezzato (risparmio energetico)
- DDR3, bus con clock quadruplicato rispetto a DDR(1)
- DDR4, DDR5, XDR

## Dispositivi Periferici

- Sistemi di puntamento : mouse, trackball, joystick 
- Periferiche di output (hardcopy) : stampanti ad impatto, ad inchiostro, laser, plotter
- Periferiche di acquisizione : scanner, fotocamere, videocamere digitali

Mouse : Sistema di puntamento più diffuso; Di uso comune per via della diffusione delle interfacce grafiche Windows, Macintosh, ecc. 
Due principali tecnologie 
- **Meccanico** --> Utilizza il movimento di una sfera per trasmettere gli spostamenti ai dispositivi meccanici
- **Opto-elettronico** --> Necessitava di un tappetino particolare riflettente e grigliato (linee orizzontali blu, linee verticali nere)  Ciò permetteva ai rilevatori ottici di determinare lo spostamento
Tipologie di collegamento: seriale, bus mouse, porta mouse (tipo PS/2), porta USB, radiofrequenza Non fornisce posizioni assolute 

Evoluzione
- Tramite l’uso di un DSP (Digital Signal Processor) effettua molte volte al secondo, mediante un sensore ottico, la scansione della trama del tappetino (o di qualunque altro oggetto su cui è appoggiato)
- Determina il movimento del dispositivo calcolando la correlazione tra le varie scansioni

Periferiche di output (hardcopy)
Qualità raggiungibile dipende da numero di punti per pollice che possono essere creati 
- Può essere diverso in orizzontale e verticale 
- Inverso dello spazio inter-punto 
- Esempio, su $x$, inverso della distanza tra i punti $(x,y)$ e $(x+1,y)$
Dimensione del singolo punto creato (dot o spot size)
Dot size più grande dello spazio-interpunto
Compromesso, linee smussate oppure dettagli

![[Pasted image 20241210180410.png]]

Risoluzione : Numero di linee per pollice distinguibili che un dispositivo può (ri)creare 
La più piccola distanza a cui linee bianche e nere adiacenti possono essere discriminate da un osservatore

Colori : Molti dispositivi hardcopy sono in grado di generare solo un numero limitato di colori per un determinato punto 
Incrementabile mediante sintesi sottrattiva o additiva

![[Pasted image 20241210180938.png]]

Nell’ambito delle tecniche di stampa non tipografica si possono distinguere varie tecnologie 
- Ad impatto : punzonatura della carta tramite uno o più nastri inchiostrati
- Termiche (trasferimento termico, sublimazione) : Utilizzano una carta speciale in grado di annerirsi se scaldata
- Sublimazione : Inchiostro scaldato, vaporizzato e sublimato sulla carta. 256 sfumature di ciano, di magenta e di giallo
- Laser : Laser diretto su di un cilindro/tamburo rotante rivestito di materiale foto-sensibile (selenio, conduttore solo quando illuminato) carico positivamente. Le aree raggiunte dal laser perdono la propria carica, che rimane solo dove la copia deve essere nera
- Ad inchiostro 
In bianco e nero ed a colori

**Scanner** : Periferica in grado di acquisire immagini su supporto opaco (fogli stampati) e trasparente (diapositive) 
Una luce bianca, prodotta da una lampada, viene diretta sull’originale ¨ La luce riflessa viene raccolta da un sistema di specchi e diretta su di un sensore ottico (CCD, CMOS), e quindi trasformata in un segnale digitale (fotoni-cariche elettr.) ¨ Dopo aver letto una linea si fa avanzare il meccanismo e si legge la linea successiva
![[Pasted image 20241210181807.png]]

Fotocamere digitali 
Sebbene la fotografia chimica rimanga ancora in alcuni casi il punto di riferimento come risoluzione e resa dei colori, le fotocamere digitali possono orami considerarsi di utilizzo comune per un infinito numero di applicazioni

Vantaggi del digitale 
- Limitati tempi e costi di sviluppo 
- Sensibilità uniforme con luce diurna e artificiale 
- Elevata portabilità grazie alle ridotte dimensioni 
- Programmazione avanzata del dispositivo
Svantaggi del digitale 
- Risoluzione generalmente inferiore (100-200 Mpixel) 
- Gamma tonale inferiore

