# **Unit 1: Intro e le 3 "I" del VR**

- **Realtà Virtuale (VR):** Un'interfaccia uomo-macchina di alto livello che prevede la simulazione in tempo reale di un mondo realistico e interazioni attraverso molteplici canali sensoriali (vista, udito, tatto, e talvolta olfatto/gusto).
- **Realtà Aumentata (AR):** Una combinazione di percezioni dirette del mondo reale e contenuti mediati dal computer (sovrapposizione di elementi digitali al reale).
- **Realtà Mista (MR):** Un termine ombrello che descrive l'intero spettro tra la realtà fisica e la virtualità totale.

Il Continuum Realtà-Virtualità (Milgram, 1994) : la classificazione di Milgram ordina le tecnologie in base a quanto l'ambiente dell'utente sia generato dal computer:

1. **Realtà:** Il mondo fisico senza alcuna mediazione digitale.
2. **Realtà Aumentata (AR):** Il mondo reale è l'elemento predominante, arricchito da dati digitali.
3. **Virtualità Aumentata (AV):** L'ambiente è virtuale, ma vengono inseriti oggetti o persone reali (es. una videochiamata dentro un mondo VR).
4. **Realtà Virtuale (VR):** Immersione totale in un mondo sintetico; l'utente è "isolato" dal mondo fisico.

un'esperienza VR è definita da tre pilastri:
- **Immersione:** La capacità tecnica del sistema di fornire un ambiente circostante e vivido (dipende da hardware: risoluzione, FOV, tracking)
- **Interazione:** La capacità del mondo virtuale di rispondere istantaneamente alle azioni dell'utente (cruciale il concetto di _tempo reale_).
- **Immaginazione:** La capacità dell'utente di percepire la simulazione come reale, colmando le lacune del sistema (legata alla "Sospensione dell'Incredulità").

Immersione vs Presenza:
- **Immersione:** È una caratteristica **tecnologica** (oggettiva). Un visore con 4K e 110° di FOV è "più immersivo" di uno con 720p.
- **Presenza:** È uno stato **psicologico** (soggettivo). È la sensazione di "essere lì". Puoi sentirti "presente" anche in un mondo con grafica semplice se l'interazione è perfetta.

 Tipi di sistemi VR: 
- **VR desktop (Windows on World - WoW):** utilizza interfacce desktop standard (monitor, mouse, tastiera). Ha un basso livello di immersione e mobilità, ma è altamente accessibile.
- **VR immersiva:** la “vera” VR in cui l'utente è fisicamente circondato dalla simulazione. 
Le caratteristiche principali includono:
- **Canale visivo:** prospettiva accoppiata alla testa, visione stereoscopica e ampio campo visivo (FOV).
- **Feedback multimodale:** utilizzo di feedback audio, tattile e motorio.
- **Interazione realistica:** interfacce specifiche per la manipolazione e il controllo.

Evoluzione Storica
- **Sensorama (1962):** Creato da Morton Heilig. Era un simulatore di moto "passivo" ma multisensoriale (visione 3D, vibrazioni, odori, vento).
- **The Sword of Damocles (1968):** Ivan Sutherland realizza il primo vero Head-Mounted Display (HMD) collegato a un computer, capace di mostrare wireframe 3D che cambiavano con il movimento della testa.
- **Anni '80 e '90:** Nascono le prime aziende (VPL di Jaron Lanier) e prodotti come il _DataGlove_ Tuttavia, la VR fallisce commercialmente a causa di:
    1. Potenza computazionale insufficiente.
    2. Costo troppo elevato.
    3. Latenza eccessiva (causava nausea).

La VR è tornata alla ribalta grazie a:
- **Evoluzione delle GPU:** Capacità di rendering fotorealistico in tempo reale.
- **Miniaturizzazione:** Sensori (accelerometri, giroscopi) derivati dagli smartphone.
- **Motori Grafici (Unity, Unreal):** Hanno reso accessibile lo sviluppo di contenuti complessi.

