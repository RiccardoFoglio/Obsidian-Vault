## Grafica Raster

Tutte le tecnologie successive al CRT (cristalli liquidi, plasma...) hanno adottato il modello della grafica raster

Scopo: illuminare i pixel in modo opportuno per formare l'immagine desiderata

Per ottenere i colori opportuni è necessario modulare la stimolazione degli elementi luminosi durante la scansione (digitale)

È necessario memorizzare i valori di intensità (colore) da assegnare ad ogni pixel

- area di memoria dedicata al display -> **frame buffer**
- Esempio: 1280x1024x24bit = 3,75MB

Pro: costi inferiori, possibilità di festire aree "filled"

Contro: natura discreta dei pixel, conversione delle **primitive grafiche** in pixel nel frame buffer o *rasterizzazione* (**scan conversion** o **rasterization**) approssimazione (**aliasing**)

### Algoritmo di rasterizzazione

Quali pixel illuminare per una linea?
![[Pasted image 20240924193252.png|500]]

1. illuminare i pixel intersecati dalla linea
![[Pasted image 20240924193323.png|500]]

2. regola (test) del diamante
![[Pasted image 20240924193358.png|500]]

#### Approccio Incrementale

```C
v = v1
for(u=u1; u<=u2; u++){
	v+= s;
	draw(u, round(v));
}
```
![[Pasted image 20240924193802.png]]


