## Rasterizzazione vs Ray tracing

- Ordine in cui si elaborano i campioni
![[Pasted image 20241126114156.png]]

- Approccio per determinare la visibilità
![[Pasted image 20241126114237.png]]

- Sofisticazione del modello di illuminazione
![[Pasted image 20241126114307.png]]

Forward ray tracing (in analogia al comport. fisico) 
- Si tracciano direttamente raggi lum. a partire dalla sorgente 

Backward ray tracing (considerato finora) 
- Si tracciano raggi luminosi “immaginari” verso gli oggetti a partire dal punto di vista dell’osservatore 
- Approccio image-precision (dipend. dal punto di vista) 
- Viene selezionato un centro di proiezione (il punto di vista) e una finestra su di un piano di vista arbitrario, una griglia composta di pixel alla risoluzione desiderata 
- Per ogni pixel nella finestra si traccia un raggio di vista dal centro di proiezione verso la scena passando per il punto 
- Viene determinata la visibilità (determinazione delle superfici visibili) / viene impostato il colore del pixel a quello dell’intersezione con l’oggetto più vicino (rendering)

Rasterizzazione
- Applica i modelli di illuminazione locali nei punti identificati tramite determinazione delle superfici visibili - Questi modelli forniscono una risposta approssimata alla domanda base: nota la luce che arriva in un punto di una superfice, quale è la luce riflessa lungo una det. direzione? ¨ Problema analizzato solo a livello locale, senza considerare interazioni multiple, ecc. 

Ray tracing 
- Applica più volte (in modo ricorsivo) i modelli di illuminazione locali nei punti di intersezione del raggio con le geometrie, gestendo riflessioni e rifrazioni multiple 
- Metodo/Modello per gestire fenomeni di illuminaz. globali 
	- Altri approcci: es., radiosity e photon mapping

Ray casting
Operazione base (casting) 
- Raggio verso un oggetto
	- Non necessariamente per renderizzare un’immagine 
- Rendering se il raggio
	- Parte dal punto di vista 
	- Attraversa un pixel (sul piano dell’immagine) 
	- Raggiunge il primo punto visibile sulla superficie dell’oggetto 
	- In quel punto si utilizza un modello di illuminazione (che considera la/le sorg. di luce) per calcolare il colore 
	- Se tutto qui, simile a rasterizzaz.
![[Pasted image 20241126114509.png]]

Caso più semplice: Considerata solo la luce proveniente dalla/e sorgente/i (illuminazione diretta)
![[Pasted image 20241126114525.png]]

Caso più generale: La luce incidente può essere il risultato di altre riflessioni nella scena, e non arrivare (solo) direttamente da un oggetto che emette luce: illuminazione indiretta
![[Pasted image 20241126114541.png]]

Calcolo dell’eq. di ill. in modo ricorsivo, seguendo/tracciando il percorso della luce: ray tracing

Tracciati anche raggi secondari (rispetto al raggio primario, proiettato dal punto di vista) 
- Riflessioni speculari e rifrazioni (trasparenze) 
	- Raggi riflessi e raggi rifratti proiettati dalle intersezioni, in modo condizion., in base alle proprietà del materiale (BRDF) 
	- Possono generare altri raggi (ricorsione), es. riflessi/trasmessi 
- Ombre 
	- Un ulteriore raggio (d’ombra) tracciato da ogni intersezione verso ciascuna sorgente di luce 
	- Se il raggio interseca un oggetto, allora quel punto è in ombra e, nel calcolo della illuminazione, si ignorerà il contributo di quella sorgente per quel punto

Algoritmo base, “classico” 
- Per ogni pixel nella finestra si traccia (proietta) un **raggio di vista** dal centro di proiezione verso la scena passando per il pixel, e si determina la prima intersezione (virtuale) con la superficie di un oggetto
- Dal punto di intersezione, si tracciano i due raggi secondari ($I_r$ ed $I_t$ intensità del raggio riflesso e rifratto/trasmesso, $k_t$ coefficiente di trasmissione) e, tenendo conto anche dei raggi d’ombra (uno per sorgente di luce, per quel punto) si calcola il colore del pixel come
![[Pasted image 20241126114606.png]]

![[Pasted image 20241126114620.png]]

Ray tracing, limiti
Con l’algoritmo base, classico 
- Gli effetti delle sorgenti di luce indiretta, ovvero riflessa o rifratta, non sono corretti 
	- Es. L3 non è soggetto alla giusta rifrazione, una sorgente di luce riflessa in uno specchio non produce ombre, ecc. 
