sistemi vegetali

piante hanno strutture ramificate --> ricorsione

piante modellate in modi diversi: 
- alberi come particelle --> extension blender

piante crescono e cambiano

Strutture di ramificazione
![[Screenshot 2025-05-06 at 4.36.12 PM.png|400]]

Componenti strutturali
![[Screenshot 2025-05-06 at 4.36.55 PM.png|400]]

piante erbacee: leggere, piccole, non risente effetti ambientali come gravità e vento (non modificano in modo permanente il sistema)

piante con fusto in legno: pesanti, strutturate, soggette a effetti ambientali come gravità

Fusto esce dal terreno e cresce verticalmente verso l'alto e da origine a foglie
- ramificazione:
![[Screenshot 2025-05-06 at 4.40.51 PM.png|300]]

 gemme = embrioni di fusti, foglie, fiori
 ![[Screenshot 2025-05-06 at 4.42.35 PM.png|500]]

Foglie: nascono da gemme vegetative
![[Screenshot 2025-05-06 at 4.43.09 PM.png|500]]

Crescita : 
- invecchiamento
- trasmissione cellulare : si riferisce al passaggio di nutrienti e ormoni tra cellule adiacenti. processo di trasmissione cellulare è bidirezionale
- trofismo : è la risposta della pianta a influenza esterne come la luce (foto-trofismo) e gravità (geo-trofismo)
- ostacoli : ostacoli fisici possono influenzare la forma di una pianta

gli L-system son dei modelli matematici concepiti per rappresentare le piante e il loro sviluppo dal biologo Aristid Lindenmayer

Il caso più semplice è quello dei sistemi deterministici e context-free (D0L - systemˆ2)
	vengono specificate un insieme di regole di produzione, dove si hanno predecessori e successori
	nei sistemi deterministici il predecessore è specificato una volta sola come termine sinistro di una regola di produzione

Una sequenza di simboli è specificata come stringa iniziale
Una regola di produzione può essere applicata ad una stringa se il simbolo compare come termine sinistro della regola stessa:
	- le regole di produzione sono applicate in parallelo ai simboli della stringa
	- per i simboli della stringa che non hanno una corrispettiva regola di produzione vale 
	  pred--> pred
L'applicazione in parallelo delle regole di produzione produce una nuova stringa sulla quale vengono nuovamente applicate le stesse regole
![[Screenshot 2025-05-06 at 4.56.51 PM.png|400]]

Le stringhe prodotte da un L-system sono solo sequenze di simboli: per produrre immagini da una stringa è necessaria un'interpretazione geometrica
Esistono due modi per interpretare geometricamente una stringa: 
- sostituzione geometrica
- grafica della tartaruga

### Sostituzione geometrica
ogni elemento della stringa è sostituito mediante un elemento geometrico
![[Screenshot 2025-05-06 at 4.59.49 PM.png|500]]

### grafica della tartaruga
La geometria è ricavata interpretando i simboli della stringa come comandi di disegno da impartire ad un cursore "turtle"
Lo stato della tartaruga è esprimibile mediante una terna $(x,y,\alpha )$ dove X e Y sono coordinate spaziali, e $\alpha$ rappresenta l'orientamento rispetto ad una direzione di riferimento
L'utente può specificare il passo di avanzamento lineare e rotazionale

![[Screenshot 2025-05-06 at 5.04.09 PM.png|500]]
![[Screenshot 2025-05-06 at 5.05.15 PM.png|500]]
![[Screenshot 2025-05-06 at 5.05.24 PM.png|500]]

Bracketed L-System : parentesi utilizzate per delimitare l'inizio e fine di punti di diramificazione aggiuntivi
\[ = inserzione  
\] = estrazione

![[Screenshot 2025-05-06 at 5.14.29 PM.png|400]]

L-System stocastici
l'utente assegna un peso (probabilità) ad ogni regola di produzione:
- somma delle probabilità deve essere 1
- probabilità indicano quanto è probabile che una regola di produzione formi una ramificazione

Animazione del processo di crescita
- cambiamenti nella topologia
- allungamenti della struttura esistente

I cambiamenti nella topologia son modellati mediante sistemi L-system e avvengono ad intervalli discreti

