Come calcolare il colore risultante di una determinata proporzione di una superficie di un oggetto / scena 3D
- Modelli di illuminazione (lighting) --> riguarda pixel
- Modelli di ombreggiatura (shading) --> riguarda geometria

Approssimazioni per determinare la visibilità di una superficie --> basi teoriche, prestazioni efficaci e pratiche
Parte della luce viene rifratta, altra viene assorbita, e altra viene riflessa

Modelli di illuminazione Globali: cercano di considerare scambio di luce tra tutte le superfici della scena

Modelli di illuminazione Locali: considerano solo la luce che arriva direttamente dalla sorgente luminosa. Molto efficienti, ma possono mancare diversi aspetti.

## Equazione di Rendering

$$
L_o = L_e(p,\omega)+\int_\Omega k_r(p,\omega_i\rightarrow \omega_o)L_i(p,\omega_i)\cos\theta d\omega_i
$$
La luce (radianza) osservata (Lo) è la somma della luce emessa (Le) e della luce riflessa: la luce riflessa è ottenuta moltiplicando la luce che arriva da tutte le direzioni (Li), per il coefficiente di riflessione (kr), e per il coseno tra l’angolo di incidenza e la normale alla superficie nel punto

Riflessione : processo per cui luce incidente interagisce con superficie lasciando la superficie dalla stessa parte da cui è arrivata, senza cambiare frequenza
Il coefficiente di riflessione $k_r(p,\omega_i\rightarrow \omega_o)$ “codifica” l'interazione della luce con una superficie

Viene definito attraverso il BRDF (Bidirectional Reflectance Distribution Function)
- Riflessione speculare ideale : specchio perfetto
- Riflessione diffusa ideale : riflessione uniforme in tutte le direzioni
- Riflessione speculare non-ideale : riflessione distribuita principalmente nell'intorno della direzione di riflessione
- Retro-riflessione : riflessione verso la sorgente
### Luce d'Ambiente
Diverse semplificazioni:
- oggetto visualizzato nella sua interezza con un suo unico valore di intensità luminosa
- Senza sorgenti di luce esterne
- equazione di illuminazione

più comunemente, invece che considerare luce emessa dagli oggetti, si assume l'esistenza di una sorgente di luce diffusa.

Equazione di illuminazione diventa: $I = I_ak_a$  (coefficiente di riflessione ambientale $k_a$)
### Riflessione Diffusa 
Sorgente di luce puntiforme (emissione uniforme in ogni direzione a partire da un singolo punto)

Riflessione diffusa ideale (Lambertiana) : luminosità uniforme indipendentemente dall'angolo di vista. "Colore" dell'oggetto. Riflessione luce con uguale intensità in tutte le direzioni.

Luminosità uniforme determinata da 2 fattori: 
- superfici lambertiane : quantità di luce riflessa da dA verso osservatore è direttamente prop. a $\cos\theta$
- raggio di luce che incide su dA' copre un area la cui dimensione è inv. prop. a $\cos\theta$

Indipendente dalla direzione di vista, è proporzionale solo a $\cos\theta$ (angolo di incidenza)

$$
I = I_p k_d\cos\theta \longrightarrow I = I_p k_d(\bar{N}\cdot \bar{L})
$$

Se la sorgente di luce puntiforme è sufficientemente distante dagli oggetti illuminati, l'angolo della luce incidente è lo stesso rispetto a tutte le superfici: **luce direzionale**

