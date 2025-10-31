Curve e superfici : richieste in diverse applicazioni grafiche per simulare la geometria di oggetti fisicamente esistenti, per definire la geometria di oggetti non ancora esistenti ecc...

Geometria = studio dello spazio e delle figure spaziali, insieme di regole pratiche per la misurazione e rappresentazione di superfici ed altre grandezze

Rappresentazioni:
![[Pasted image 20241119114609.png|500]]

Superficie = parte esterna di un oggetto
diverse codifiche digitali per le superfici:
- esplicite : nuvole di punti, mesh poligonali, bezier...
- implicite : superfici algebriche, CSG, insiemi di livello, frattali

## Rappresentazioni Implicite
I punti non sono noti in maniera diretta (ovvero esplicita), ma soddisfano una qualche relazione
In generale, da tutti i punti per cui f (x,y,z)=0

Difficile da verificare se un punto fa parte della superficie se rappresentata esplicitamente, mentre implicitamente basta controllare la relazione

#### Constructive Solid Geometry CSG
rappr. implicita che va a creare/descrivere forme complesse attraverso operazioni booleane di base
Operazioni di base concatenate a formare espressioni in una struttura ad albero
![[Pasted image 20241119115011.png]]

#### Meta-superfici
invece di sfruttare op. Booleane "nette" vanno ad effettuare una miscelazione
![[Pasted image 20241119115104.png]]

#### Level Set (insiemi di livello)
Griglie di valori che approssimano la funzione, superficie = dove i valori interpolati sono = 0
Problemi : aliasing, memorizzazione
![[Pasted image 20241119115513.png]]
#### Frattali
Si ripete nella forma su scale diverse. Ingrandendo una qualunque parte si ottiene qualcosa di simile all'originale. Usato per rappresentare fenomeni naturali. Complicato controllare la forma
![[Pasted image 20241119115628.png]]

### Pro e Contro delle Rappr. Implicite
Pro:
- descrizione compatta
- facile determinare se un punto è all'interno della superficie
- operazioni possono risultare semplici
- per forme semplici : rappresentazione esatta
- facile gestire cambiamenti nella topologia

Contro:
- complicato individuare tutti i punti della superficie
- complicato descrivere forme complesse

## Rappresentazioni Esplicite

#### Nuvole di Punti
Elenchi di punti spesso accompagnati dalle normali
Facile rappresentare qualsiasi geometria
Facile ottenere nuvole dense
Difficile interpolare regioni sotto-campionate
complicate da elaborare (in simulazioni)
![[Pasted image 20241119115929.png]]
#### Mesh Poligonali
memorizzano oltre ai vertici, i poligoni (spesso triangoli o quadrilateri)
facili da elaborare
campionamento adattivo
strutture dati complicate (irregolari)
![[Pasted image 20241119120016.png|300]]

Vertici memorizzati come triplette di coordinate, triangoli memorizzati come triplette di indici
Punti appartenenti al triangolo ottenuti per interpolazione (coordinate baricentriche)

Possono essere utilizzate per realizzare una forma qualsiasi
- Oggetti costituiti da superfici piane vengono rappresentati esattamente
- Oggetti con superfici curve possono invece essere rappresentati solo in modo approssimato
	- Spezzata, composta da polinomi del primo grado
	- Risultati migliori, sempre approssimati, aumentando il numero di poligoni al costo di maggiori requisiti in termini di memoria e risorse/tempi di calcolo

## Poligoni vs Polinomi

Funzioni di grado n con n > 1
Possono eventualmente solo approssimare le forme curve desiderate 
- Occupando però meno spazio
- Semplificando le operazioni di manipolazione/interazione 

Superfici in forma esplicita:
- Rappresentazione di y e z come funzioni di x
	- Impossibile ottenere valori multipli di y per un dato x (quindi, curve come circonferenze ed ellissi devono essere rappresentate mediante segmenti multipli)
	- Definizione non invariante per quel che riguarda le rotazioni (descrivere una versione ruotata della curva richiede una gran mole di lavoro e spesso è necessario anche in questo caso ricorrere a segmenti multipli)
	- Difficile descrivere curve con tangenti verticali (ovvero rappresentare pendenze infinite)

- Rappresentazione parametrica con x, y, z funzioni di un parametro t
	- Permette di superare i limiti delle altre rappresentazioni
	- Si rimpiazzano le pendenze geometriche (che possono essere infinite) con vettori parametrici tangenti (che non sono mai infiniti)
	- Ogni segmento Q della curva complessiva è definito da tre funzioni x, y e z che sono polinomi di grado n nel parametro t

# Curve Parametriche

In genere n = 3, polinomi di grado tre (curva cubica, superficie bi-cubica)
- Per avere sufficiente flessibilità nel controllare la forma
- Ordine k (grado n=k -1), k =n +1 punti di controllo 

