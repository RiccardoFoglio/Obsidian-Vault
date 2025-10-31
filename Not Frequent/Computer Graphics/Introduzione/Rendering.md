Cubo ¨ Centrato nell’origine (0,0,0) ¨ Dimensione 2×2×2 ¨ Spigoli allineati agli assi x, y e z

Rappresentazione dei vertici 
A: ( 1, 1, 1)      E: ( 1, 1, -1) 
B: ( -1, 1, 1)     F: ( -1, 1, -1) 
C: ( 1, -1, 1)     G: ( 1, -1, -1) 
D: ( -1, -1, 1)   H: ( -1, -1, -1) 

Rappresentazione degli spigoli 
AB, CD, EF, GH, 
AC, BD, EG, FH, 
AE, CG, BF, DH

da oggetto 3D a immagine 2D?

##### Mappare i vertici 3D in punti 2D
![[Pasted image 20240924133828.png|400]]
proiezione prospettica, pinhole camera (stenoscopio o camera oscura)

più l'albero è lontano, più l'immagine è piccola

Proiezione prospettica, vista laterale
![[Pasted image 20240924134118.png]]
Mappatura di un punto $p = (x,y,z)$ sul piano dell'immagine
Punto sul piano dell'immagine : $q = (u,v)$
![[Pasted image 20240924134333.png]]
Camera di dimensione 1x1x1
C è il centro di proiezione, foro
$v/1 = y/z$
Analogamente $u = x/z$

Posizione della Camera ( C )
per ciascuno dei 12 spigoli:
- Convertire (X, Y, Z) degli estremi dello spigolo in (u, v)
	- sottraendo le coordinate di C dal vertice
	- dividendo (x, y) per z = (u, v)
- Disegnare un segmento tra (u1, v1) e (u2, v2)



![[Pasted image 20240924134926.png|400]]
![[Pasted image 20240924134940.png|400]]

## Algoritmi di Rendering

- Online Rendering
	- Interattivo : 1-10 fps
	- Real-Time : 10-100fps
- Offline Rendering

Molto diversi
- applicazioni
- vincoli
- qualità
- algoritmi

![[Pasted image 20240924191904.png]]