Per ottenere un equazione di illuminazione più realistica, si aggiunge la componente di luce d'ambiente
$$
I = I_a k_a + I_p k_d(\bar{N}\cdot \bar{L})
$$
Attenuazione della sorgente di luce:
$$
I = I_a k_a + f_{att} I_p k_d(\bar{N}\cdot \bar{L})
$$
Scelta di $f_{att}$ : $f_{att} = \min(\frac{1}{c_1 + c_2d_L + c_3d_L^2}, 1)$  (tiene conto dell'attenuazione atmosferica)

Luce colorata: aggiunta di RGB all'equazione

$$
I_R = I_{aR}k_aO_P{dR}
$$


Attenuazione atmosferica: tra oggetto ed osservatore, si usano indizi di profondità, 2 piani: front e back. Dato lo $z_o$ dell'oggetto, viene derivato un fattore di scala $s_o$ da utilizzarsi per interpolare $I_\lambda$ e $I_{dc\lambda}$ 
$$
I'_\lambda = s_o I_\lambda + (1-s_o)I_{dc\lambda}
$$
### Riflessione Speculare
osservata su qualunque superficie lucida --> alone (highlight)
se cambia il POV si sposta anche l'alone

oggetti riflettenti non perfetti -> modello di illuminazione di phong. 
Approssimazione basata su riflessione diffusa, speculare e luce ambiente.
Assume che la riflessione sia massima quando $\alpha = 0$ e decresca rapidamente al crescere di $\alpha$
![[Pasted image 20241105121328.png|300]]

Modello di illuminazione di Phong:
$$
I_\lambda = I_{a\lambda}k_aO_{d\lambda} + f_{att}I_{p\lambda}[k_dO_{d\lambda}(\bar N \cdot \bar L) + k_sO_{s\lambda}(\bar R \cdot \bar V)^n]
$$
### Sorgenti multiple
con m sorgenti di luce:
$$
I_\lambda = I_{a\lambda}k_aO_{d\lambda} + \sum_{1\le i \le m} f_{att_i}I_{p\lambda_i}[k_dO_{d\lambda}(\bar N \cdot \bar L_i) + k_sO_{s\lambda}(\bar R_i \cdot \bar V)^n]
$$
### Ombreggiatura
è sempre possibile calcolare l'ombreggiatura di una superficie calcolando la normale per ogni punto visibile e applicando un modello di illuminazione al punto.

- Ombreggiatura costante ( constant shading ), anche noto come faceted shading o flat shading : applicare modello di illuminazione una sola volta per determinare un unico valore di intensità che sarà usato per ombreggiare l'intero poligono
- Ombreggiatura interpolata ( interpolated shading) : info di ombreggiatura interpolate linearmente lungo un triangolo a partire dai valori dei vertici. approccio incrementale
  ![[Pasted image 20241105133629.png]]

coordinate baricentriche : possono essere usate per interpolare qualunque attributo associato ai vertici. 
Le 3 funzioni sono ottenute nella fase di verifica dei 3 semipiani nella rasterizzazione --> no ricalcolo

Servono modelli per geometrie poligonali che sfruttino le info relative ai poligoni adiacenti per simulare una superficie smooth:
- Gouraud shading : interpolazione di intensità o di colore, obiettivo: eliminare discontinuità di intensità/colore.
  Tende a disperdere l'alone e a non considerare del tutto aloni che non cadono ai vertici
- Phong shading : interpolazione del vettore normale, per ogni pixel lungo la scan line

Problemi:
- contorno poligonale
- distorsione prospettica
- dipendenza dall'orientamento
- problemi nei vertici comuni
- normali ai vertici non rappresentative

# Textures

immagine (texture map) digitalizzata o sintetizzata composta di texel ( mappa rettangolare con suo spazio di coordinate (u,v) )
oppure definita con una procedura : texture funzione 3D della posizione dell'oggetto.

Coordinate di Texture: definiscono mapping da coordinate della superficie a punti nello spazio della texture, spesso definite interpolando linearmente le coordinate di texture associate ai vertici dei triangoli

Algoritmo:
```
Per ogni pixel nell’immagine rasterizzata 
	Interpola le coordinate (u,v) sul triangolo 
	Campiona la texture in (u,v) 
	Utilizza il valore per colorare il pixel
```

A causa della proiezione, i pixel nello spazio dello schermo corrispondono a regioni diverse nello spazio della texture. 
![[Pasted image 20241105141052.png]]

- ingrandimento: camera vicina ad oggetto, singolo pixel da mappare su regioni piccole della texture, basta interpolare valore nel centro del pixel
- rimpicciolimento: singolo pixel da mappare su regioni grandi della texture, occorre calcolare valore medio della texture sul pixel

Aliasing perchè un singolo pixel sullo schermo copre più di pixel della texture.
Idealmente si calcola un valore medio (molto costoso)
Idea: pre-calcolare dei valori medi (una volta soltanto) e poi usare queste info più volte a run-time come necessario

MIPmap : memorizza immagini pre-filtrate ad ogni scala
Disposizioni intelligenti in grado di ridurre la quantità di memoria in eccesso necessaria per memorizzare i vari livelli (d)
![[Pasted image 20241105141536.png]]
quale livello (d) utilizzare?
- campionamento da livelli diversi anche all'interno di uno stesso triangolo
- calcola diff tra coordinate di texture per campioni vicini
- $d = \log_2 L$, con $L = \sqrt{\max(L_x^2, L_y^2)}$

- arrotondamento : si possono produrre artefatti ai passaggi di livello (da texture nitida a sfocata)
- idea alternativa : invece di arrotondare all'intero più vicino, utilizza direttamente valore di d continuo
- problema : interpolare tra un insieme di livelli MIPmap discreto
	- soluzione: filtraggio tri-lineare (estensione filtraggio 2D a dati 3D) : computazionalmente complesso, da 4-8 texel, da 3-7 interpolazioni lineari

### Texture Mapping
- calcolo u, v dai campioni (x,y) attraverso interpolazione baricentrica
- calcolo delle differenze tra la coordinate di texture per campioni vicini per ottenere L
- calcolo del livello di MIPmap d a partire da L
- conversione delle coordinate di texture normalizzate (u,v) nello spazio \[0,1] a coordinate di pixel (U, V) nello spazio della texture \[W, H]
- Individuazione dei texel necessari per il filtraggio
### Ombre
- è possibile generare ombre fittizie senza eseguire verifica di visibilità --> trasformando ogni oggetto nella sua proiezione poligonale dalla sorgente di luce (puntiforme) sul piano
### Trasparenze
Superfici che trasmettono luce
- trasparenti : si vede attraverso anche se i raggi sono rifratti
- traslucide : trasmissione diffusa, raggi mescolati dalla superficie o da irregolarità interne. Oggetti visti attraverso superfici di questo tipo appaiono "confusi"

Trasparenza
- non rifrattiva : ciò che è visibile sulla linea di vista attraverso una superficie trasparente è anche localizzato geometricamente su quella linea di vista. Non realistica ma utile.
	- trasparenza interpolata
	- trasparenza filtrata
- rifrattiva : linee di vista ottica e geometrica diverse, indici di rifrazione