Sfide Attuali
- **Cyber-sickness:** Malessere dovuto al conflitto tra ciò che gli occhi vedono e ciò che l'orecchio interno (sistema vestibolare) percepisce.
- **Isolamento Sociale:** Difficoltà nel condividere l'esperienza con altri nella stessa stanza.
- **Interfacce:** Trovare modi naturali per camminare (locomozione) e toccare (feedback aptico) nel mondo virtuale.

# Unit 2 : Input Devices

La presenza è la sensazione soggettiva di "essere lì". Si basa su 3 componenti:
- **Place Illusion (PI):** L'illusione di trovarsi in un luogo fisico reale. È legata alle capacità del sistema (es. se giro la testa, la vista si aggiorna correttamente).
- **Plausibility Illusion (Psi):** L'illusione che ciò che sta accadendo sia reale. Dipende dalla coerenza della simulazione (es. se tocco un tavolo, mi aspetto che sia solido o che faccia rumore).
- **Virtual Embodiment:** La sensazione che il corpo virtuale (avatar) sia il proprio. Un esperimento classico citato è la **Rubber Hand Illusion**.

Gradi di Libertà (DOF - Degrees of Freedom): È il concetto tecnico più importante per classificare i sensori:
- **3 DOF:** Il sensore traccia solo l'**orientamento** (rotazione). Le tre rotazioni sono:
    - **Beccheggio (Pitch):** Alto/Basso.
    - **Imbardata (Yaw):** Destra/Sinistra.
    - **Rollio (Roll):** Inclinazione laterale.
- **6 DOF:** Il sensore traccia sia l'orientamento che la **posizione** (traslazione sugli assi X, Y, Z). È necessario per potersi muovere fisicamente nello spazio virtuale.

Prestazioni del Tracking
- **Accuratezza (Accuracy):** La differenza tra la posizione reale e quella misurata.
- **Risoluzione:** Il più piccolo cambiamento che il sensore può rilevare.
- **Latenza:** Il tempo tra il movimento fisico e l'aggiornamento a schermo. **Deve essere < 20ms** per evitare il mal di mare (motion sickness).
- **Jitter:** Rumore nel segnale (il sensore "trema" anche se sei fermo).
- **Drift:** Errore cumulativo nel tempo (tipico dei sensori inerziali).

**Tecnologie di Tracking**
- **Magnetico:** Usa un trasmettitore di campo magnetico e ricevitori.
    - _Pro:_ Non serve linea di vista (funziona anche dietro la schiena).
    - _Contro:_ Molto sensibile ai metalli e alle interferenze elettromagnetiche.
- **Acustico (Ultrasuoni):** Misura il tempo di volo del suono.
    - _Pro:_ Economico.
    - _Contro:_ Bassa precisione, influenzato da rumori ambientali e temperatura.
- **Ottico (Infrarossi):** Il più comune oggi. Può essere:
    - **Outside-In:** Telecamere fisse nella stanza guardano i LED sul visore (es. Oculus Rift originale).
    - **Inside-Out:** Le telecamere sono sul visore e guardano l'ambiente esterno (es. Quest 2/3). Usa algoritmi **SLAM** per mappare la stanza.
- **Inerziale (IMU):** Accelerometri e giroscopi.
    - _Pro:_ Altissima frequenza di aggiornamento.
    - _Contro:_ Ha molto **drift** (va resettato spesso).

Interfacce Specializzate:
- **Data Gloves:** Guanti che misurano la flessione delle dita (es. tramite fibre ottiche o sensori di resistenza).
- **Locomozione:** Dispositivi per camminare restando fermi, come i **Tapis roulant omnidirezionali (ODT)** o la **VirtuSphere**.
- **Brain Computer Interface (BCI):** Uso dell'elettroencefalogramma (EEG) per controllare il VR con il pensiero (usato molto in ambito medico o militare).