- Problemi di precisione numerica (inters. e raggi secondari)
- Resa inferiore con luce diffusa 
	- Troppo complesso considerare tutti i raggi provenienti da riflessioni multiple: si può utilizzare, es., una componente di luce ambiente per modellare il contributo dell’illuminazione globale
- Generazione di immagini in un certo senso “innaturali” 
	- Riflessioni speculari nette, ombre nette, aliasing

Ray tracing distribuito
Alcuni limiti dovuti al fatto che si utilizza, pur in maniera ricorsiva, un numero limitato di raggi per simulare fenomeni complessi 
Evoluz.: approccio distribuito (comunque biased, intr. errore sistematico nella calcolo della radianza) ¨ Strategia che permette, mediante il tracciamento di più raggi secondari la cui direzione è distribuita a seconda del fenomeno da riprodurre (simulazione di numerosi effetti) 
- Rifl. speculari sfumate 
- Penombra 
- Traslucenza 
- Profondità di campo 
- Motion-blur 
- Antialiasing

Ray tracing Monte Carlo
Determinare il colore di un punto 
- Senza distinguere tra raggi primari e secondari, ma sempre con ricorsione 
- Considerando, in ogni intersezione, il contributo alla radianza di tutte le possibili direzioni della luce incidente 
- Problema affrontabile mediante campionamento ed integrazione
	- Concetto di campione diverso rispetto alla rasterizzazione 
	- Non pixel, ma direzioni (o percorsi, con ricorsione) della luce
![[Pasted image 20241126114711.png]]
Metodo di integrazione Monte Carlo 
- Scelta di un certo num. di campioni e calcolo della media
	- Più campioni si prendono, più il risultato sarà accurato, garanzia di convergenza al valore corretto (consistenza) - Rimozione della varianza/del rumore 
	- Particolarmente efficiente se si scelgono con maggior probabilità i campioni che appartengono alle regioni che contribuiscono maggiormente al calcolo dell’integrale (pesati in maniera opportuna) - Campionamento per importanza (importance sampling) 
- Nel rendering di immagini, ray tracing Monte Carlo 
	- Calcolo della media dei valori di radianza ottenuti dall’equazione di illuminazione

![[Pasted image 20241126124518.png]]

Ray/Path tracing e ricorsione
Quando fermarsi nella ricorsione? 
- Certamente quando il raggio, emesso dal punto di vista, incontra una sorgente di luce, ma non solo 
- Quando si raggiunge un limite di profondità nell’albero dei raggi (in genere qualche unità)? 
- Siccome i vari coefficienti di trasmissione/riflessione sono minori di 1, quando il contributo di un raggio è minore di una determinata soglia)? 
- Gli ultimi due metodi possono determinare la perdita di importanti contributi all’immagine 
	- Il risultato del calcolo dell’illuminazione non converge più al valore corretto, errore sistematico (distorsione, bias) 
	- Meglio scartare i raggi che hanno un basso contributo in maniera casuale, es. “roulette russa” (approccio unbiased)

Introdotto comunque un bias 
- Esempio : Per uno specchio perfetto, la probabilità che il campione selezionato corrisponda all’unica direzione di riflessione (ideale) è incredibilmente bassa
- Potrebbe sembrare non così critico, visto che nella realtà non esistono superfici ideali 
- Tuttavia, anche in condizioni non ideali, la probabilità che un raggio emesso dal punto di vista (campione), dopo un certo numero di rifl., raggiunga una sorgente di luce magari piccola (approccio backward) può essere bassa 
	- Conviene scegliere i percorsi “giusti” (anche per velocizzare la convergenza, efficienza) 
	- Ma come sceglierli, quali sono?

Ray/Path tracing e campionamento
Campionamento per importanza 
- Si possono sfruttare le informazioni relative al materiale (BRDF): direzioni privilegiate 
Campionamento per importanza multiplo
- Informazioni non solo dai materiali, ma anche dalle sorgenti di luce (multiple importance sampling) 
- Si possono tracciare anche raggi verso di esse, se si ritiene possano contribuire significativamente alla illum. del punto

Ray/Path tracing e percorsi
Nell’approccio classico (backward) ci si ferma quando si raggiunge una sorgente, o in base a tecniche che interrompono la ricorsione secondo un qualche criterio
Approccio bi-direzionale : Si “collegano” il percorso backward (dal punto di vista verso la sorgente) e quello forward (dalla sorgente verso il punto di vista). Approccio che aiuta a scegliere i percorsi da considerare
![[Pasted image 20241126124939.png]]