Motivazioni, per una curva 
- Grado tre: quattro coefficienti/vincoli (es., i due estremi e i due valori della derivata negli estremi stessi) 
- Grado due: non adatta, tre coefficienti, due estremi più un’altra condizione, es. una pendenza, o tre punti che definiscono un piano
- Grado uno: linea retta/segmento, due coeff. determinati dai due estremi (derivata già definita, non controllabile)
- Di grado superiore a tre: costi di elaborazione troppo alti rispetto ai vantaggi ottenibili (es., efficienza aerodinamica)

Polinomi di grado tre che definiscono un segmento di curva $Q(t) =[x(t) y(t) z(t)]^T$ sono nella forma
![[Pasted image 20241119123505.png]]
$Q (t)=C\cdot T$, definendo $T=[t^3 \ t^2 \ t \ 1]^T$ e scrivendo la matrice dei coefficienti come
![[Pasted image 20241119123603.png]]

![[Pasted image 20241119123614.png]]

Concetto di (livelli di) continuità (tra segmenti di curva) 
- La derivata di Q(t) rappresenta il vettore parametrico tangente alla curva 
- Applicando questa definizione all’equazione che descrive Q(t) si ha
![[Pasted image 20241119123833.png]]

Continuità parametrica 
- $C^0$ se le due curve **coincidono** in un punto (di giunzione) 
- $C^1$ se i due vettori tangenti alla curva **sono uguali** (**direzione** e **modulo**) nel punto di giunzione 
- $C^2$ se la derivata seconda **è uguale** in quel punto

Continuità geometrica 
- $G^0$ se le due curve **coincidono** in un punto (di giunzione) 
- $G^1$ se i due vettori tangenti alla curva sono **proporzionali** nel punto di giunzione (hanno la stessa direzione, ma non necessariamente lo stesso modulo) 
	- Pendenza uguale, uno multiplo dell’altro (TV1=k·TV2, k>0) 
	- Spesso richiesto, es. nel CAD
- $G^2$ se la derivata seconda è **proporzionale** in quel punto

La continuità $C^1$ implica $G^1$ , ma il viceversa non è solitamente vero
La continuità parametrica di ordine n implica continuità geometrica di ordine n, ma non viceversa

Diverso rappresentare/disegnare una curva parametrica rispetto ad una funzione

- Funzione: ad esempio, la variabile indipendente è mostrata sull’asse x, la variabile dipendente sull’asse y
- Curva parametrica: t non viene rappresentato - Non si può quindi determinare, guardando la curva, il modulo del vettore tangente (solo la direzione) 
	- Per ottenere una giunzione omogenea tra due segmenti di curva è sufficiente che la direzione dei due vettori tangenti sia coerente (continuità geometrica G1) 
	- Il modulo del vettore tangente indica l’entità della tensione della curva nella direzione del vettore (prima della curvatura)

Un segmento di curva $Q(t)$ è definito dai vincoli sugli estremi, sui vettori tangenti e sulla continuità tra segmenti diversi 
- **Cubiche**, polinomi con quattro coefficienti, quindi quattro vincoli, quattro equazioni in quattro incognite
- Tre principali tipi di curve, diverse rappresentazioni
	- **Hermite**: definite dai due estremi (le curve passano per quei punti, li interpolano) e dai due vettori tangenti negli estremi
	- **Bézier**: definite dai due estremi e da due altri punti che controllano i vettori tangenti negli estremi
	- Alcuni tipi di **spline**: definite da quattro punti di controllo, con continuità C1 e C2 nei punti di giunzione, passano vicino ai punti di controllo, ma in genere non li interpolano (B-spline uniformi e non uniformi)

Per verificare la dipendenza da quattro vincoli, considerata l’espressione $Q(t)=C\cdot T$ è possibile riscrivere la matrice di coefficienti $C$ come $C=G\cdot M$ 
- M matrice di base 4×4 ¨ 
- G matrice geometrica, quattro elementi che rappresentano i vincoli geometrici, le condizioni che definiscono la curva (con Gx ci si riferisce al vettore riga dei componenti x, ecc.)

Gli elementi di G ed M sono costanti, $Q(t)=G \cdot M \cdot T$ tre polinomi di terzo grado in t
![[Pasted image 20241119133248.png]]

Un altro modo di leggere $Q(t)=G\cdot M\cdot T$
- La curva è la **somma pesata** delle quattro colonne della matrice geometrica G (due colonne per un segmento lineare), ognuna delle quali rappresenta un punto o un vettore nello spazio a tre dimensioni 
- Calcolando $x(t)=G_x \cdot M \cdot T$
![[Pasted image 20241119134342.png]]
- I pesi B sono polinomi in t chiamati funzioni di blending e sono definiti come $B=M \cdot T$