# Unit 3 : 3D UI

Un'interfaccia 3D è un'interfaccia uomo-macchina in cui le azioni dell'utente avvengono in un contesto spaziale tridimensionale. Si possono usare:
- **Dispositivi di input 3D** (es. controller tracciati).
- **Dispositivi 2D mappati nel 3D** (es. un mouse che muove un cursore su un piano virtuale).

Task Fondamentali in una 3D UI: qualsiasi interazione 3D può essere scomposta in quattro categorie di task:
1. **Navigazione:** Muoversi nel mondo (Locomozione) e capire dove ci si trova (Wayfinding).
2. **Selezione:** Scegliere uno o più oggetti.
3. **Manipolazione:** Modificare le proprietà di un oggetto (posizione, rotazione, scala).
4. **Controllo del Sistema:** Cambiare stati dell'applicazione (aprire menu, salvare, ecc.).

Tecniche di Selezione e Manipolazione
- **Direct Manipulation (Mano Virtuale):** Tocchi l'oggetto direttamente. È la più naturale ma limitata dalla lunghezza del braccio.
- **Ray-casting (Puntatore Laser):** Un raggio esce dalla mano e seleziona l'oggetto colpito. Ottimo per oggetti lontani.
- **Go-Go Technique:** Una tecnica "magica" dove il braccio virtuale si allunga in modo non lineare (se estendi molto il braccio reale, quello virtuale arriva molto più lontano).
- **World-in-miniature (WIM):** Hai una copia in miniatura del mondo nelle mani. Muovendo gli oggetti nella miniatura, si muovono nel mondo reale.

Navigazione: Locomozione e Wayfinding:
- **Locomozione (Parte Motoria):**
    - **Walking:** Camminata fisica reale (limitata dallo spazio della stanza).
    - **Steering:** Usare un joystick per "guidare" il movimento (causa spesso motion sickness).
    - **Teleportation:** Istantaneo. È la tecnica più usata perché elimina il conflitto vestibolare (nausea).
- **Wayfinding (Parte Cognitiva):** Fornire aiuti all'utente per non perdersi, come mappe, bussole o punti di riferimento (landmark).