L'allungamento (elongazione) può essere modellata mediante regole di produzione del tipo:
- F -> FF (allungamento sempre uguale)
- regole di produzione "in serie"

le regole di produzione in serie presentano il problema di richiedere un numero eccessivo di simboli
L'utente può ovviare a questo problema rappresentando l'operazione di disegno in modo parametrico: L-System parametrici

L-System parametrici

L-System combinati

L-System temporizzati

interazioni con l'ambiente


# Frattali e caos

Effetto Farfalla : microscopiche oscillazioni dei valori sono in grado di provocare grossi mutamenti a livello macroscopico, ogni piccolo errore di misura si rivelava catastrofico, capace di amplificarsi enormemente e in modo del tutto imprevedibile, fino a sconvolgere completamente le previsioni attese

Teoria del caos : pone limiti definiti alla prevedibilità dell'evoluzione di sistemi complessi non lineari
Nei sistemi lineari, una piccola variazione nello stato iniziale di un sistema provoca una variazione altrettanto piccola nel suo stato finale
La variazione è prevedibile utilizzando le normali equazioni della fisica

basta una minima differenza fra due condizioni iniziali per ottenere traiettorie completamente diverse. Il caos non contempla gemelli

Un sistema può anche comportarsi in modo caotico in certi casi e non caotico in altri.
ES: rubinetto

Sistemi frattali: figure geometriche caratterizzate dal ripetersi sino all'infinito di uno stesso motivo su scala sempre più ridotta
Questa è la definizione più intuitiva che si possa dare di figure che in natura si presentano con una frequenza impressionante ma che non hanno ancora una definizione matematica precisa
L'atteggiamento corrente è quello di considerare frattale un insieme F che abbia proprietà simili a:
- autosimilarità
- struttura fine
- irregolarità
- dimensioni di autosimilarità > della dimensione topologica

### Autosimilarità
man mano che zoommo si ritrova la figura originale
![[Screenshot 2025-05-06 at 6.22.42 PM.png|500]]

### Struttura fine
dettagli ad ogni ingrandimento
![[Screenshot 2025-05-06 at 6.23.41 PM.png|500]]

### Irregolarità
non si può descrivere come luogo di punti che soddisfano condizioni geometriche o analitiche
La funzione è ricorsiva

### Dimensioni di autosimilarità > della dimensione topologica

La caratteristica di queste figure, caratteristica dalla quale deriva il loro nome, è che, sebbene esse possano essere rappresentate in uno spazio convenzionale a due o tre dimensioni, la loro dimensione non è intera. 
In effetti la lunghezza di un frattale "piano" non può essere misurata definitamene, ma dipende strettamente dal numero di iterazioni al quale si sottopone la figura iniziale.


Dimensione: la dimensione di un insieme è un numero di parametri indipendenti necessari alla descrizione di un punto dell'insieme
Un concetto matematico che modella fedelmente questa idea ingenua è la dimensione topologica di un insieme
![[Screenshot 2025-05-06 at 6.27.54 PM.png|400]]

altre definizioni di dimensione
1. Una misura di estensione spaziale; in particolare, larghezza, altezza o lunghezza.
2. Il minimo numero di coordinate indipendenti richieste per specificare univocamente i punti in uno spazio. 
3. La prima definizione formale fu enunciata da Brouwer nel 1913: 
	- “A (solid) cube has the topological dimension of three because in any decomposition of the cube into smaller bricks there always are points that belong to at least four (3+1) bricks.”

![[Screenshot 2025-05-06 at 6.30.30 PM.png|600]]

Frattale di KOCH (snow flake)
![[Screenshot 2025-05-06 at 6.33.36 PM.png|600]]

Dimensione di autosimilarità: $\frac{\log (4)}{\log (3)} = 1.26$

Dimensione di Koch (island)
![[Screenshot 2025-05-06 at 6.37.01 PM.png|600]]

Dimensione di autosimilarità: $\frac{\log (8)}{\log (4)} = 1.5$

Pure essendo continue non ammettono una tangente unica in alcun punto
presi due punti della curva la distanza è sempre infinita
si può dimostrare che a successione rappresentata dalla somma dei lati del fiocco di neve è divergente, cioè tendente ad infinito

curve frattali : esempi
felci, broccolo romanesco, pino, abete, fulmine