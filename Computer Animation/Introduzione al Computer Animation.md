
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