### Hermite
Forma di Hermite del segmento di curva cubica polinomiale
- Definita dai vincoli sugli estremi P1 e P4 e dai vettori tangenti negli estremi R1 ed R4
- Per trovare la matrice di base di Hermite $M_H$ che lega il vettore geometrico di Hermite $G_H$ ai coefficienti 
- polinomiali si possono scrivere quattro equazioni (una per ciascun vincolo) nei quattro coefficienti incogniti e risolvere poi nei coefficienti incogniti stessi 
- Si definisce la componente x della matrice geometrica di Hermite come $G_{Hx}$=\[$P_{1x}$ $P_{4x}$ $R_{1x}$ $R_{4x}$]
- Si può riscrivere $x(t)$ come
$$
x(t)=C_X·T=G_{Hx}·M_H·T=G_{Hx}·M_H·[t^3 \ t^2 \ t \ 1]^T 
$$
- I vincoli in x(0) e x(1) si trovano per sostituzione diretta
$$
x(0)=P_{1x} =G_{Hx}·M_H·[0 \ 0 \ 0 \ 1]^T
$$
$$
x(1)=P_{4x} =G_{Hx}·M_H·[1 \ 1 \ 1 \ 1]^T
$$
- Come per il caso generale, si ottengono $x’(0)$ e $x’(1)$ calcolando la derivata $x’(t)$ di $x(t)$ in 0 e 1
- Le equazioni per i vincoli sui vettori tangenti diventano 
$$
x’(0)=R_{1x} =G_{Hx}·M_H·[0 \ 0 \ 1 \ 0]^T 
$$
$$
x’(1)=R_{4x} =G_{Hx}·M_H·[3 \ 2 \ 1 \ 0]^T 
$$
- I quattro vincoli possono essere riscritti in forma matriciale come
![[Pasted image 20241124170555.png]]

- Perché questa equazione (e quelle in y e z) sia soddisfatta, $M_H$ deve essere l’inversa della matrice 4×4 sopra riportata
![[Pasted image 20241124170620.png]]

La matrice $M_H$ può quindi essere utilizzata per: 
- Trovare $x(t)$ dato il vettore $G_{Hx}$, considerato che $x(t)=G_{Hx}·M_H·T$ (e analogamente per y e z )
- Quindi $Q(t)=[x(t) y(t) z(t)]T=G_H·M_H·T$ dove $G_H$ è la matrice $[P_1 P_4 R_1 R_4]$

Espandendo il prodotto $M_H ·T$ in $Q(t)=G_H·M_H·T$ si ottengono le funzioni di blending di Hermite $B_H$
$Q(t)=G_H·M_H·T = G_H·B_H = +(2t^3-3t^2+1)P_1+(-2t^3+3t^2)P_4+ +(t^3-2t^2+t)R_1+(t^3-t^2)R_4$![[Pasted image 20241124171907.png]]

![[Pasted image 20241124172155.png]]

Come altre curve parametriche
- Semplici da disegnare
- Si valuta l’equazione della curva per $n$ valori successivi di $t$ distanziati di una quantità / un passo δ
### Bezier

Forma di Bézier dei segmenti di curva polinomiali. In genere, segmenti cubici, 4 punti di controllo 
- Si specificano indirettamente i vettori tangenti negli estremi mediante **due punti intermedi** che non si trovano sulla curva 
- In particolare, i vettori tangenti $R_1$ ed $R_4$ rispettivamente nell’estremo iniziale $P_1$ e finale $P_4$ sono determinati dai vettori $P_1P_2$ e $P_3P_4$ e sono legati ad $R_1$ ed $R_4$ dalle relazioni 
	$R_1 = Q’(0) = 3(P_2-P_1)$ 
	$R_4 = Q’(1) = 3(P_4-P_3)$ 
Come per Hermite, la curva interpola i due punti di controllo estremi ed approssima gli altri due

Matrice geometrica di Bézier : $G_B$ --> consiste di quattro punti, $G_B$=\[$P_1$ $P_2$ $P_3$ $P_4$] 
Matrice $M_{HB}$ che definisce la relazione $G_H=G_B·M_{HB}$  tra la matrice geometrica di Hermite $G_H$ e la matrice geometrica di Bézier $G_B$
![[Pasted image 20241124174645.png]]

Per trovare la matrice di base di Bézier $M_B$ si utilizza l’espressione $Q(t)=G_H·M_H·T$ per la forma di Hermite, si sostituisce $G_H=G_B·M_{HB}$ e si definisce $M_B=M_{HB}·M_H$
$$
Q(t)=G_H·M_H·T=(G_B·M_{HB})·M_H·T=G_B·(M_{HB}·M_H)·T=G_B·M_B·T
$$
Il prodotto $M_B=M_{HB}·M_H$ fornisce
![[Pasted image 20241124175345.png]]
Il prodotto $Q(t)=G_B·M_B·T$ è invece
$$
Q(t)=(1-t)^3P_1+3t(1-t)^2P_2+3t^2(1-t)P_3+t^3P_4
$$
I quattro polinomi $B_B=M_B·T$ (i pesi nell’equazione) sono chiamati polinomi (basi) di Bernstein, definiti a partire dal grado $n=k-1$ (ad esempio, n=3, k=4)
![[Pasted image 20241124175533.png]]
Con Bézier, per ogni valore di t, funzioni di blending non negative e somma sempre 1