Tassonomia delle interfacce (Classificazione HUD)
- **Diegetiche:** Esistono nel mondo di gioco e sono percepite dal personaggio (es. guardare l'orologio sul polso dell'avatar per vedere l'ora).
- **Non-Diegetiche:** Menu classici 2D sovrapposti alla vista (es. barra della vita in alto a sinistra). Rompono l'immersione.
- **Spaziali:** Elementi UI messi nel mondo 3D, ma che il personaggio non vede "fisicamente" (es. un'aura luminosa sotto un oggetto selezionato).
- **Meta:** Elementi che non esistono nel mondo 3D ma mantengono la narrazione (es. schizzi di sangue sullo schermo quando vieni colpito).

Principi di Design (Human Factors):
- **Affordance:** L'oggetto deve suggerire come essere usato (es. una maniglia suggerisce di essere afferrata).
- **Feedback:** L'utente deve sempre avere una risposta immediata (visiva, sonora o vibrazione) quando interagisce.
- **Vincoli (Constraints):** Limitare le azioni possibili per evitare errori (es. non poter muovere un tavolo attraverso un muro).

# Unit 4 : Output Devices

Il sistema di output riceve i dati dal motore di simulazione e li trasforma in stimoli sensoriali per l'utente.

Display Visivi (HMD - Head Mounted Displays): Il visore è il cuore dell'esperienza VR
- **FOV (Field of View):** Il campo visivo umano è di circa 200° in orizzontale. I visori attuali variano tra i 90° e i 110°. Più è ampio, maggiore è il senso di immersione.
- **Risoluzione e Densità (PPD):** Non conta solo il numero totale di pixel, ma i **Pixel Per Degree (PPD)**. Se i pixel sono troppo pochi o "spalmati" su un FOV troppo grande, si vede la griglia dei pixel (**Screen Door Effect**).
- **Refresh Rate:** La frequenza di aggiornamento delle immagini (solitamente 72Hz, 90Hz o 120Hz). Frequenze basse causano sfarfallio e nausea.
- **Latenza Motion-to-Photon:** Il tempo totale tra il movimento della testa e la luce che esce dai pixel del visore. Deve essere **< 20ms**.

Visione Stereoscopica : per vedere in 3D, il sistema deve generare due immagini leggermente diverse (una per occhio), simulando la **disparità retinica**.
- **IPD (Inter-Pupillary Distance):** La distanza tra le pupille. È fondamentale che il visore permetta di regolare le lenti per allinearle agli occhi dell'utente, altrimenti l'immagine risulterà sfocata e affaticante.

Output Audio (3D Spatial Audio) : L'audio in VR non è semplice "stereo", ma deve essere posizionale.
- **HRTF (Head-Related Transfer Function):** È l'algoritmo matematico che modella come le onde sonore interagiscono con la forma delle nostre orecchie, testa e spalle. Questo permette al cervello di capire se un suono viene dall'alto, dal basso, davanti o dietro.

Feedback Aptico (Tatto): Si divide in due grandi categorie
- **Feedback Tattile (Haptic Feedback):** Stimola i recettori della pelle. Fornisce sensazioni di vibrazione, texture (ruvido/liscio) o temperatura. (Es: la vibrazione dei controller).
- **Feedback Cinestesico (Force Feedback):** Stimola i muscoli e i tendini. Fornisce sensazioni di resistenza fisica, peso o forza. (Es: un braccio meccanico che ti impedisce di chiudere la mano quando stringi un oggetto virtuale solido).

**Display Inerziali** (Simulatori di Movimento): sistemi meccanici (come le piattaforme Stewart a 6 gradi di libertà) che muovono fisicamente l'utente.
- Servono per simulare le forze di accelerazione (gravità, curve, frenate).
- Sono fondamentali nei simulatori di volo o di guida professionali.

# Unit 5 : Architetture per VR

Un sistema VR non è solo un computer che "mostra immagini", ma un insieme di processi coordinati.

Il VR Engine (Motore VR) : è l'astrazione software che gestisce l'intera esperienza. 
Si occupa di tre compiti principali:
- **Input Processor:** Legge i dati dai sensori (posizione, bottoni, gesti).
- **Simulation Processor:** Gestisce la logica del mondo (fisica, collisioni, script, intelligenza artificiale).
- **Rendering Processor:** Genera gli output per l'utente (immagini 3D, suoni spazializzati, vibrazioni).

Il Flusso di Dati : esiste un ciclo continuo (loop) che deve essere il più veloce possibile per mantenere l'immersione:
1. L'utente compie un'azione.
2. I sensori inviano i dati all'**Input Processor**.
3. La simulazione aggiorna lo stato del mondo (**Simulation Processor**).
4. Il sistema genera i feedback visivi/uditivi/aptici (**Rendering Processor**).
5. L'utente percepisce il cambiamento e reagisce.

Architetture Hardware: il "VR Engine" può essere implementato in diversi modi:
- **Standalone (Singolo Computer):** Tutto il carico (input, fisica, grafica) è gestito da un'unica macchina (es. PC con visore cablato o visori come il Meta Quest).
- **Architetture Distribuite:** Il carico è diviso tra più computer collegati in rete. Questo è necessario per:
    - Sistemi multi-utente (VR collaborativa).
    - Sistemi ad altissime prestazioni (es. CAVE o simulatori professionali).

Networking in VR (Sistemi Multi-utente): quando più utenti sono nello stesso spazio virtuale, sorgono sfide tecniche:
- **Topologia Client-Server:** Un server centrale mantiene la "verità" dello stato del mondo. I client inviano i propri movimenti e ricevono gli aggiornamenti sugli altri utenti.
    - _Vantaggi:_ Coerenza dei dati.
    - _Svantaggi:_ Se il server cade, cade tutto; latenza dovuta alla distanza dal server.
- **Sistemi Multi-Server:** Più server collaborano (scalabilità). Migliora l'affidabilità ma è più complesso sincronizzare i cambiamenti tra i server.

Problematiche della Rete:
- **Latenza di Rete:** Il tempo necessario affinché un pacchetto dati viaggi tra client e server. In VR, una latenza alta causa "lag" nei movimenti degli altri avatar (li vedi a scatti).
- **Banda Passante:** Necessaria per trasmettere modelli 3D complessi o flussi video in tempo reale.
- **Sincronizzazione:** Garantire che tutti gli utenti vedano la stessa cosa nello stesso momento (es. se lancio una palla, tutti devono vederla colpire lo stesso punto).
# Unit 6 : Modellazione VR

La modellazione è la fase di definizione delle caratteristiche degli oggetti nel **World Database**. Si articola in quattro livelli principali:

1. **Modellazione Geometrica** : rappresenta la forma e l'aspetto visivo dell'oggetto.
- **Metodi di creazione:**
    - **Sintesi:** Creazione manuale tramite software (es. Blender, Maya) usando poligoni o superfici matematiche.
    - **Campionamento (3D Scanning):** Uso di laser scanner o fotogrammetria per ricostruire oggetti reali.
- **Elementi chiave:**
    - **Geometria:** Solitamente basata su mesh poligonali (triangoli). Per la VR real-time è fondamentale il **Polygon Budget** (mantenere basso il numero di poligoni).
    - **Apparenza (Materiali e Texture):** Definizione di come l'oggetto riflette la luce, il suo colore e i dettagli superficiali tramite mappe di immagini.

2. **Modellazione Cinematica** : definisce come gli oggetti possono muoversi nello spazio senza considerare le forze (causa del movimento).
- **Gradi di Libertà (DOF):** Quanti e quali movimenti sono permessi a un oggetto o a una parte di esso.
- **Gerarchie e Vincoli:** Gli oggetti possono avere strutture gerarchiche (es. braccio -> avambraccio -> mano). Se muovo la spalla, la mano segue il movimento.

3. **Modellazione Fisica** : aggiunge "massa" e regole dinamiche agli oggetti per renderli realistici.
- **Proprietà:** Massa, attrito, elasticità, baricentro.
- **Collision Detection:** Il sistema deve calcolare costantemente se due oggetti si toccano. In VR, per risparmiare calcoli, si usano spesso **Bounding Boxes** (scatole invisibili semplificate) invece della mesh complessa per rilevare gli impatti.

4. **Modellazione del Comportamento (Agenti Intelligenti)** : Riguarda gli oggetti "vivi" o autonomi (NPC - Non-Player Characters).
- **Stato Interno:** L'agente può avere variabili come "fame", "paura" o "energia".
- **Percezione:** La capacità dell'agente di "vedere" o "sentire" l'utente o altri oggetti nel raggio d'azione.
- **Logica Decisionale:**
    - **Macchine a Stati Finiti (FSM):** L'agente passa da uno stato all'altro (es. da "Pattuglia" a "Inseguimento") in base a trigger.
    - **Behavior Trees (BT):** Strutture ad albero più complesse per comportamenti gerarchici e prioritari.
# Unit 7 : Management

Il problema fondamentale della VR è che il rendering deve essere completato in meno di **11ms** (per i 90Hz) o **13ms** (per i 72Hz). Se la scena è troppo complessa, il frame rate cala.

Librerie e Livelli di Astrazione : Esistono diversi modi per gestire il rendering:
- **Librerie di basso livello:** OpenGL, DirectX, Vulkan. Offrono massimo controllo ma sono difficili da usare.
- **Scene Graph API:** Librerie come OpenSceneGraph che organizzano il mondo in una gerarchia di nodi (albero).
- **VR Engines (Unity/Unreal):** Forniscono strumenti pronti all'uso per fisica, audio, grafica e IA.

Tecniche di Ottimizzazione : Per mantenere il tempo di calcolo sotto la soglia critica, si usano diverse strategie:
**A. Culling (Rimozione di ciò che non si vede)**
- **View Frustum Culling:** Il sistema non renderizza gli oggetti che si trovano fuori dalla "piramide visiva" della telecamera.
- **Occlusion Culling:** Se un oggetto grande (es. un muro) copre un oggetto piccolo dietro di sé, il sistema evita di disegnare l'oggetto nascosto.
**B. LOD - Level of Detail (Livello di Dettaglio)** Questa è la tecnica più celebre. 
Consiste nell'avere diverse versioni dello stesso modello 3D:
- **High Poly:** Molti dettagli, usato quando l'oggetto è vicino all'utente.
- **Low Poly:** Pochi dettagli, usato quando l'oggetto è lontano.
- Il sistema scambia queste versioni in base alla distanza dalla telecamera per risparmiare calcoli della GPU.
**C. Detail Elision** Se un oggetto è così piccolo o lontano da occupare meno di un pixel sullo schermo, il sistema smette completamente di renderizzarlo.

Gestione del Database (World Database) : il mondo virtuale può essere troppo grande per stare tutto nella RAM. Si usano tecniche di gestione della memoria:
- **Cell Segmentation:** Il mondo viene diviso in "celle" (stanze o settori). Viene caricata in memoria solo la cella in cui si trova l'utente e quelle adiacenti.
- **Predictive Loading (Look-ahead):** Il sistema analizza la direzione del movimento dell'utente e inizia a caricare dal disco le celle o gli oggetti che probabilmente vedrà tra pochi secondi.

Time-critical Rendering : se il sistema si accorge che non farà in tempo a renderizzare il frame, può decidere di:
- Ridurre temporaneamente la qualità dei modelli (LOD forzato).
- Interrompere il rendering di oggetti non essenziali.
- **Latenza:** Ricorda che la latenza totale "Motion-to-Photon" deve essere **< 20ms**. Se il rendering è troppo lento, questa soglia viene superata.
# Unit 8 : System Architecture

L'obiettivo dell'AR è mantenere l'utente nel mondo fisico, arricchendo la sua percezione con informazioni generate dal computer.

Secondo la definizione classica, un sistema AR deve:
1. **Combinare reale e virtuale** in un ambiente reale.
2. **Essere interattivo in tempo reale**.
3. **Essere registrato in 3D** (allineamento spaziale preciso tra oggetti virtuali e reali).

Esistono due approcci principali per mostrare contenuti AR:
**A. Video See-Through (VST)**
- L'utente guarda il mondo attraverso una telecamera. Il computer combina il flusso video reale con la grafica virtuale e mostra il risultato su uno schermo.
- **Pro:** Controllo totale sui pixel (posso oscurare completamente il reale), latenza sincronizzata tra reale e virtuale.
- **Con:** Percezione del mondo limitata dalla risoluzione della telecamera, ritardo (lag) visivo del mondo reale.
**B. Optical See-Through (OST)**
- L'utente vede il mondo reale direttamente attraverso lenti trasparenti. La grafica viene proiettata sulle lenti tramite specchi o guide d'onda (es. HoloLens, Magic Leap).
- **Pro:** Visione naturale e istantanea del mondo reale (sicurezza), risoluzione del reale infinita.
- **Con:** Difficile rendere gli oggetti virtuali opachi (sembrano "fantasmi"), FOV ridotto, problemi di calibrazione tra occhio e lenti.

In AR, il tracking è molto più critico che in VR. Se il tracking "balla" in VR, l'utente sta male; in AR, l'oggetto virtuale sembra scivolare sulle superfici reali, rompendo l'illusione.
- **Tracking basato su Marker:** Usa immagini predefinite (QR code, loghi) per stabilire la posizione.
- **Tracking Markerless (SLAM):** _Simultaneous Localization and Mapping_. Il sistema mappa l'ambiente circostante in tempo reale usando le telecamere e stima la posizione del dispositivo senza bisogno di marker.

Componenti di un sistema AR :
- **Sensori:** Telecamere, IMU (accelerometri/giroscopi), sensori di profondità (LiDAR/ToF).
- **Processore:** Deve gestire la visione artificiale (computer vision) per riconoscere il mondo e il rendering grafico simultaneamente.
- **Display:** Mobile (smartphone), HMD (visori), o Spatial AR (proiettori che proiettano direttamente sugli oggetti reali).

SDK e Strumenti di Sviluppo
- **ARKit:** Framework di Apple per dispositivi iOS.
- **ARCore:** Framework di Google per Android.
- **Vuforia:** SDK molto popolare per il tracking di immagini e oggetti 3D.
- **MRTK (Mixed Reality Toolkit):** Libreria per sviluppare interazioni avanzate (gesti, tracciamento oculare) su HoloLens e altri visori.

# Unit 9 : Futuro del VR

Miglioramento della Qualità Visiva :per arrivare alla "risoluzione retinica" (dove l'occhio non distingue più i pixel), c'è ancora molta strada da fare:
- **PPD (Pixel Per Degree):** L'occhio umano ha una risoluzione di circa **60-70 PPD**. I visori attuali (come l'Apple Vision Pro) arrivano a circa 34-40 PPD. Il futuro punta a superare i 70 PPD.
- **Risoluzione:** Si punta all'**8K per occhio** per coprire un campo visivo ampio senza perdere nitidezza.

Eye Tracking e Foveated Rendering : una delle tecnologie più citate per il futuro prossimo:
- **Eye Tracking:** Sensori interni al visore tracciano dove sta guardando esattamente la pupilla.
- **Foveated Rendering:** Sfrutta il fatto che l'occhio umano vede nitidamente solo nella parte centrale della retina (la _fovea_).
    - Il sistema renderizza alla massima qualità solo la piccola zona che l'utente sta guardando.
    - Tutto il resto (la visione periferica) viene renderizzato a bassa risoluzione.
    - **Risultato:** Risparmio enorme di potenza di calcolo (fino al 50-70%) senza che l'utente se ne accorga.

Problemi di Accomodazione (Vergence-Accommodation Conflict) : nei visori attuali, le lenti hanno un fuoco fisso. Questo crea un conflitto:
- **Vergenza:** Gli occhi convergono correttamente sull'oggetto virtuale.
- **Accomodazione:** Il cristallino dell'occhio rimane focalizzato sulla distanza fissa dello schermo del visore.
- **Soluzione futura:** **Display Varifocali** o **Light Field Displays**, che permettono all'occhio di cambiare fuoco dinamicamente proprio come nella realtà.

Nuove Frontiere Sensoriali: 
- **Tatto Avanzato:** Guanti aptici più leggeri (es. _SenseGlove_) o tute integrali (_Teslasuit_) che usano l'elettrostimolazione muscolare (EMS) per simulare impatti e resistenze.
- **Olfatto e Gusto:** Esistono prototipi di "collari" che rilasciano essenze profumate sincronizzate con la scena (es. odore di foresta o di bruciato), ma sono ancora poco diffusi.
- **Interfacce Neurali (BCI):** Il sogno è il collegamento diretto con il cervello (stile Matrix), bypassando i sensi fisici (es. i progetti di _Neuralink_).

Social VR e Avatar : il futuro mira a superare gli avatar "cartooneschi" per arrivare ai **Codec Avatars** (Facebook/Meta): ricostruzioni fotorealistiche basate su AI che replicano le espressioni facciali dell'utente in tempo reale tramite telecamere interne.