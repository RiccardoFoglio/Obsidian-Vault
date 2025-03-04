
Architettura HW di un sistema grafico
![[Screenshot 2025-02-25 at 6.16.05 PM.png|500]]

Architettura SW di un sistema grafico
![[Screenshot 2025-02-25 at 6.17.19 PM.png|500]]

Frame Buffer: porzione di memoria dedicata alla memorizzazione dell'immagine come insieme di pixel da mostrare a video
Caratteristiche:
- risoluzione (numero di pixel)
- profondità (bit per pixel)
La struttura a matrice del frame buffer riproduce la struttura a maschera del CRT raster (o matrice LCD)


dispositivo di rappresentazione ha un limite di gamut rispetto ai colori rappresentabili
sistema RGB: somma additiva per ottenere il colore da rappresentare. I $2ˆ{24}$ possibili colori sono numericamente diversi ma non percettivamente, quindi limitato. Si sviluppano alternative ad RGB come HSV.
Diagramma CIE (ferro di cavallo) X Y L (luminanza fissa, quindi rappresentabile in 2D)
sui bordi del ferro ci sono i Red Blue Green puri, il triangolo che unisce i punti individua la regione rappresentabile in RGB. Il gamut del monitor è un subset ancora del triangolo RGB.

RGB sensibilità rispetto a lunghezza d'onda: G più sensibile all'occhio umano, quindi è il colore che si perde di più nel subset del triangolo RGB

### Grafica 3D

- Creare scene in mondo virtuale 3D composte da oggetti geometrici con attributi pittorici + immagini e testo
- Le scene esistono indipendentemente dal fatto che vengano visualizzate o meno
- Guardare le scene da un dato POV e visualizzare l'immagine corrispondente
- Interagire virtualmente con gli oggetti che compongono le scene
- Animare le scene in conseguenza di cambiamenti di: POV, Cono di vista, posizione degli oggetti, deformazione degli oggetti

Sintesi di immagini 3D:
Oggetti e Osservatore. Il punto di vista dell'osservatore cambia il render degli oggetti

punti, linee, triangoli (con rispettivi attributi pittorici) possono essere usati come primitive. Scheda video ottimizzata per processare le primitive semplici.

Trasformazioni geometriche: posizionare gli oggetti, traslazione, scalo, rotazione --> matrici.
Scheda grafica opera con le matrici, così come la CPU lavora con gli scalari.

possibile creare sfondi e inserire testo
Inutile la complessità se è distante e non si nota il dettaglio



Pin Hole Camera: foro di diametro infinitesimo
raggi di luce entrano dal foro e formano immagine sul fondo della camera
pinhole è detto centro di proiezione
rapporto tra lunghezza della camera e dimensioni del suo fondo determinano la porzione di scena inquadrata
modello astratto
- fuoco infinito --> l'occhio umano mette a fuoco a seconda della distanza, con limiti
- luminosità infinitesima

macchina fotografica:
- profondità di campo limitata
- maggior luminosità
- distorsioni varie

in Computer Grafica: tante differenze
immagine si forma dritta, non ribaltata, perchè si forma anteriormente al centro di proiezione

COP = Center of Projection
Parametri: 
- Eulero --> 6 gradi di libertà (3 pos e 3 orienamento)
- distanza focale

due set di parametri quindi:
- estrinseci --> dove e come l'ho messa
- intrinseci --> distorsione (dati dal produttore della lente/camera)

tutto rappresentabile tramite matrici

Proiezione Prospettica
- Front (Near) Clipping Plane
- Back (Far) Clipping Plane
usati per praticità, toglie informazioni che non contribuiscono alla scena perchè troppo lontani
front clipping plane di solito allo stesso livello del view plane, perchè toglie oggetti direttamente davanti alla visuale del COP, per mostrare correttamente e senza ostacoli il soggetto
![[Screenshot 2025-03-04 at 4.25.57 PM.png|500]]


Proiezione Ortografica
focale infinita, volume di vista non è piramide ma un parallelepipedo
una coordinata fissa
proiezioni perpendicolari

## Pipeline di Rendering
una volta specificati scena e viewer, il render avviene dopo:
- inquadrare scena secondo il viewer
- sintetizzare l'immagine corrispondente
- trasferire l'immagine sul frame buffer

Vertices --> Transformer --> Clipper --> Projector --> Rasterizer --> Pixels

L'algoritmo di rendering è strutturato per disegnare le primitive elaborandone una alla volta indipendentemente (object order)
Ogni primitiva passa attraverso la sequenza di elaborazione
- diversi stadi della pipeline eseguiti in parallelo su diverse primitive
- diversi gruppi di primitive alimentano diverse pipeline