Altra possibilità : Quando si trova un “buon” percorso (ad esempio, guardando quanto cambia il valore dell’illuminazione calcolata di percorso in percorso), lo si perturba per trovare altri percorsi “buoni” nelle sue vicinanze - Es. metodo Metropolis Light Transport (MLT)
![[Pasted image 20241126124956.png]]

Radiosity
Approccio altern. per gestire fen. di illum. globali
- Basato su modelli per lo studio dell’emissione e della riflessione di radiazioni elettromagnetiche
- Assunzione: 
	- Conservazione dell’energia luminosa in un ambiente chiuso 
	- L'energia emessa o riflessa dalle superfici viene riflessa o assorbita dalle altre (condizione di equilibrio)
	
Radiosità: energia che lascia la superficie (emessa o riflessa/trasmessa) per unità di superficie
Si determinano dapprima tutte le interazioni luminose nella scena in maniera indipendente dal punto di vista (poi intensità di energia incidente su una superficie, rendering)

Sorgenti luminose : Finora trattate separatamente rispetto alle sup. illuminate
Ora, ogni superficie può emettere luce 
- Tutte le sorgenti di luce sono quindi modellate come se dotate di un’area 
	- Ambiente suddiviso un un numero finito n di regioni discrete, che emettono o riflettono luce in maniera uniforme 
	- Assunzione: regioni come superfici Lambertiane opache che emettono e riflettono solo luce diffusa, illuminazione costante 
		- $B_i$ e $B_j$ radiosità di due superfici i e j (energia per unità di tempo per unità d’area, es. W/m2) 
		- $E_i$ energia emessa (stessa unità di misura di B ) 
		- $ρ_i$ riflettività (adimensionata) 
		- $F_{j→i}$ fattore di forma (frazione di energia che lascia j ed arriva ad i , tiene conto della posizione reciproca, indipendente da punto di vista e caratteristiche superficie) 
		- $A_i$ e $A_j$ aree di i e j

Equazione : Radiosità per una regione i
$$
B_i = E_i + \rho_i\sum_{1\le j\le n}B_jF_{j\rightarrow i} A_j | A_i
$$
Considerando che $A_i F_{i→j} =A_j F_{j→i}$ (reciprocità)
$$
B_i - \rho_i\sum_{1\le j \le n} B_jF_{i\rightarrow j} = E_i
$$
Sistema di n equazioni

A partire da 
- Geometrie, riflettività, sorgenti di luce, det. fattori di forma
- Calcolo della radiosità per le regioni
- Rendering delle regioni da un punto di vista arbitrario 
	- Con un alg. per la det. delle superfici visibili convenzionale e ombregg. delle superf. (eventualmente con interpolazione)

Photon mapping
Tecnica di rendering a due passi, che combina i vantaggi del forward e backward ray tracing
- Nel primo passo, fotoni tracciati dalla sorgente luminosa 
	- Utilizzati per generare informazioni sull’illuminazione della scena 
	- Ogni volta che un fotone viene assorbito da una superficie, si memorizza posizione, direzione di incidenza e flusso (colore e potenza) del fotone in una una struttura dati (photon map) 
- Il secondo passo calcola il rendering finale raccogliendo (“gathering”) i contributi dei fotoni dal punto di vista dell’osservatore 
	- Raggi tracciati dal punto di vista verso la scena per determinare la visibilità di un punto, poi la photon map è utilizzata per ottenere il suo valore di illuminazione

Dato un certo numero di fotoni da emettere nella scena e un insieme di sorgenti luminose 
Per ogni fotone
- Si sceglie casualmente una sorgente luminosa di emissione (con probabilità proporzionale alla sua potenza)
- Si sceglie un punto di emissione sulla superficie della sorgente (con probabilità uniforme)
- Si sceglie la direzione con cui il fotone lascia la sorgente (con probabilità legata alla direzione, si emette con maggior probabilità lungo la direzione normale nel punto) 

Un fotone emesso percorre un cammino, che si calcola in funzione delle caratteristiche delle superfici che incontra: 
- Quando un fotone colpisce una superficie avviene lo scattering e può essere: riflesso, trasmesso, assorbito 
- Si salvano le informazioni in caso di assorbimento


Confronto: assumendo due tipi di superfici ideali nella scena (riflessione diffusa e speculare)
- 4 modalità di trasmissione della luce tra superfici
	- Path tracing modella la prima (anche seconda e terza nella variante distribuita)
	- radiosity l'ultima
	- photon mapping tutte
![[Pasted image 20241204175619.png]]

