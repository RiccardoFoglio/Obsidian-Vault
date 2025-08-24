Pipeline di rasterizzazione: a partire da input applicare una serie di trasformazioni di vario genere per arrivare a produrre l'output atteso (matrice di pixel)
Ultimo passo: scrittura dei campioni nel frame buffer, gestendo profondità e trasparenze
![[Pasted image 20241114133441.png]]

Problema di visibilità: dato un insieme di oggetti a di parametri di vista, determina:
- quali oggetti son visibili
- direzione di proiezione
al fine di disegnare solo quelli visibili
Processo = Determinazione / Eliminazione delle superfici visibili/non visibili

Determinare superfici visibili: implementazione complicata
- **Object-precision** : confrontare ogni oggetto nella scena con sé stesso e con ciascuno degli altri oggetti, determinando quali sono le parti dell’oggetto la cui vista non è ostruita
- **Image-precision** : determinare quali degli n oggetti è visibile per ciascun pixel dell’immagine stabilendo quale sia l’oggetto più vicino all’osservatore intercettato dal proiettore passante per quel pixel

# Profondità

La determinazione delle superfici visibili deve essere effettuata in uno spazio 3D, prima che la proiezione rimuova le informazioni di profondità necessarie per confrontare le distanze

Dati punti, si trovano sullo stesso proiettore? In caso affermativo, z1 e z2 dovranno essere confrontati per stabilire quale dei due sia più vicino all’osservatore.

Come calcolare il valore di profondità z per un particolare punto di campionamento (x, y) ?
- Interpolando i valori di profondità usando le coordinate baricentriche

# Visibilità

Problema di visibilità visto come un problema di ordinamento, approccio object-precision.
Algoritmo detto “del pittore”, o di depth sorting
- Si ordinano i poligoni in base alla loro distanza dal punto vista, poi li si disegna in ordine dal più lontano al più vicino
- Semplice
- non funziona sempre (un poligono può avere più di un valore di profondità, possono esservi compenetrazioni, ecc.), 
- può essere complesso da gestire dal punto di vista computazionale (calcolo intersezioni)
![[Pasted image 20241114140706.png]]

Problema di visibilità in termini image-precision
- Campionamento: per ciascun campione (x,y ), quale triangolo/quali triangoli è/sono visibile/i
![[Pasted image 20241114140715.png]]

## Algoritmo Z-Buffer

buffer di profondità, o depth-buffer (z-buffer), Z
Memorizza la profondità z del triangolo più vicino incontrato sino a quel momento
![[Pasted image 20241114140818.png]]

Lo z-buffer è inizializz. al valore più lontano possibile (valore di z sul piano di clipping post.)
Il frame buffer è inizializz. con il colore di sfondo
rasterizzazione dei triangoli
- ordine arbitrario
- per ogni campione (x, y), si effettua il cosiddetto depth test
	- Se il punto (x, y) è più vicino all’osservatore rispetto al punto il cui colore e valore di z sono già in F e Z, allora il suo colore e valore di z sono sostituiti in F e Z (altrimenti nulla)

```C
bool pass_depth_test(z1, z2) { // due valori di profondità 
	return z1 > z2; // vero se il primo (z1) è più vicino 
} 
draw_sample(x, y, z, c) { // nuova profondità (z) e colore (c) 
	if( pass_depth_test( z, zbuffer[x][y] )) { // il triangolo è l'oggetto più
	// vicino visto fino a quel momento per quel campione 
	zbuffer[x][y] = z; // aggiorna il buffer di profondità 
	color[x][y] = c; // aggiorna il buffer di colore 
	} 
	// altrimenti, non aggiornare 
}
```
![[Pasted image 20241114141046.png]]

Le profondità relative dei triangoli possono essere diverse nei vari punti di campionamento
	Risultato corretto se il depth test viene effettuato in ogni punto di campionamento
![[Pasted image 20241114141124.png]]

Gestione dell’aliasing (mediante sovracampionam.)
	Corretto se il depth test è effettuato per (sovra) campione
![[Pasted image 20241114141245.png]]

Visibilità con z-buffer:
- Memorizzazione di un valore di profondità per (sovra) campione (non uno per pixel) 
- Spazio di memorizzazione per campione costante 
	- Indipendente dal numero di primitive sovrapposte
- Tempo necessario per la verifica costante 
	- Lettura e scrittura/aggiornamento del buffer di profondità e colore solo in caso di depth test superato (uno per campione) 
	- Altrimenti, solo lettura
