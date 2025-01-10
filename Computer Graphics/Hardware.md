## Schermi CRT

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
![[Pasted image 20241210122606.png]]Limite alla 

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

# Sistemi di Visualizzazione Raster

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
- Non solo la quantità di dati trasferibili nell’unità di tempo
- Ma la strada che questi percorrono all’interno del sistema
L’AGP ottimizza uno dei “colli di bottiglia” dell’architettura dei calcolatori
Permette di utilizzare direttamente la memoria di sistema per immagazzinare le texture con un meccanismo di allocazione dinamica da parte del sistema operativo nei confronti del sistema grafico 
Le texture possono essere lette direttamente dalla memoria di sistema senza alcuna operazione di prefetch nel frame buffer del sistema video

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