- Ray/Path Tracing : Discretizza il piano di vista per determinare i punti in cui valutare l’equazione di illuminazione 
	- Particolarmente adatto per gestire riflessioni speculari 
	- Carico di lavoro ulteriore per modellare fenomeni di diffusione che variano di poco per grandi aree dell’immagine o tra immagini generate a partire da punti di vista diversi (dipendenza dal punto di vista) 
	- Tutte le tecniche considerate hanno un elevato carico computazionale, dipendono fortemente dalla complessità della scena, e non sono in genere calcolabili in tempo reale (a meno di non utilizzare hardware specifici)

- Radiosity : Discretizza la scena e la elabora per ottenere informazioni sufficienti per valutare l’equazione di illuminazione in ogni punto e da ogni direzione di vista (indipendente dal punto di vista) 
	- Resa delle ombre nette non sempre adeguata 
	- Modella fenomeni di diffusione in modo efficiente (ma richiede molta memoria per catturare abbastanza informazioni utili a gestire riflessioni speculari) 
	- Permette il rendering in tempo reale o quasi di prospettive diverse della stessa scena

- Photon mapping : Tecnica dipendente dal punto di vista, come ray/path tracing. Consente però di conservare e riutilizzare molte informazioni, in quanto le “mappe” sono indipendenti dal punto di vista (i fotoni sono emessi una sola volta, e le informazioni ad essi relative possono essere riutilizzate se si cambia il punto di vista)


Blender usa 3 motori di rendering integrati:
- **Workbench**, anche noto come Blender Render, non pensato (almeno, non più) come motore di rendering (finale), ma strumento ottimizzato per visualizzazione rapida nelle fase di modellazione/animazione (preview) usando rasterizz.
- **Eevee** (Extra Easy Virtual Environment Engine), basato su OpenGL, codice simile a quello di Unreal Engine, progettato per offrire velocità elevata/interattività (al costo di un’accuratezza inferiore); non usa raggi, ma rasterizz., seppure orientato al realismo, pensato per il rendering in tempo reale
- **Cycles**, basato su path tracing, utilizza raggi di vista (camera), d’ombra, riflessi, e trasmessi, con gli ultimi due ulteriormente suddivisi a seconda che si considerino fenomeni di rifl. o trasm. diffusa o speculare)


Ray/Path tracing ed intersezioni
Operazione base degli algoritmi visti : Intersezione raggio-triangolo 
- Verifica del fatto che il raggio intersechi il triangolo 
- Determinazione del punto esatto di intersezione 
Come z-buffer, solo intersezioni raggio-oggetto, non tra oggetti (image-precision) 
Operazione complessa, eseguita su geometrie composte da numeri anche anche molto elevati di triangoli 
Utilizzate ottimizzazioni, es. per semplificare/ridurre il numero di intersezioni da calcolare, parallelizzare, ecc.

Calcolo dell’intersezione:
- Usando le coordinate baricentriche per rappr. il triangolo in forma parametrica come $$f(u,v)=(1-u-v)p_0+up_1+vp_2$$
- Esprimendo il raggio in forma parametrica come $r(t)=o+td$
- Ed imponendo l’uguaglianza tra le due funzioni
- Si ottiene in triangolo unitario sul piano $u$, $v$ e il raggio ortogonale al piano, semplificando il calcolo
![[Pasted image 20241204180921.png]]

L’interesse, in particolare, è per il “primo” triangolo intersecato 
Però, se si calcola l’intersezione raggio-triangolo per tutti i triangoli e poi si considera solo il più vicino
La complessità è $O(N)$ , con N il numero di triangoli

Si può fare meglio? Utilizzando, ad es., dei bounding box (volume) 
Test min-max : Se il raggio interseca il bounding box, allora bisogna considerare i vari triangoli all’interno § Non molto meglio 
Sempre $O(N)$, nel caso pegg. (interseca) 
Oltre a costr. bounding box ed intersezione raggio-bounding box

Come semplificare il problema? Se si trattasse, ad es., di cercare il valore più vicino ad un valore dato in un array di interi? 
- Approccio base : Complessità $O(N)$
- Approccio migliore, esempio ordinando prima l’array : Complessità $O(N \log N )$, Ma $O(N)$ per ordinamento, ammortizzabile sul numero di ricerche da effettuare, $O(\log N)$ se si considera solo la ricerca 
- Ottimizzazione ottenuta riorganizzando i dati

Si possono riorganizzare anche i triangoli in modo da ottimizzare il calcolo delle intersezioni?
Caso semplice : Il raggio non interseca il bounding box che contiene tutti i triangoli della scena
![[Pasted image 20241204181828.png|350]]   ![[Pasted image 20241204181843.png|280]]