- Algoritmo non esclusivamente per triangoli 
	- È sufficiente poter valutare la profondità della superficie in esame per ogni (sovra) campione (nessun calcolo di intersezioni)
### Opacità

L'opacità di un oggetto viene tipicamente espressa facendo ricorso ad un valore alpha (α) tra tra 0 e 1
![[Pasted image 20241114141503.png]]

Il canale alpha è usato anche per sovrapporre immagini una sull'altra. Operazione di compositing / blending non è commutativa

Con pre-moltiplicazione, il compositing dell’alpha è gestito come per il colore (diversamente da quanto avviene in assenza di pre-moltiplicazione)
Senza pre-moltiplicazione, non si ottiene il risultato atteso dalla sovrapposizione.

Esempio: facendo il compositing/blending di due oggetti rossi brillanti (1,0,0) ciascuno con α = 0.5
- Senza pre-moltiplicazione
	- 0.5(1,0,0)+(1-0.5)0.5(1,0,0) = (0.75,0,0)
	- troppo scuro
- con pre-moltiplicazione
	- (0.5,0,0.5)+(1-0.5)(0.5,0,0.5) = (0.75,0,0.75)
	- diviso per $\alpha$ da (1,0,0) che è corretto

Vantaggi della pre-moltiplicazione:
- Possibilità di trattare il problema nello stesso modo per tutti i canali (colore ed alpha)
- Riduzione del numero di operazioni aritmetiche richieste
- Corretta gestione di sovrapposizioni multiple
- Corretta gestione del ridimensionamento di texture con trasparenze
- Rappresentazione ideale per una pipeline di rasterizzazione (analogia con coordinate omogenee)

Combinazioni di primitive opache e trasparenti
- rendering di tutte le primitive opache (in un qualsivoglia ordine) utilizzando la verifica della visibilità con lo z-buffer
	- Se il depth test è superato, il colore del triangolo viene scritto nel frame buffer per quel campione
- si disabilita l’aggiornamento del depth buffer, e si renderizzano le primitive semi-trasparenti in ordine back-to-front
	- - Se il depth test è superato, si effettua il compositing/ blending della primitiva con il colore nel frame buffer per quel campione

## Pipeline di Rasterizzazione, End-to-End

Input:
- Descrizione della scena (triangoli, attributi, texture)
- Trasformazione dal sistema di coordinate degli oggetti a quello della camera $T ∈ ℝ^{4×4}$ 
- Trasformazione di proiezione, es. prospettica $P ∈ ℝ^{4×4}$
- Dimensione dell’immagine di output (W,H)
![[Pasted image 20241114143118.png]]
Trasformazione dei vertici dei triangoli nel sistema di coordinate della camera (3D)
![[Pasted image 20241114143019.png]]
Applicazione della proiezione e trasformazione dei vertici dei triangoli nel sistema di coordinate normalizzate (3D)
![[Pasted image 20241114143028.png]]
Esecuzione del clipping (in 3D)
- Scartando i triangoli esterni al cubo unitario (culling)
- Gestendo quelli che lo intersecano (es. generando nuovi triangoli)
- Altre operazioni possibili, es. back-face culling
![[Pasted image 20241114143104.png]]
Applicazione della divisione omogenea, e trasformazione delle coordinate normalizzate dei vertici in coordinate dello schermo (2D)
![[Pasted image 20241114143133.png]]
Pre-elaborazione dei triangoli
- Prima di rasterizzare il triangolo, si possono calcolare su di esso informazioni utili per i passi successivi
- Equazioni dei lati, equazioni di interpolazione degli attributi ai vertici ecc..
![[Pasted image 20241114143254.png]]
Campionamento della copertura
- Calcolo degli attributi per tutti i campioni coperti dal triangolo
![[Pasted image 20241114143314.png]]
Calcolo del colore del campione, utilizzando un modello di illuminazione (materiale) o campionando una texture
![[Pasted image 20241114143331.png]]
Aggiornamento del depth buffer (se attivo) verificando il superamento del depth test
![[Pasted image 20241114143353.png]]
Aggiornamento del frame buffer (se il depth test è superato), eventualmente considerando anche le trasparenze
![[Pasted image 20241114143421.png]]
# Pipeline OpenGL (2.x)

![[Pasted image 20241114143457.png]]

