animazione interattiva 

Sistema real-time = task completato entro deadline specificata 

Per evitare di aspettare l'aggiornamento del fotogramma successivo, il rendering è eseguito sul buffer di memoria nascosto : **Double Buffering**

![[Screenshot 2025-05-13 at 4.16.15 PM.png|400]]

## Compositing

Compositing = combinare layer di immagini separate in un unica immagine

Permette all'animatore di combinare insieme elementi ottenuti da different sorgenti
Gli elementi individuali possono essere manipolati selettivamente senza dover rigenerare la scena complessiva

uso comune del compositing : separare background e foreground

Foreground può essere segmentato per avere ogni oggetto su un singolo layer
	Il compositing offre vantaggi ancora più interessanti quando vengono usati dei buffer per memorizzare informazioni di profondità (z-value)

### Senza Z-Value

Il procedimento di compositing che non tiene conto di z-value si può riassumere come:

$$
compositing(render(scena1),render(scena2)) = render(merge(scena1,scena2))
$$

l'operazione di compositing si riferisce all'unione di due immagini, mentre il merge fa riferimento all'unione di geometrie

la funzione di render rappresenta il processo di rendering di un'immagine partendo da una geometria.

La procedura di render deve etichettare come trasparenti i pixel ce non sono coperti da oggetti della scena

l'equivalenza vale solo se: 
- le scene sono disgiunte in profondità rispetto all'osservatore
- la funzione di compositing dà precedenza all'immagine più vicina all'osservatore

La visibilità tra elementi di piani differenti può essere gestita assegnando un valore di priorità ai vari layer

l'operatore di *over* è quello che alloca un piano su di un altro

Nel caso più semplice l'immagine di foreground occlude completamente il background
- utile quando foreground è piccolo
- foreground = sprite
- coordinate 2D posizionano lo sprite rispetto al background

![[Screenshot 2025-05-13 at 4.31.27 PM.png|400]]

Più spesso, info aggiuntive espressi in forma di maschera di occlusione, sono fornite
	(riferite con i termini: *matte* o *key*)

una maschera ad un bit può essere usata per indicare quali pixel del foreground devono occludere il background durante il processo di compositing
- tecnica molto usata per inserire informazioni testuali (es: sottotitoli)
- la "key" forse più famosa è la chroma-key (green screen, tono di verde non usato in nulla se non green-screen)

![[Screenshot 2025-05-13 at 4.35.26 PM.png|500]]

Compositing = operazione binaria --> combinare due immagini in un'immagine singola
numero arbitrario di immagini può essere composto
- immagini devono essere ordinate in profondità ed elaborate due alla volta
- immagini composte in qualunque ordine, purchè siano adiacenti nell'ordinamento in profondità

maschere a scale di grigio = maschere alpha
- canale alpha è aggiunto a RGB --> RGBA
- tipicamente dimensione: 8-bit

![[Screenshot 2025-05-13 at 4.41.10 PM.png|400]]

![[Screenshot 2025-05-13 at 4.46.18 PM.png|400]]
### Con Z-Value

spesso si memorizzano i valori RGB pre-moltiplicati per canale alpha:
RGBA(1,1,1,0.5) --> rgba(0.5, 0.5, 0.5, 0.5)
velocizza i calcoli

ora si considerano anche i valori di profondità, che gli algoritmi di rendering (come ray tracing) possono generare, si vanno a comporre immagini dove per ogni pixel son disponibili: rgb$\alpha$z

altro operatore, oltre a *over*, è **zmin** : seleziona i valori di rgb$\alpha$z del pixel più vicino all'osservatore (z minore)

```
if(zf < zb) 
	rgba_c = rgba_f 
else 
	rgba_c = rgba_b 
zc = min(zf,zb)
```

terzo operatore: **comp**, combina operatori di *over* e *zmin*
	pixel contiene un valore di rgb e un valore di $\alpha$
	per stimare le relazioni di copertura, ogni pixel ha un valore di z diverso da ognuno dei 4 angoli

per calcolare **c = f comp b** bisogna valutare $2ˆ4 = 16$ combinazioni ai quattro angli di un pixel

![[Screenshot 2025-05-13 at 5.00.40 PM.png|400]]

- Whole: intero pixel coperto da una superficie o l'altra --> $\beta = 0/1$
- Corner:  1 angolo diverso dagli altri tre. --> $\beta = 1.0 - (1/2*s*t)$
- Split: vertici dei lati opposti etichettati in modo differente --> $\beta = 1.0 - (s + t/2)$
- two-opposite corner: vertici diagonali sono etichettati in modo diverso --> $\beta$ come split

## Motion Blur

oggetti che cambiano nel tempo se campionati ad una frequenza non sufficiente possono dare origine ad animazioni di strobing

![[Screenshot 2025-05-13 at 5.10.43 PM.png|400]]

aliasing temporale 

# Compositing in Blender

