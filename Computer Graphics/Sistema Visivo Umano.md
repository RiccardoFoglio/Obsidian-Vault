Sistema percettivo umano comprende:
- visiva
- uditiva
- tattile
- gustativa
- olfattiva

I sistemi di Realtà Virtuale cerca di ricreare i diversi sensi, la più importante è la vista
Il compito della Computer Graphics è quello di creare immagini ed animazioni per gli esseri umani
Risulta essenziale capire come funzioni il sistema visivo

Sistema visivo è influenzato da diversi fattori percettivi:
- colore
- acutezza o acuità visiva (visus), capacità dell'occhio di risolvere e percepire dettagli fini
- profondità (tridimensionale)
- livelli di luminosità
- sensibilità alle variazioni temporali
- campo visivo (FOV)

Sistema complesso, dati grezzi forniti dagli occhi sono pesantemente elaborati dal cervello
Molti dettagli dell'elaborazione sono tuttora sconosciuti

## Risoluzione Spaziale (acutezza)

diversi aspetti: acutezza di visibilità, allineamento, riconoscimento ecc...

Acutezza di risoluzione: capacità di percepire i dettagli fini di un oggetto
Inverso delle dimensioni angolari minime che un oggetto deve avere per poter essere percepito correttamente.

Fondamentale per la progettazione dei dispositivi di output grafici

Dipende dalla luminosità e contrasto

Osservabile sperimentalmente, guardando un pattern composto di righe bianche e nere alternate sino a quando non scompaiono

Risultato: limite di 60 cicli/grado, ovvero 1/2 min d'arco (luna piena circa 30 min o mezzo grado) corrispondono a 600dpi a 12" o 300dpi a 24"

![[Pasted image 20241003171928.png|400]]

Inutile creare output grafici più fini dell'occhio umano, non viene percepito, costo inutile

Esempio: schermo da 20" osservato a 24" di distanza: serve 6000x6000 pixel
- Risoluzione degli schermi è di circa 70-100 dpi
- schermi "retina" hanno risoluzioni superiori (iphone 6: 401dpi etc...)
- stampanti hanno risoluzione tipica di 300-600 dpi
- le più alte risoluzioni spaziali raggiungono i 2500dpi (macchine per foto-composizione)

## Risoluzione Temporale

Frequenza critica di fusione (del flicker) CFF
	sopra questa frequenza l'osservatore non è in grado di osservare variazioni dovute ad uno stimolo luminoso intermittente (sfarfallio, flicker)
	Dipende da tanti fattori, circa 35-60Hz

2 aspetti:
- immagini senza **flicker** (**refresh rate**) -> cicli di attiv./disattiv. delle tecnologie di visualizzazione. La frequenza di aggiornamento deve essere superiore a CFF (60Hz per schermi CRT)
- **animazioni fluide**, da sequenza di immagini (**frame rate**) -> la frequenza di aggiornamento deve essere superiore a CFF (24Hz, fotogrammi proiettati due/tre volte, 48/72Hz)

## Campo Visivo

Angolo sotteso dalla superficie visibile dal punto di vista dell'osservatore
- occhio singolo: 150 gradi
- i campi visivi dei due occhi si sovrappongono in orizzontale
- area di sovrapposizione: 120 gradi con 30-35 gradi di visione monoculare sui lati
- campo visivo orizzontale combinato: 180-200 gradi
- campo visivo verticale: 120-135 per entrambi gli occhi
- tipico schermo desktop: 40x32 a 46cm

L'essere umano usa gli occhi, testa e corpo per mantenere gli oggetti all'interno della cosiddetta **regione foveale** (a massima risoluzione)

![[Pasted image 20241003180556.png]]
## Frequenze e Range Dinamico

L'occhio umano è in grado di rispondere ad una banda ristretta della radiazione elettromagnetica
da 430nm (violetto) a 790nm (rosso), picco a 559nm
allineata all'emissione spettrale della luce solare

![[Pasted image 20241003180905.png]]

Livelli di luminosità percepibili: da pochi fotoni a livelli di luminosità di 10 ordini di grandezza più elevati, ma non contemporaneamente, effettua un **adattamento**
Nessuno schermo raggiunge l'intero range percepibile

La luminosità soggettiva (percepita) è una funzione logaritmica di quella incidente:
la curva $B_a - B_b$ rappresenta il range di luminosità che l'occhio può percepire se adattato a quel livello
![[Pasted image 20241003181133.png|350]]

Esperimento (Weber): sfondo costante, luce lampeggiante

Percezione dipende da diversi fattori, come la luminosità dello sfondo.
Diversi recettori dell'occhio si occupano di diversi livelli di luminosità