Si può fare meglio : Applicando l’approccio basato su bounding box/volume in maniera gerarchica: Bounding Volume Hierarchy (BVH)
![[Pasted image 20241204182154.png]]

Suddivisione dei triangoli tra i nodi: 
I nodi foglia contengono liste (ridotte) di triangoli 
I nodi interni descrivono i bounding box 

Partizioni disgiunte di triangoli: Ciò nonostante, le partizioni possono sovrapporre nello spazio, negativo dal punto di vista della complessità
![[Pasted image 20241204182343.png]]

Ulteriori ottimizzazioni: migliori suddivisioni (euristiche)
![[Pasted image 20241204182331.png]]

Alternativa? Anzichè partizionare/suddividere i triangoli, in insiemi disgiunti (che possono però sovrapporsi nello spazio)
--> Partizionare/Suddividere lo spazio, usando griglie regolari oppure gerarchiche/adattative (es. strutture ad albero) : I triangoli possono però essere contenuti in più regioni
![[Pasted image 20241204182447.png]]

**Binary Space Partitioning** (BSP): Dividono lo spazio in coppie di sotto-spazi separati da un piano di posizione e orientamento arbitrari
- Ogni nodo dell’albero è associato ad un piano e possiede un puntatore ai due sotto-spazi identificati dal piano
- Utilizz. per descr. oggetti solidi 
	- Con normali uscenti dall’oggetto, il figlio di sinistra è dentro l’oggetto (dietro il piano), quello di destra fuori (davanti)
	- Sudd. ricorsiva con regioni omogenee interamente all’interno o all’esterno dell’oggetto (celle in ed out
![[Pasted image 20241204182548.png]]

K-Dimensional tree (K-D tree) 
- Spazio suddiviso usando piani allineati agli assi principali 
- I nodi interni descrivono le suddivisioni spaziali
- Divers. da BVH, si termina il calcolo alla prima intersezione
![[Pasted image 20241204185534.png]]

Griglia uniforme : Spazio suddiviso in volumi di ugual dimensione (elementi di volume, o voxel, utilizzabili anche per desc. ogg. solidi) 
- Ogni cella contiene triangoli che possono occupare più di un voxel
- Struttura molto semplice da costruire
- Si attraversa il volume in ordine 
	- Possibili impl, molto efficienti (rasterizzaz. di un linea in 3D) 
	- Si calcolano insers. solo con le celle intersecate dal raggio
![[Pasted image 20241204185612.png]]

Possibile euristica: Numero di celle paragonabile al numero di triangoli 
- Numero costante di triangoli per cella, con una distribuzione uniforme dei triangoli
- Complessità $O(\sqrt[3] N )$, non necessariamente inferiore a $O(\log N)$

Griglia uniforme adatta per alcune scene, ma non necessariamente per tutte. La griglia uniforme non si “adatta” alla disposizione dei triangoli nella scena

Alternativa: **oct-tree** deriva dalla rappresentazione quad-tree (per immagini) 
- Suddivisione di un piano nelle due dimensioni per creare quadranti pieni, parzialmente pieni, e vuoti in base a quanto intersecano l’area da rappresentare, numerati da 0 a 3 (zigzag, da in basso a sinistra a in alto a destra) 
- Quadranti parzialmente pieni suddivisi nuovamente in modo ricorsivo in sotto-quadranti (fino a quando tutti i quadranti sono omogenei, tutti pieni oppure vuoti, o fino ad un livello)

Octree, suddivisione dello spazio in tre dimensioni, usando ottanti da 0 a 7 
Come per la griglia uniforme, la costruzione è semplice, non occorre scegliere come posizionare i piani per suddividere lo spazio 
Si adatta però maggiormente alla distribuzione dei triangoli nella scena 
Capacità di adattamento inferiore a K-D tree, che ha però una complessità superiore

Scelta della struttura dati/strategia di ottimizzazione
- Partizionamento 
	- Delle primitive: numero limitato di nodi, e facile da costruire/aggiornare, es. se le primitive si spostano (BVH)
	- Dello spazio: facile attraversare lo spazio (in ordine), ma la stessa primitiva potrebbe essere intersecata più volte 
- Approcci gerarchici/adattativi (BVH, K-D tree, …) 
	- Costruzione complessa (costo da ammortizzare), ma complessità minore del calcolo delle intersezioni per distribuzioni dei triangoli non uniformi 
- Approcci non adattativi (griglia uniforme) 
	- Costruzione facile, bassa complessità del calcolo delle intersezioni per triangoli distribuiti in modo uniforme 
- Molte combinazioni possibili, scelta in base all’obiettivo