Come risultato, Q(t) è la media pesata dei punti di controllo e si trova sempre all’interno della chiusura convessa (convex hull) dei punti di controllo
- Minimo poligono convesso che li raccorda tutti come vertici
- Ottimizzazioni: clipping, un polinomio come 1 - somma

Come rappresentare forme più complesse? 
- Aument. il grado si ottengono curve difficili da controllare
Alternativa: 
- Combinazione di segmenti di curva, ciascuno definito su intervalli diversi del parametro t 

Per garantire l’aspetto voluto occorre agire nei punti di giunzione

Per unire due segmenti di curva (ottenere continuità) servono delle condizioni
- Per $G^0$ e $C^0$ occorre imporre il passaggio per il punto comune di giunzione: l’estremo finale di un segmento deve coincid. con quello iniziale del segmento successivo 
- Per $G^1$ ($C^1$) occorre imporre che il vettore tangente nell’estremo finale di un segmento sia allineato (sia identico) al vettore tang. nell’estremo iniziale dell’altro 
- Per $G^2$ o $C^2$ occorre imporre vincoli anche sulle derivate seconde nel punto di giunzione

Quindi, chiamando $x^l$ ed $x^r$ i segmenti sinistro (left) e destro (right)
- Si possono trovare le condizioni per la continuità C0 e C1 nel punto di giunzione come
![[Pasted image 20241124180550.png]]
- Per x (analogamente per y e z) si ha
![[Pasted image 20241124180600.png]]
- E, come atteso, continuità quando P4-P3=P5-P4

Svantaggi
- Un punto di controllo non influenza solo il suo intorno, ma l’intera curva (le funzioni di blending sono non nulle su tutto l’intervallo di controllo)
- Relazione tra grado della curva e punti di controllo: per approssimare $n$ punti di controllo si è obbligati ad usare più segmenti o ad aumentare il grado della curva
- Per ottenere continuità $G^2$ ($C^2$) occorre imporre pesanti restrizioni sulla forma della curva
## Spline

Termine utilizzato un tempo per indicare dei fogli di metallo utilizzati nella costruzione di aerei, automobili, navi, ecc.
- Piegati applicando opportuni pesi alle estremità
- Se non sottoposti a stress eccessivo, continuità $C^2$ 

Equivalente matematico
- Spline cubiche naturali, curve polinomiali $C^0$, $C^1$ e $C^2$ che interpolano (passano attraverso) determinati punti di controllo 
	- Un grado di continuità in più rispetto ad Hermite e Bézier, quindi implicitamente più smussate 
	- Ma, polinomi dipendenti da tutti gli $n$ punti di controllo: lo spostamento di un punto di controllo influenza l’intera curva (manipolazione potenzialmente inefficiente)
### B-Spline Uniformi Non Razionali

Risolvono alcuni svantaggi di Bèzier 
- Segmenti di curva i cui coefficienti polinomiali dipendono da meno punti di controllo 
	- Comportamento chiamato controllo locale : Riduzione dei tempi di calcolo, stessa continuità delle spline naturali, ma i punti di controllo non vengono interpolati (bensì approssimati) 
- Curva intesa come unione di segmenti 
	- Un segmento non ha bisogno di passare per i punti di controllo
	- Le condizioni di continuità su un segmento derivano dai segmenti adiacenti 
		- Punti di controllo condivisi tra segmenti

Curve cubiche B-spline (ma esistono di ogni grado) 
- Approssimano una serie di m+1 punti di controllo P0, P1, …, Pm (m ≥3) con una curva composta da m-2 segmenti di cubiche polinomiali Q3, Q4, …, Qm 
- Ciascuna curva potrebbe essere definita sul dominio 0≤t<1 
- Ma si può sostituire il parametro $t$ con $t = t+k$ in modo che i domini per i vari segmenti siano consecutivi 
- In questo modo, per un segmento Qi si ha ti ≤t (3≤i≤m) 
- Per il caso particolare m=3 si ha un solo segmento Q3 definito nell’intervallo t3 ≤t<t4 da quattro punti di controllo P0, …, P3
- Per ogni i ≥4, in ti si ha un punto di giunzione (o nodo, knot) tra Qi -1 e Qi (valore del parametro chiamato valore del nodo), anche i punti iniziale e finale $t_3$ e $t_{m+1}$ sono chiamati nodi 
	- Quindi $m-1$ nodi 
	- Per chiudere la curva basta ripetere gli ultimi 3 punti di controllo
![[Pasted image 20241124185539.png]]