livelli diversi che possono essere visti in un determinato punto di un immagine monocromatica è di poche decine, ma muovendo velocemente l'occhio ci permette di percepire un insieme diverso di livelli ad ogni adattamento.
L'occhio è in grado di discriminare tra un insieme più ampio di intensità globali

il numero di livelli per un'immagine a toni di grigio dipende dal **range dinamico** del dispositivo di visualizzazione (rapporto tra livelli di intensità massimo e minimo $I_M / I_m$ )
Assumendo 1.01 il rapporto minimo affinché due intensità siano indistinguibili, il numero teorico di livelli *n* è ottenuto con: $n^{1,01} = I_M / I_m$

![[Pasted image 20241003192058.png]]

**Effetto Mach**: strisce verticali hanno un tono grigio omogeneo, ma vengono percepiti diversamente

L'effetto è dovuto al fatto che il sistema visivo tende a "esagerare" o "smorzare" la percezione del confine tra regioni con intensità diverse (**inibizione laterale**, neuroni non occhi)
Particolarmente rilevante per l'ombreggiatura di superfici poligonali

![[Pasted image 20241003212322.png|300]]

## Profondità

Approccio basato sui suggerimenti (**cues**) informazioni 2D che permettono di percepire l'immagine come 3D

Informazioni monoculari:
- Occlusioni
- Ombre
- Altezza / Dimensioni Relative
- Dimensioni di Oggetti Comuni
- Prospettiva Atmosferica
- Prospettiva Lineare
- Gradienti di Texture

Informazioni oculomotorie:
- Convergenza e accomodazione (fissazione e messa a fuoco): abilità di determinare la posizione degli occhi e della tensione muscolare

Informazioni binoculari:
- Disparità binoculare
- Stereopsi: capacità percettiva che consente di unire le immagini proveniente dai due occhi

Informazioni dal movimento:
- Parallasse
# Fisiologia del Sistema Visivo
 
L'occhio umano è molto simile ad una sferra di 20mm di diametro
4 membrane racchiudono l'occhio:
- **cornea** --> tessuto trasparente che copre la superficie anteriore dell'occhio
- **membrana sclerotica** --> tessuto opaco contiguo alla cornea che ricopre il resto del bulbo oculare
- **membrana coroidea** --> immediatamente sotto quella sclerotica, contiene ricca rete di vasi sanguigni che costituiscono la principale fonte di nutrimento dell'occhio.
  Nella parte anteriore: divisone in corona ciliare e iride.
  Iride: contrae o allarga a seconda della quantità di luce che entra nell'occhio
  Pupilla: apertura centrale dell'iride, ha un diametro variabile 2-8mm
![[Pasted image 20241003214151.png|450]]
- Corpo (o umor, vitreo) contribuisce al nutrimento dell'occhio ed a mantenere la forma sferica sotto pressione
- Cristallino: (lente) è costituito da strati concentrici di cellule fibrose ed è sospeso mediante fibre che si attaccano alla corona ciliare, per il 60/70% contiene acqua, 6% grasso, resto proteine
  Assorbe l'8% della luce visibile e l'assorbimento aumenta al diminuire della lunghezza d'onda
  Luci ultraviolette e infrarosse sono quasi totalmente assorbite dalle proteine nel cristallino, eccessiva esposizione danneggia l'occhio.
- **Retina** : membrana più interna, forma l'immagine. la formazione è prodotta da 2 tipi di recettori: **coni** e **bastoncelli**
	- Coni (CONES): 6-7M di recettori, localizzati nella parte centrale della retina (fovea) e altamente sensibili al colore ed elevati livelli di illuminazione (visione fotopica)
	  L'occhio umano risolve i dettagli usando i coni perché ognuno è connesso ad un nervo.
	- Bastoncelli (RODS): muscoli che controllano l'occhio e lo fanno ruotare finché l'immagine non cade nella fovea. 75-150M. bastoncelli sono distribuiti su tutta la superficie della retina, connessi a gruppi di nervi, riduce l'ammontare di dettagli distinguibili.
	  
![[Pasted image 20241003215627.png|500]]

la principale differenza tra il cristallino e una lente è la flessibilità.
I raggi di curvatura, anteriore e posteriore, del cristallino sono differenti e vengono controllati dalla tensione delle fibre del corpo ciliare.
Per focalizzare oggetti lontani, i muscoli ciliari fanno si che il cristallino si appiattisca, mentre lo rendono più spesso per focalizzare oggetti vicino (accomodamento)

![[Pasted image 20241003213224.png]]


Example:
Proportion: 15/100 = H/17
![[Pasted image 20241003215243.png|500]]
H = 2.55mm