Ogni oggetto ha un sistema di riferimento
- portare tutti i vertici che definiscono le primitive nello stesso sistema di riferimento
- ogni vertice viene sottoposto ad una trasformazione geometrica, che può essere diversa da primitiva a primitiva

Clipper: si decide cosa è visibile e cosa no al viewer
- porzioni di primitiva che stanno fuori dall'inquadratura vengono scartate
- in questa fase si decide anche il colore da dare ai vertici

Projector: vertici che cadono nel volume di vista vengono proiettati sull'immagine virtuale
- si calcola posizione di ogni vertice
- ogni posizione del piano cade in una cella del frame buffer

Rasterizer: si sa quale posizione del frame buffer definisce i vertici di ogni primitiva visibile
- necessario trovare tutti i pixel che nel frame buffer sono ricoperti dalla primitiva
- ciascuno dei pixel viene colorato con un colore desunto da quelli dati ai vertici secondo la regola
![[Screenshot 2025-03-04 at 4.38.53 PM.png|300]]

## Trasformazioni Geometriche

Traslazioni, Rotazioni, Scalamenti

Scena viene inquadrata da un dato punto di vista e con data angolazione
Esprimere oggetti della scena nel sistema di coordinate di vista oppure posizionare il sistema di coordinate di vista in un altro punto e con un altro orientamento

3 vettori linearmente indipendenti forniscono uno spazio vettoriale 3D, di solito si scelgono 3 versori
$$w = a_1v_1 + a_2v_2 + a_3v_3$$
![[Screenshot 2025-03-04 at 4.42.46 PM.png|300]]

Sistema di riferimento (FRAME)
- uno spazio di vettori (versori) di riferimento
- un origine da cui i versori partono

come si rappresenta un punto $P_0$? 
$$P = P_0 + a_1v_1 + a_2v_2 + a_3v_3$$

è possibile rappresentare tutto attraverso coordinate omogenee, in modo che non sia ambiguo
$$\{v_1, v_2, v_3, P_0\}$$

Vettore : $w = a_1v_1 + a_2v_2 + a_3v_3 + 0P_0$
Punto : $w = a_1v_1 + a_2v_2 + a_3v_3 + 1P_0$

Quarta coordinata: se 1 = punto, se 0 = vettore

Dati 2 sistemi di riferimento, si possono esprimere origine e vettori in termini dell'altro tramite una matrice 4x4 di cambiamento di sistema di riferimento

GPU in 1 colpo di clock riesce a fare un calcolo matriciale

dato un vettore/punto e le sue rappresentazioni A nel primo e B nel secondo sistema si ottiene:
![[Screenshot 2025-03-04 at 4.53.34 PM.png|500]]

la matrice M è invertibile perchè traslazione scalo e rotazione sono sempre invertibili

Esempio: Camera Virtuale
mondo: $\{x, y, z, O\}$
vista: $\{x_v, y_v, z_v, COP\}$
![[Screenshot 2025-03-04 at 4.57.42 PM.png|500]]

Trasformazioni affini:
- funzione che prende un punto e lo mappa in un altro
![[Screenshot 2025-03-04 at 4.59.39 PM.png|500]]

### Traslazione
$P' = P + d$ vale solo per i punti, non per vettori
![[Screenshot 2025-03-04 at 5.01.43 PM.png|500]]

### Rotazione
lascia un centro di rotazione fisso, e la distanza dei punti resta costante
caso particolare: rotazione attorno ad un asse: fissi tutti i punti della retta di rotazione
qualunque rotazione è ottenibile mediane una sequenza di rotazioni attorno ad assi indipendenti (x,y,z)

Rotazione 2D: rotazione intorno all'origine (asse z) di un angolo $\theta$ esprimendo in coordinate polari
![[Screenshot 2025-03-04 at 5.08.18 PM.png|500]]

### Scalamento

Trasformazione che lascia un punto fisso e dilata/comprime lo spazio attorno a quel punto in una o più direzioni per un dato fattore di scala

![[Screenshot 2025-03-04 at 5.13.14 PM.png|500]]


### Composizione di trasformazioni

diverse trasformazioni si possono comporre in sequenza, ognuna rappresentata da una matrice
applicare una trasformazione = moltiplicare una matrice per un vettore

Il prodotto matriciale è associativo ma non è commutativo, CBAp != BACp != ABCp

Esempio: si vuole applicare al punto p la trasformazione A seguita dalla trasformazione B seguita infine dalla trasformazione C

bisogna calcolare: CPAp
![[Screenshot 2025-03-04 at 5.15.55 PM.png|500]]