- Spline **uniformi** : Nodi ad intervalli regolari in t, ad esempio $t3=0$, intervallo $t_i+1-t_i =1$, successivamente, spline non uniformi
- Spline **non razionali** : Diverse dalle curve polinomiali razionali per le quali x(t), y(t) e z(t) sono definiti ognuno come il rapporto di due polinomi
- **B** sta per **base** : ad indicare che le spline possono essere rappresentate come somma pesata di funzioni polinomiali di base. Diversamente dalle spline naturali, per le quali questa proprietà non è verificata

Ognuno degli $m-2$ segmenti di curva di una B-spline è definito da quattro degli $m+1$ punti di controllo (es., Qi definito da $P_{i-3}$, $P_{i-2}$, …, $P_i$ )
Quindi, la matrice geometrica B-spline $G_{Bsi}$ per $Q_i$ è $G_{Bsi}$ = \[$P_{i-3}$, $P_{i-2}$, $P_{i-1}$, $P_i$ ], $3≤i≤m$ 
- Primo segmento di curva $Q_3$ definito dai punti $P_0$ fino a $P_3$ sull’intervallo da $t_3=0$ fino a $t_4=1$
- $Q_4$ definito dai punti $P_1$ fino a $P_4$ sull’intervallo da $t_4=1$ fino a $t_5=2$
- Ultimo segmento $Q_m$ definito dai punti $P_{m-3}$ fino a $P_m$ sull’intervallo da $t_m =m-3$ fino a $t_{m+1}=m-2$ 

In generale : $Q_i$ comincia vicino a $P_{i-2}$ e finisce vicino a $P_{i-1}$

Così come ogni segmento di curva è definito da quattro punti di controllo 
- Ogni punto di controllo (ad eccezione di quelli all’inizio ed alla fine della sequenza P0, P1, …, Pm ) influenza quattro segmenti di curva 
- Spostando un punto di controllo in una certa direzione si spostano i quattro segmenti influenzati in quella stessa direzione 
- Gli altri segmenti di curva non sono influenzati (“controllo locale”, proprietà anche dei tipi di spline)

![[Pasted image 20241124191348.png]]

Definito il vettore colonna $T_i =[(t -t_i)^3 \ (t -t_i)^2 \ (t-t_i) \ 1]^T$ 
- La formulazione della spline per il segmento i diventa $Qi (t)=G_{Bsi}·M_{Bs}·T_i$ , $ti ≤t_{i+1}$
- Intera curva generata applicando l’equazione per $3 ≤ i ≤ m$

La matrice di base B-spline $M_{Bs}$ mette in relazione i vincoli geometrici $G_{Bs}$ con le funzioni di blending ed i coefficienti polinomiali
![[Pasted image 20241124213957.png]]

Funzioni di blending B-spline $B_{Bs}$ date dal prodotto $M_{Bs}·T_i$ (come per Bézier ed Hermite)  
- Sono le stesse per ogni segmento di curva, perchè per ogni segmento i, i valori di $t-t_i$ variano tra 0 per $t=t_i$ ad 1 per $t=t_i+1$
- Sostituendo t -ti con t e l’intervallo \[ti , ti+1] con \[0,1]
![[Pasted image 20241124222440.png]]
![[Pasted image 20241124222448.png]]

Espandendo l'equazione per $Q_i(t)$ sostituendo $t-t_i$ con t si può dimostrare che $Q_i$ e $Q_i+1$ sono $C^0$, $C^1$ ed anche $C^2$ continue nel punto di giunzione (al costo di un minore controllo su quale sarà la forma della curva)

Si può fare in modo che la curva interpoli punti specifici replicando i punti di controllo 
Ad esempio, se $P_{i-2}=P_{i-1}$ la curva è tirata più vicino a questo punto perché il segmento $Q_i$ è definito solo da tre punti diversi e $P_{i-2}=P_{i-1}$ è pesato due volte
Se un punto di controllo è usato tre volte, ad esempio se $P_{i-2}=P_{i-1}=P_i$ allora $Q_i(t) = P_{i-3}\times B_{Bs_{-3}}+P_i\times (B_{Bs_{-2}}+B_{Bs_{-1}}+B_{Bs_{0}})$ diventa una retta
![[Pasted image 20241124223352.png]]

### Non Uniforme
Intervallo tra i nodi non necessariamente uniforme 
- Le funzioni di blending non sono più le stesse per ogni intervallo, ma cambiano da un segmento all’altro

Vantaggi 
- La continuità nel punto di giunzione può essere ridotta da $C^2$ a $C^1$ a $C^0$, o rimossa del tutto 
- Se la continuità è ridotta a $C^0$, allora la curva interpola un punto di controllo, ma senza l’effetto spiacevole introdotto nelle B-spline uniformi dove i due segmenti di curva diventano rette
- Semplice aggiungere un nuovo nodo ed un nuovo punto di controllo per dare una nuova forma alla curva
### Cubiche Polinomiali Razionali

Segmenti di curva razionali, espressi come rapporti di polinomi
$x(t)=X(t)/W(t) \quad  y(t)=Y(t)/W(t) \quad z(t)=Z(t)/W(t)$