## Trasformazione dei vertici
Vertice inteso come un insieme di attributi che ne definiscono la posizione nello spazio (3D), il colore, il vettore normale, le coordinate di texture, ecc. 
Questo stadio riceve in input gli attributi di singoli vertici ed esegue operazioni che riguardano
- trasformazioni di posizione 
- Calcoli di illuminazione per vertice 
- Generazione e trasformazione di coordinate di texture
## Ass. primitive e rasterizzazione
Questo stadio riceve in ingresso i vertici trasformati e le informazioni di connettività (che dicono alla pipeline come i vertici contribuiscano a formare una primitiva) e si occupa di assemblare le primitive

E’ responsabile anche 
- Di effettuare il clipping rispetto al cubo unitario 
- Di implementare il back face culling

il processo di rasterizzazione crea i frammenti (fragment), uno per campione
Un fragment è da intendersi come quell’informazione che verrà utilizzata per aggiornare un pixel in una specifica posizione del frame buffer. Contiene non solo informazioni di colore, ma anche coordinate di texture, vettore normale, ecc., tutte informazioni necessarie per calcolare il colore del pixel

L’output di questo stadio quindi è rappresentato dalla posizione dei fragment nel frame buffer e dai valori interpolati, per ciascun fragment, degli attributi calcolati nello stadio di trasformazione dei vertici
## Textur./Colorazione dei fragment

Questo stadio riceve in input le informazioni interpolate dei fragment 
	Nello stadio precedente è stato calcolato un colore per interpolazione 
	In questo può essere combinato, ad esempio, con un texel (sono infatti state già interpolate anche le coordinate di texture) 

Il tipico risultato di questo stadio è, per ciascun fragment, un’informazione di colore e di profondità
## Operazioni sul raster

Questo stadio riceve in input 
- La posizione dei pixel 
- Le informazioni di colore e profondità dei fragment

Ed esegue una serie di test sui fragment : Alpha test, depth test, scissor test, stencil test 

Nel caso in cui il test sia passato, le informazioni del fragment sono utilizzate per aggiornare il valore del pixel in base alla modalità di gest. delle trasparenze
	Gli stadi precedenti non hanno accesso al frame buffer 

Un fragment non è ancora un pixel : sono ancora necessarie fasi di elaborazione (es., di test), potrebbe essere utilizzata una tecnica di antialiasing, ecc. 
La relazione un pixel - un fragment non è quindi necessariamente vera

![[Pasted image 20241114150259.png]]

# Pipeline OpenGL (4.x)
Funzionalità aggiuntive
- Tessellation: stadio aggiuntivo (opzionale) in cui gruppi di vertici (e relativi dati) sono suddivisi in primitive più piccole 
- Utilizzo di un geometry shader (opzionale), che controlla l’elaborazione delle primitive (riceve in input una primitiva e può produrre in output da 0 a N primitive) 
- Buffer utilizzati, ad esempio, per memorizzare i dati prodotti dalla fase di trasformazione dei vertici

Le schede grafiche moderne consentono di programmare le funzionalità di alcuni degli stadi della pipeline: 
- vertex shader che rimpiazzano il comportamento predefinito dello stadio di trasformazione dei vertici 
- fragment shader che rendono controllabili le funzionalità dello stadio di textur./coloraz. dei fragment
- Altre possibilità, in base alla versione di OpenGL considerata (es., stadio di tessellation, geometry shader, ecc.)
- Oltre ad essere programmabili, nelle versioni più recenti addirittura shader come elementi per l’elaborazione generica (“compute”), uniti ad uno scheduling più flessibile degli stadi
## Vertex Processor

responsabile dell’esecuzione dei vertex shader, il cui input è rappresentato dalle informazioni (attributi) relative ai vertici definiti dal programmatore/dall’applicazione, ovvero posizione, colore, vettore normale, ecc. trasmesse allo shader dall’applicazione OpenGL
```
glBegin(...); 
	glColor3f(0.2,0.4,0.6); 
	glVertex3f(-1.0,1.0,2.0); 
	glColor3f(0.2,0.4,0.8); 
	glVertex3f(1.0,-1.0,2.0); 
glEnd();
```

In un vertex shader si possono scrivere istruzioni per eseguire operazioni di:
- Trasformazione della posizione dei vertici usando le matrici di modellazione e proiezione 
- Trasformazione delle normali e loro normalizzazione
- Generazione di coordinate di texture e trasformazione
- Determinazione del colore