X(t), Y(t), Z(t), W(t) curve polin. cubiche i cui punti di controllo sono definiti in coordinate omogenee 
- Per passare in uno spazio a tre dimensioni occorre, come di consueto, dividere per W(t)
- Si può convertire una qualunque curva non razionale in una curva razionale aggiung. W (t)=1 come quarta coordinata
- La quarta coord. determina la “forza” del punto di controllo  

I polinomi 
- Possono essere di Bézier, Hermite, ecc. 
- Nel caso di B-spline le curve sono definite NURBS

Curve razionali (es. NURBS), utili per due motivi :
- Invarianti rispetto a rotazione, scalamento, traslazione e trasformazioni prospettiche dei punti di controllo (le curve non razionali solo rispetto alle prime tre trasformazioni) 
	- È sufficiente quindi applicare le trasformazioni ai punti di controllo (l’alternativa è quella di convertire la curva nei punti corrispondenti ed applicare le trasformazioni ai punti, molto meno efficiente) 
- Possono rappresentare correttamente ogni tipo di sezione conica (non possibile, esempio, con Bézier, solo appross.) 
	- Una curva non razionale può solo approssimare una conica, utilizzando un numero sufficiente di punti di controllo vicino alla conica 
- Maggior flessibilità: un punto di controllo in coordinate omogenee può attrarre in modo continuo la curva

## Cubiche a Confronto
![[Pasted image 20241124224445.png]]
Non è necessario scegliere una rappresentazione : Si può passare dall’una all’altra, es. manipolazione con Bézier e B-spline uniformi, rappresentazione interna basata su B-spline razionali non uniformi (più generale)

# Superfici

## Superfici parametriche bi-cubiche

Generalizzazione delle curve cubiche 
- Forma generale della curva $Q(t)=G·M·T$ , con G costante 
- Per avere una notazione più conveniente 
	- Si sostituisce $t$ con $s$, ottenendo $Q (s )=G·M·S$ 
- Se si consente ai punti in G di variare in 3D lungo un qualche percorso parametrizzato in $t$ allora $Q(s,t)=[G1(t) \ G2(t) \ G3(t) \ G4(t)]·M·S$ 
	- Per un valore fisso $t_1$ $Q(s,t)$ è una curva, $G (t1)$ è costante
	- Con un $t$ diverso, es. $t_2$, con $t_2- t_1$ molto piccolo, si ottiene una curva leggermente diversa
	- Ripetendo il processo per un numero arbitrario valori di $t_2$ tra 0 e 1 si ottiene un’intera famiglia di curve
	- Op. nota anche come prodotto tensoriale

Superficie definita da un insieme di curve 
- Se le curve sono cubiche, la superficie è detta bi-cubica
- Ogni curva può essere rappresentata come $G_i(t)=G_i·M·T$, con $G_i$ =\[$g_{i1} \ g_{i2} \ g_{i3} \ g_{i4}$] ($g_{i1}$ primo elemento della matrice geometrica per la curva $G_i(t)$ e così via)
- Trasponendo l’equazione sfruttando $(A·B·C)^T= A^T·B^T·C^T$ si ha $G_i(t)=T^T ·M^T · G_i^T = T^T ·M^T · [g_{i1} \ g_{i2} \ g_{i3} \ g_{i4}]^T$
- Sostituendo in $Q(s,t)=[G1(t) \ G2(t) \ G3(t) \ G4(t)]·M·S$ si ottiene la forma generale per x, y e z
![[Pasted image 20241124225412.png]]
![[Pasted image 20241124225514.png]]
## Superfici di Bèzier
![[Pasted image 20241124225531.png]]
![[Pasted image 20241124225542.png]]

Pro:
- Molto facili da disegnare, basta combinare ogni segmento di superficie in una griglia regolare
Contro: 
- Non è detto però che si riesca sempre ad ottenere la continuità voluta
- Inoltre, troppo semplici per descrivere forme complesse (connettività troppo regolare)
## Superfici NURBS
Pro:
- Permettono di rappresentare coniche in maniera esatta
- Facile ottenere alti livelli di continuità
Contro:
- Difficile collegare i segmenti di superficie tra loro 
- Difficili da manipolare (molti gradi di libertà)
![[Pasted image 20241124225646.png]]

## Tecniche di suddivisione

Rappres. esplicite, con un approccio (un punto di partenza) diverso per descrivere una curva 
- Si inizia con una curva “di controllo” 
- La si suddivide ripetutamente, usando come nuove posizioni la media pesata delle posizioni di partenza 
- Scelta attenta della funzione utilizzata per calcolare la media (talvolta si può ottenere la stessa curva che si otterrebbe con le tecniche precedenti)
![[Pasted image 20241124225949.png]]

Analogamente, per una superficie si parte da una “gabbia” (superficie) di controllo, e si aggiornano i vertici andando a determ. delle pos. medie 
- Approcci diversi : Catmull-Clark (quadrilateri), Loop (triangoli) 
- Problematiche comuni: Interpolazione o approssimazione, Continuità
- Più facili da manipolare delle precedenti, più difficili da valutare punto per punto 
- Largam. utilizzate (Academy Awards, 2019)
![[Pasted image 20241124230053.png]]

## Varietà (Manyfold) e Mesh Poligonali
Tipicamente, assunzione : Forme (superfici) che siano varietà di dimensione 2
In termini semplificati, in geometria una superficie è una varietà se, avvicinandosi molto, è possibile disegnare su di essa una griglia piana

Non tutte le superfici sono delle varietà:
![[Pasted image 20241124231110.png]]

Per una mesh poligonale, perché si tratti di una varietà sono sufficienti due condizioni 
- Ogni lato è contenuto in al più due poligoni (ovvero non ci sono “sporgenze”)
- Per ogni vertice, i poligoni che lo contengono formano un solo anello (un solo “ventaglio”)

Nei punti “di confine” (dove la superficie termina) 
- Localmente, semicerchio 
- Globalmente, ogni confine forma un anello di poligoni
- Per mesh poligonali
	- Per ogni lato sul confine, un solo poligono, forma indicata

Le griglie regolari sono semplici/efficienti 
- Un pixel ha sempre un numero fisso di pixel vicini
- Facile indicizzare i pixel, applicare filtri, ecc.
- Si possono memorizzare come una semplice sequenza di valori 

Anche se non sono sempre, necessariamente la codifica migliore possible, per una qualunque immagine bitmap. Sono sempre una buona alternativa (generalità della rappresentazione)

Analogamente, per una mesh poligonale è vantaggioso considerare solo varietà bidimensionali. Assunzioni sulla geometria dell’oggetto da rappresentare che possono rendere le strutture dati e gli algoritmi che lavorano su di essa semplici/efficienti
Nella maggior parte dei casi, tali assunzioni non si traducono in una limitazione alle forme rappresentabili

Rappresentare Mesh Poligonali:
- Collezioni di vertici, lati e poligoni
- Diverse rappresentazioni possibili --> Anche nell’ambito della stessa applicazione 
	- Per archiviazione su memoria di massa 
	- Per uso interno 
	- Per la manipolazione/l’interazione 
- Scelta della rappresentazione più adeguata 
	- Compito dello sviluppatore

Operazioni comuni:
- Trovare tutti i lati incidenti su di un vertice
- Trovare i poligoni che condividono un lato o un vertice
- Trovare i vertici uniti da un lato
- Trovare i lati di un poligono
- Visualizzare la mesh
- Individuare errori nella rappr. (es. lato mancante, ecc.)

Due criteri di valutazione: spazio e tempo

In genere: Più le relazioni (informazioni di connettività, “vicinanza”) tra vertici, lati e poligoni sono rappresentate in modo esplicito, minore è il tempo di elaborazione, ma maggiore è lo spazio necessario per la memorizzazione

Strutture dati per rappresentare valori numerici
- Array: tempo di accesso cost., aree di memoria contigue 
- Lista likata: tempo di accesso lineare, aree di memoria non contigue - Vantaggiosa però, ad esempio, per inserimenti/eliminazioni
![[Pasted image 20241124231709.png]]

Approccio più semplice: Memoriz., per ogni triangolo, **solamente tre coordinate** - Nessun’altra informazione di connettività (come una nuvola di punti, “nuvola di triangoli”)
Pro: 
- Decisamente semplice 
Contro: 
- Ridondanza 
- Difficile eseguire op. più complesse del disegnare triangoli (serviranno altre strutture dati, es., per individuare el. vicini)
#### Elenco di Adiacenze
Altro approccio: **Elenco di adiacenze**
Vertici mem. come triplette di coordinate (x,y,z) - Triangoli (polig.) mem. come tripl. (tuple) di indici 
Pro : Alcune operazioni semplici (es., trovare tutti i triangoli che condividono un determinato vertice) 
Contro: Complessità elevata per geometrie composte da un numero elev. di triangoli
#### Matrice di incidenze
Se si ha la necessità di disporre di informazioni di “vicinanza” perchè non memorizzarle? ¨ Es. usando **matrici di incidenza** 
- 1 “coinvolto”, 0 “non coinvolto” 
- Per evitare di memorizzare una quantità elevata di valori a 0, si possono usare matrici sparse
Pro : Individ. vicini ha comples. costante, Non serve assumere siano varietà
Contro : Costo di mem. comunque elevato, difficile cambiare la connettività
#### Half-Edge
Alternativa, approccio “di tipo” lista likata, diversamente dai precedenti, “di tipo” array 
- Memorizzando alcune informazioni di vicinanza 
- Non come un elenco di tutte le adiacenze, ma tramite puntatori
Attraverso l’uso di una struttura dati chiamata **half-edge** (letteralmente, “mezzo-lato”)
Idea base : Half-edge come un meccanismo per “incollare” (collegare) i triangoli tra loro
Con due mezzi-lati si collegano due triangoli
Ogni vertice, lato e faccia possiede un puntatore ad un solo half-edge
Vincolo: la rappresentazione half-edge assume che la mesh sia una varietà, per costruzione, può rappresentare solo varietà
L’uso di questa rappresentazione rende però facile l’attravers. della mesh (usando le informazioni di vicinanza), usando Puntatori “next” e “twin” per muoversi sulla mesh
La rappr. half-edge semplifica anche le operazioni di manipolazione (punt. “vertex”, “edge” e “face”) 
Ad esempio, split di lati, collassamento di lati, ecc.
Vantaggi della lista linkata: inserimento o rimozione di elementi mediante aggiornamento di puntatori