Non è necessario programmare tutte le suddette operazioni
Tuttavia, se si scrive un vertex shader per rimpiazzare il comportamento predefinito, ciò che non verrà programmato non verrà gestito (lo shader è responsabile del funzionamento dell’intero stadio)

Il vertex processor lavora su un vertice per volta, e produce in output un solo vertice
Non è a conoscenza di informazioni di connettività, di quale sia la primitiva cui il vertice appartiene 

Non è quindi possibile eseguire in questo stadio operazioni che richiedono di conoscere la topologia della mesh: ad esempio, un vertex shader non può effettuare back face culling, in quanto non conosce gli altri vertici della faccia 

E’ responsabile di scrivere una variabile, denominata `gl_Position`, generalmente trasformando il vertice con le matrici `GL_MODELVIEW` e `GL_PROJECTION` 

Ha accesso allo stato OpenGL, può quindi eseguire operazioni che riguardano l’illuminazione, ad esempio usando i materiali, o le texture 

Non ha accesso al frame buffer, quindi ai pixel
## Fragment Processor

Il fragment processor si occupa invece della esecuzione dei fragment shader

È responsabile di operazioni che riguardano
- La determinazione dei colori e delle coordinate di texture per pixel
- L’applicazione delle texture
- Il calcolo delle normali nel caso di illuminazione per pixel

L’input è rappresentato dai valori interpolati di posizione, colore e normali ai vertici calcolati nel precedente stadio della pipeline (a partire dai valori calcolati, per vertice, dal vertex shader)

Come per il vertex processor, le istruzioni scritte in un fragment shader rimpiazzano il comportamento predefinito del relativo stadio della pipeline : il programmatore deve codificare tutti gli effetti richiesti dall’applicazione
Opera su singoli frammenti : non ha informazioni in merito ai fragment adiacenti
Ha accesso allo stato OpenGL

Non può cambiare le coordinate del pixel calcolate nel precedente stadio della pipeline
Può accedere alle informazioni di singoli pixel data la loro posizione, ma non ha accesso al frame buffer

Un fragment shader ha due possibilità: 
- scartare il frammento, e non produrre nulla in output
- calcolare la variabile `gl_FragColor` (il colore finale del fragment) 

Può anche determinare un valore di profondità. Potrebbe non essere richiesto, se calcolato nello stadio precedente

Si potrebbe creare
- Un vertex shader che riceva un colore tramite `gl_Color` e calcoli il colore delle facce rivolte verso l’osservatore, impostando `gl_FrontColor` a `gl_Color` 
- Combinato con un fragment shader che riceva un valore interpolato nella variabile `gl_Color` ed imposti `gl_FragColor` a quel valore

```C
// VERTEX SHADER
void main() { 
	gl_FrontColor = gl_Color; 
	gl_Position = gl_ProjectionMatrix * gl_ModelViewMatrix * gl_Vertex; 
}

// FRAGMENT SHADER
void main() { 
	gl_FragColor = gl_Color; 
}
```

# Pipeline Grafica e Hardware

Necessità 
- Gestire scene composte anche da centinaia di migliaia o milioni di triangoli
- Eseguire calcoli complessi nei vertex e fragment shader
- Produrre immagini per schermi ad elevate risoluzioni (es. 10 milioni di pixel e sovracampionamento), anche più d’uno, es., per visori di realtà virtuale
- Operare a frame rate potenzialmente elevati (30-120 fps)

Processori specializzati nell’esecuzione degli stadi della pipeline di rasterizzazione
![[Pasted image 20241114154602.png]]

Approccio misto, con core per operazioni generiche, ma anche per operazioni specializzate
![[Pasted image 20241114154554.png]]

Recentemente, hardware specializzato anche per il ray tracing
![[Pasted image 20241114154636.png]]
## Algoritmo Ray-Tracing

Algoritmo base, ray casting
	Inizialmente sviluppato per calcoli balistici/particellari, per la valutazione di operatori booleani, ecc. 
	Utilizzabile/utilizzato per la determinazione delle superfici visibili

Ray tracing, algoritmo di rendering
	Integraz. del calcolo del colore ed estens. ricorsiva per la gestione delle riflessioni speculari, rifrazioni ed ombre
![[Pasted image 20241114155743.png]]

Passo chiave:
Determinare l’intersezione di un raggio con una qualsivoglia superficie
Può essere complicato per alcuni tipi di superfici (lento)
Come nello z-buffer, solo intersezioni raggio-oggetto, non tra oggetti (image precision), seppur in numero anche molto elevato