Le condizioni da verificare perchè una mesh poligonale sia una varietà 
- Non dicono nulla riguardo la posizione dei vertici (solo connettività)
- Le superfici potrebbero quindi essere molto irregolari, nonostante siano delle varietà

![[Pasted image 20241124232240.png]]
## Elaborazione di geometrie
Possibile generalizzare le canoniche tecniche per l’elaborazione dei segnali a “segnali geometrici” (in particolare, rappresentati come mesh poligonali) 
- Operazioni tipiche: ricostruzione, filtraggio, sovra-/sottocampionamento, ricampionamento, compressione, ecc.
- Operazioni essenziali in molte aree/molti algoritmi dell’informatica grafica (animazione, rendering, ecc.)
![[Pasted image 20241124232836.png]]
### Ricostruzione

Dato un insieme di campioni della geometria, ricostruzione della superficie 
Campioni rappresentati come: 
- Punti, punti e normali, ecc.
- Coppie/insieme di immagini (es. stereo, multi-vista)
- Integrali di linea di funzioni scalari (es. densità dei tessuti ottenuta mediante tecniche di diagnostica per immagini)
![[Pasted image 20241124233016.png]]
### Filtraggio
Rimozione di rumore o enfatizzazione di caratteristiche importanti 
Es., spigoli, anche in maniera esasperata
![[Pasted image 20241124233035.png]]
### Sovra-/sotto-campionamento
Aumento/Riduzione della risoluzione (numero di campioni) della geometria (preservandone la forma/l’aspetto) 
- Es., attraverso suddivisione o decimazione iterativa (es. collassamento di lati)
- Come per altri segnali 
	- Il sovra-campionamento ha un impatto sulle prestazioni 
	- Il sotto-campionamento porta/può portare alla perdita di informazioni
![[Pasted image 20241124233118.png]]

Possibile effetto dell’applicazione di operazioni di sovra-/sotto-campionamento ¨ Degrado del segnale
![[Pasted image 20241124233215.png]]
### Ricampionamento
Modifica della distribuzione dei campioni per migliorare la “qualità” 
Diverse interpretazioni del concetto di qualità per una mesh poligonale, in base all’obiettivo (es., visualizzazione, calcolo di funzioni, ecc.)
![[Pasted image 20241124233255.png]]
### Qualità di una mesh poligonale
Mesh “di qualità” se fornisce una buona approssimazione della forma originale 
- Presenti/mantenuti gli elementi che contribuiscono a veicolare l’informazione di forma, in numero maggiore, es., dove la curvatura è elevata 
- Purtroppo, la sola posizione dei vertici non è sufficiente - Es., normali?

Altra regola intuitiva (per mesh triangolari) : Tutti gli angoli di circa 60°

Delaunay: circonf. passanti per i tre vertici del triangolo vuote (nessun vertice di altri triangoli) 
- Massimizza l’angolo minimo, produce l’interpol. migliore, ecc. 
- Più sofisticata, non implica comunque la precedente

Compromessi : Buona “efficienza” o buona approssimazione geometrica

grado dei vertici “regolare” 
- Grado 6 per mesh composte da triangoli 
- Grado 4 per mesh composte da quadrilateri
![[Pasted image 20241124233437.png]]

Regolarità dei vertici : Si traduce in forme più regolari, calcoli più regolari, suddivisioni più regolari, ecc.

Come migliorare la qualità (senza variare il numero di triangoli)?
Es., come rendere la mesh più Delaunay? 
- Se α+β > π, ribalt. il lato 

Come migliorare il grado dei vertici? 
- Se ciò consente di ridurre complessivamente la deviazione dalla regolarità di i, j, k ed l, ribaltare il lato 

Come migl. gli angoli? 
- Ricentrare i vert. (media)

![[Pasted image 20241124233533.png]]

Es. ricampionamento isotropico
- Spezzare ogni lato la cui lunghezza supera i 4/3 della media delle lunghezze 
- Collassare ogni lato la cui lunghezza è inf. ai 4/5 della media delle lunghezze
- Ribaltare i lati per migliorare la regolarità dei vertici
- Ricentrare i vertici (tangenzialmente)