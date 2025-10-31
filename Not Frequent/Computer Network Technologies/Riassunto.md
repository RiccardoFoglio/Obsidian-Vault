## ✅ 1. IPv4 – Concetti Chiave

### 📌 Indirizzi IPv4
- Formato: `b1.b2.b3.b4/x` (32 bit)
- Validità:
  - `bi` deve essere multiplo di `2^(32-x)` a seconda del prefisso
  - `/31` **non valido** per subnet normali (solo 2 indirizzi: rete e broadcast)

### 🧮 Calcolo Netmask e Prefix Length
- `#indirizzi = #host + 2`
- Trova il più piccolo `2^n ≥ #indirizzi`
- `Prefix length = 32 - n`
- Netmask = `256 - 2^n` sul byte rilevante

### 🔁 Range di Indirizzi
- `#bit host = 32 - prefix`
- Indirizzo rete = tutti 0 nei bit host
- Indirizzo broadcast = tutti 1 nei bit host
- **Range utilizzabile**: da `(rete + 1)` a `(broadcast - 1)`

### 📚 Longest Prefix Matching
- Router fa `AND` tra IP e netmask delle route
- Sceglie il **prefisso più lungo** tra i match

### 📦 Aggregazione di Reti
- Due subnet sono aggregabili se condividono i bit iniziali
- Es.: `130.192.0.0/25` + `130.192.1.0/25` → `130.192.0.0/23`

### 🤝 Comunicazione Diretta
- Host A e B comunicano **direttamente** se nella stessa subnet
- Altrimenti, serve **router** (dipende dai rispettivi prefix)

### 🛠 ARP e Multicast
- ARP: scoperta indirizzi MAC (solo LAN)
- Multicast:
  - Range: `224.0.0.0 – 239.255.255.255` (classe D)
  - IGMP gestisce l'iscrizione ai gruppi multicast

---

## ✅ 2. IPv6 – Concetti Chiave

### 📌 Indirizzi IPv6
- 128 bit, formato esadecimale
- **Tipi principali**:
  - **Unicast**: un destinatario (es. `2000::/3`)
  - **Link-Local**: solo LAN (`FE80::/10`)
  - **Loopback**: `::1`
  - **Unspecified**: `::`
  - **Multicast**: `FF00::/8`
  - **Anycast**: instradato all’interfaccia più vicina
  - **Unique Local**: privati (`FC00::/7`)
  - **Embedded IPv4**: `::/80` (transizione)

### 🧱 Header IPv6
- **Lunghezza fissa**, più semplice di IPv4
- Rimossi: `checksum`, `frammentazione`, `header length`
- **Extension headers** concatenabili (es. autenticazione)

### 🔄 Autoconfigurazione
- **Stateless**:
  - Host genera da solo indirizzi (Link-Local e Global Unicast)
  - Usa `Router Advertisement` per ottenere prefisso
  - Interface ID derivato da MAC (EUI-64) o casuale (Privacy Extensions)
  - Usa **Duplicate Address Detection (DAD)**
- **Stateful**:
  - Tramite **DHCPv6**
- **Manuale**:
  - Impostazione da amministratore

### 📡 ICMPv6
- Sostituisce ARP e IGMP
- Tipi principali:
  - **Neighbor Solicitation / Advertisement** → scoperta MAC
  - **Router Solicitation / Advertisement** → scoperta rete
- Usa **Solicited-Node Multicast Address** (`FF02::1:FFXX:XXXX`)

### 🔁 Transizione IPv4 → IPv6
- **Dual Stack**: coesistenza IPv4 e IPv6
- **Tunneling**:
  - Incapsulamento (es. `6to4`, `Teredo`, `MAP-E`)
- **Translation**:
  - Conversione diretta (es. `NAT64`, `DS-Lite`)
- **Address + Port (A+P)**:
  - Più utenti condividono lo stesso IPv4 con porte diverse

---

## 📶 Capitolo 3 – Reti Wireless e Cellulari

### 📡 IEEE 802.11 (Wi-Fi)
- Standard per LAN wireless (reti WiFi)
- Protocollo MAC: **CSMA/CA** (Collision Avoidance, ma non elimina le collisioni)
- Frame può contenere **4 indirizzi MAC**, uno è l’Access Point

#### 🔁 Collisioni
- Collisione implicita: il mittente **non riceve l'ACK** → capisce che c'è stata collisione
- **Hidden terminal problem**: due nodi che non si vedono tra loro interferiscono col destinatario comune

---

### ⚙️ Tecniche di accesso multiplo
- **FDMA** (Frequency Division): ogni utente ha una frequenza diversa
- **TDMA** (Time Division): ogni utente trasmette in uno slot temporale
- **CDMA** (Code Division): ogni utente usa un codice diverso
- **SDMA** (Space Division): usa la posizione spaziale (es. antenne direzionali)

---

### 🗺️ Reti Cellulari: Concetti Base

#### 📡 Riutilizzo della frequenza
- **R** = raggio della cella  
- **G** = dimensione cluster (celle adiacenti con frequenze uniche)
- ↓R → ↑capacità, ↑costo (più antenne)
- ↑G → ↓capacità, ↑qualità (meno interferenze)

---

### 📲 Gestione della Mobilità

| Procedura         | Descrizione |
|------------------|-------------|
| **Roaming**       | Tracciamento del terminale in qualsiasi cella |
| **Location Updating** | Terminale comunica alla rete dove si trova |
| **Paging**        | La rete cerca un terminale per una chiamata o messaggio |
| **Handover**      | Passaggio automatico tra celle durante una chiamata |

- **Effetto ping-pong**: passaggi continui tra due celle con segnale simile

---

### 📶 Generazioni Cellulari

| Generazione | Tecnologia        | Accesso       | Tipo     |
|-------------|-------------------|---------------|----------|
| 1G          | AMPS              | FDMA          | Analogico|
| 2G          | GSM, GPRS, EDGE   | FDMA, TDMA, CDMA | Digitale |
| 3G          | UMTS, HSPA        | WCDMA         | Digitale |
| 4G          | LTE               | OFDMA         | Digitale |
| 5G          | NR                | OFDMA         | Digitale |

---

### 🔧 GSM/UMTS/LTE – Dettagli Tecnici

#### 📎 Bursts (GSM)
- Blocchi di dati trasmessi su rete a commutazione di circuito
- Tipi: **Regular, Access, Synchronization, Frequency Correction, Dummy**

#### 🔄 Frequency Hopping
- Trasmette su **frequenze diverse** → migliora qualità, ma riduce capacità

#### 📡 Frequency Reuse
- Riutilizzo della stessa frequenza in celle **sufficientemente distanti**

#### 🧠 BSC (Base Station Controller)
- Gestisce più BTS
- Non contiene dati utente

#### ⏱ Timing Advance
- Terminale inizia a trasmettere **prima** dello slot assegnato per compensare ritardo

#### 📡 RACH (Random Access Channel)
- Terminale invia richiesta di canale usando tecnica Slotted-Aloha

---

### 🧩 Architettura LTE (4G)
- **All-IP**: tutto è packet switching
- Accesso tramite **eNodeB**
- Core network:
  - **SGW (Serving Gateway)**: instradamento pacchetti
  - **PGW (Packet Gateway)**: connessione alla rete esterna
  - **MME (Mobility Management Entity)**: gestione mobilità
- Uso di **tunnel IP** per mobilità

---

### 🚀 5G – Concetti Chiave

#### 🧠 Caratteristiche principali
- **Network Slicing**: divisione logica delle risorse
- **Edge Computing**: elaborazione locale dei dati → bassa latenza
- **Massive MIMO**: beamforming orizzontale/verticale, molte antenne

#### 📊 Scenari d’uso (3P)
- **eMBB**: enhanced Mobile Broadband → 10–20 Gbps, fino a 500 km/h
- **mMTC**: massive Machine Type Communications → 1M dispositivi/km²
- **URLLC**: Ultra-Reliable Low Latency Communications → <5 ms, 99.999%

#### 🔬 Frequenze e tecnologie
- **FR1**: 450 MHz – 6 GHz
- **Tecnologia**: Filtered OFDM
- **Problemi mmWave**: path loss, blockage


## 🖧 Capitolo 4 – LAN Design

### 📦 Ethernet

#### 🔁 Modalità di trasmissione
- **Half duplex**: invio o ricezione alternati, non contemporanei
- **Full duplex**: invio e ricezione simultanei

#### 📄 Filtering Database
- Contiene posizione (porta) dei MAC address appresi, con aging time (default 300s)
- Scopo: **filtrare il traffico inutile**
- Può essere aggiornato manualmente o tramite **backward learning**

#### 🔄 Collisioni
- Possibili in modalità **switched half duplex**
- Non avvengono in **switched full duplex**

---

### 🧩 VLAN (Virtual LAN)

#### 🏷 Tag VLAN
- Campo di **4 byte** nella trama Ethernet con il VLAN ID
- Permette di distinguere i pacchetti tra VLAN diverse

#### 🔌 Tipi di porte su switch
- **Access**: invia/riceve solo trame non taggate → per host finali
- **Trunk**: invia/riceve trame taggate → per collegamenti tra switch, router, server

#### 🧰 Funzionalità VLAN
- Creano **LAN virtuali** su una sola infrastruttura fisica
- Isolano i domini di broadcast → il traffico rimane nella VLAN
- Possono essere configurate per **limitare o permettere** la comunicazione tra VLAN tramite router

#### 📈 Impatti delle VLAN
- ↑ numero VLAN → ↑ domini di broadcast
- VLAN ≠ crittografia → migliorano organizzazione, **non la sicurezza**
- Comunicazione tra VLAN → richiede routing (es. **One Arm Router**)

---

### 📌 Domande chiave

- Trame possono collidere in **switched half duplex**
- Entry del filtering database → **scadenza configurabile**
- Il MAC dello switch è usato **solo per comunicazioni dirette con lo switch**
- VLAN: **C) LAN virtuali su singola infrastruttura fisica**
- Le moderne LAN usano **switch + VLAN**
- VLAN → **limita il traffico broadcast**
- Porta Access → **assegna i pacchetti a una VLAN specifica**
- Porta Trunk → **invia pacchetti taggati da più VLAN**
- Due host su VLAN diverse: possono comunicare solo se **configurati correttamente**
- Tabella filtering VLAN: **aggiornabile manualmente o automaticamente**


## 🚦 Capitolo 5 – Routing

### 🎯 Tipologie di forwarding
- **Tramite indirizzo**: basato sulla destinazione (più comune)
- **Label Swapping**: usato in MPLS (Multi-Protocol Label Switching)
- **Source Routing**: il mittente specifica l’intero percorso (raro, richiede conoscenza della topologia)

---

### 🔁 Tipologie di routing

#### 📌 Routing statico (non adattivo)
- Nessuna variazione durante l'esecuzione
- Es.: **Flooding**, **Selective Flooding**

#### 🔄 Routing dinamico (adattivo)
- Si adatta in base a informazioni di rete in tempo reale
- **Centralizzato**: un RCC (Routing Control Center) guida gli altri nodi
- **Isolato**: ogni nodo opera senza scambio di info (es. Backward Learning)
- **Distribuito**: collaborazione tra nodi (es. Distance Vector, Link State)

---

### 🧠 Algoritmi dinamici

| Algoritmo       | Metodo              | Protocolli        | Cicli         | Costo computazionale     |
|----------------|---------------------|-------------------|---------------|---------------------------|
| Distance Vector| Bellman-Ford        | RIP, IGRP         | Facili        | O(l × n)                  |
| Link State     | Dijkstra            | OSPF, IS-IS       | Rari          | O(l × log(n))             |
| Path Vector    | Info su router/AS   | BGP               | Possibili     | Elevato                   |

---

### ❌ Problemi del Distance Vector
- **Black hole**: nodo che non inoltra più
- **Count to infinity**: loop infiniti
- **Bouncing effect**: scelta subottima del path

#### 🛠 Soluzioni:
- **Split Horizon**: impedisce cicli 2-nodi
- **Path Hold Down**: quarantena temporanea dopo errore
- **Route Poisoning**: route errate a costo infinito

---

### 🧪 Quiz importanti

#### Forwarding vs Routing
**A)** Forwarding: uscita migliore. Routing: calcolo percorso. ✅

#### Source Routing
**B)** Il mittente deve conoscere la topologia. ✅

#### Static Routing
**D)** Configurazione manuale da amministratore. ✅

#### Non adattivo
**A)** Selective Flooding non è adattivo. ✅

#### Selective Flooding
**C)** Riduce il numero di pacchetti duplicati. ✅

#### Routing dinamico + VLSM
**B)** È necessaria la subnet mask nei messaggi. ✅

#### Centralized Routing
**D)** Un nodo calcola le route per gli altri. ✅

#### Svantaggi Centralizzato
**A)** Nodo centrale è un punto critico. ✅

#### Routing Isolato
**D)** Calcolo basato solo sul traffico osservato. ✅

#### Periodo di transitorio
**D)** Sempre dopo un cambiamento topologico. ✅

#### Flooding in topologia ad anello
**D)** Due copie al nodo destinatario. ✅

#### Flooding con link punto-punto
**D)** Numero di copie dipende dal TTL. ✅

#### Redistribuzione Routing
**A)** Scambio tra protocolli → possibile perdita info. ✅  
**C)** Scambio tra domini diversi (es. OSPF ↔ BGP). ✅

#### Metrica di routing
**A)** Peso usato nel calcolo del percorso. ✅

---

### 🧭 Distance Vector approfondito

#### Loop e Count to Infinity
**C)** Split Horizon riduce il rischio in reti senza maglie. ✅  
**B)** Count to Infinity → non è una funzionalità utile. ✅

#### RIP
- **C)** Instabile, rischia loop. ✅
- **B)** RIP usa UDP per semplicità. ✅
- **A)** Scambia info con router vicini. ✅
- **C)** Usa Split Horizon e Hold Down. ✅

---

### 🔍 Split Horizon

**C)** Non annunciare un prefisso al next hop verso di esso. ✅  
**C)** Riduce la probabilità di loop. ✅

---

### 📉 RIP vs IGRP
**A)** Metrica RIP meno affidabile. ✅

---

### 🗺 Link State

**D)** Fase finale → esegue Shortest Path First. ✅  
**A)** I loop sono possibili ma gestibili. ✅  
**B)** Router OSPF mantiene topologia area + sommari. ✅  
**A)** LSA Router Link → connessioni con router adiacenti. ✅  
**B)** Topologia LAN OSPF → forma a stella. ✅  
**A)** Link State Update → trasporta LSA completi. ✅  
**B**, **D)** Database Description → fase di allineamento, non sempre cifrato. ✅  
**A)** OSPF può usare metriche personalizzate. ✅  
**D)** A regime, router OSPF conosce solo la propria area. ✅  
**D)** Network Link LSA → reti transito nella propria area. ✅  
**D)** ABR conosce i dettagli della backbone. ✅  
**B)** OSPF è gerarchico, non proprietario. ✅  
**D)** Vantaggio Link State: ogni router calcola autonomamente. ✅  
**B)** Differenza chiave: Link State invia info topologia locale. ✅  
**A)** RIP non è Link State → è Distance Vector. ✅

---

### 🧭 Path Vector e BGP

**D)** Risolve Count to Infinity. ✅  
**B)** Ogni record ha destinazione + hop traversati. ✅  
**D)** BGP → elenco AS da attraversare. ✅  
**C)** Policy > topologia in BGP. ✅  
**C)** BGP sceglie path anche in base a policy. ✅  
**C)** BGP → tra router di AS diversi. ✅

---

### 🌐 Autonomous System (AS)

- Gruppo di sottoreti con gestione autonoma
- Identificato da ID a 2 byte (assegnato da IANA)
- AS decidono il routing interno/politiche
- **Border Gateway**: router che collega AS
- **NAP (IXP)**: punto neutro L2 per interconnessione AS

#### Tipi di AS
- **Tier 1**: backbone, solo peering privato
- **Tier 2**: ISP regionali
- **Tier 3**: ISP locali

---

### 🌍 Protocolli di routing

#### IGP (Interior Gateway Protocol)
- Dentro l’AS
- Metriche e multipath
- Algoritmi: RIP, IGRP, OSPF, IS-IS

#### EGP (Exterior Gateway Protocol)
- Tra AS
- Basato su policy
- Algoritmo: BGP

---

### 📘 Quiz su AS e routing inter-dominio

- **A)** Un AS è una zona amministrata in autonomia. ✅  
- **B)** AS = gruppo subnet gestite da un'organizzazione. ✅  
- **A)** Falso: non è una subnet con routing statico. ✅  
- **B)** NAP → AS connessi a livello 2. ✅  
- **B)** IXP = LAN tra router AS. ✅  
- **D)** Tier-1 solo con peering. ✅  
- **D)** AS può bloccare il transito impostando policy + ACL. ✅  
- **C)** Inter-dominio routing → scelte coerenti con accordi. ✅  
- **D)** Peering = collegamento router di ISP diversi. ✅  
- **C)** Multipath può generare loop se percorsi con costi diversi. ✅

## 🧠 Capitolo 6 – SDN e NFV

### 🔧 SDN – Software Defined Networks

#### ✅ Principi fondamentali
- Separazione fisica tra **piano di controllo** e **piano dati**
- Piano dati semplificato (plumbing)
- Piano di controllo centralizzato (fisicamente o logicamente)
- Forwarding basato sul **contesto** (traffico attuale)

#### 🧩 OpenFlow
- Primo protocollo a implementare SDN
- Supporta i punti 1–3 e potenzialmente anche il 4

#### 🧠 SDN Controller
- Software centrale per la gestione dell’intera rete

#### 🔄 Service Function Chain (SFC)
- Catena di funzioni di rete (es. NAT, firewall)
- Difficili da ottimizzare, poco flessibili
- **Soluzione SDN**: uso di OpenFlow per dirigere dinamicamente il traffico

---

### ❓ Quiz – SDN

| Domanda | Risposta corretta |
|--------|--------------------|
| Problema reti tradizionali? | B) Schemi di traffico complessi |
| Limitazione architetture tradizionali? | A) Scalabilità |
| Caratteristica separazione piani? | B) Logica in software |
| Vantaggio switch SDN? | B) Interoperabilità standard |
| Non pilastro SDN? | B) Piano dati complesso |
| Vantaggio SDN? | B) Riduzione costi switch |
| Cos’è il “big switch”? | B) Controllo centralizzato astratto |
| Problema architettura distribuita? | C) Teorema CAP |
| Problema CAP in SDN? | B) Non si possono avere consistenza, disponibilità e tolleranza insieme |
| Primo protocollo SDN? | B) OpenFlow |
| Azione comune OpenFlow? | A) Forwarding a bit-rate specifici |
| Azione non supportata SDN? | C) Instradamento L7 |
| Vantaggio configurazione reattiva? | B) Efficienza uso flow table |
| Differenza proattivo vs reattivo? | B) Regole pre-popolate |
| Funzione Flow Table? | A) Instradamento con priorità |
| Cos’è il match? | B) Confronto pacchetti in ingresso |
| Cos’è il cookie? | A) Campo di controllo del flusso |
| Northbound Interface? | B) Interfaccia verso servizi esterni |
| OpenDaylight? | B) SDN Controller open source |
| Problema SFC? | A) Uso hardware inefficiente |
| Soluzione SDN per SFC? | B) Routing via software |

---

### 🧬 NFV – Network Functions Virtualization

#### ⚙️ Concetti chiave
- Funzioni di rete virtualizzate (eseguite su VM, cloud)
- Separazione hardware/software
- Infrastruttura standard (no hardware dedicato)
- **Complementare a SDN**

#### 🧱 Componenti principali
- **VNF** (Virtualized Network Function)
- **VNF Manager** (gestione ciclo di vita)
- Hypervisor

---

### ❓ Quiz – NFV

| Domanda | Risposta corretta |
|--------|--------------------|
| Standard NFV? | B) ETSI |
| Cos'è NFV? | B) Funzioni di rete su hardware standard |
| Non vantaggio NFV? | C) Dipendenza dai vendor |
| Componente principale? | B) VNF |
| SFG (Service Function Graph)? | B) Sequenza di VNFs per servizio |
| Tecnologia con hypervisor? | B) NFV |
| Funzione virtualizzazione? | B) Esecuzione VNFs via hypervisor |
| VNF Manager? | B) Gestione ciclo di vita del VNF |
## Capitolo 7 – Reti Ottiche

### 🧠 Concetti chiave

- **Reti ottiche**: reti basate su collegamenti in fibra ottica che commutano canali ottici da una fibra di ingresso a una di uscita.
- **Wavelength Division Multiplexing (WDM)**: tecnica per multiplare segnali ottici di diverse lunghezze d’onda sulla stessa fibra.
  - **DWDM** (Dense WDM): alta densità di segnali, maggiore capacità.
  - **CWDM** (Coarse WDM): meno canali, più economico.
- **Switch ottici**: dispositivi che commutano i segnali ottici **senza convertirli** in segnali elettrici.

### ❓ Quiz – Reti Ottiche

| Domanda | Risposta corretta |
|--------|--------------------|
| Vantaggio fibra ottica (1952) | A) Switch ottici performanti e bassa complessità |
| Le reti ottiche si basano su (1900) | B) Commutazione ottica tra porte |
| Caratteristica distintiva (1982) | B) Commutare un canale ottico da ingresso a uscita |
| Funzione del DWDM | A) Multiplare/demultiplare su una sola fibra |
| Operazione tipica switch ottico | B) Route di un canale ottico da input a output |
| Cosa commuta uno switch ottico | D) Canale ottico su una data lunghezza d’onda da ingresso a uscita |

---

## Capitolo 8 – MPLS

### 🔁 Funzionamento base

- **Label Switching Router (LSR)**: commutazione MPLS.
- **Label Edge Router (LER)**: gestione ingressi/uscite da MPLS.
- **Label Switched Path (LSP)**: percorso di etichette tra due punti.
- **Forwarding Equivalence Class (FEC)**: gruppo di pacchetti trattati nello stesso modo.

#### ⚙️ Operazioni su etichette

- **Label binding**: associazione etichetta-FEC.
- **Label mapping**: inserimento in tabella di forwarding.
- **Label distribution**: diffusione delle etichette.
- **Label swapping**: sostituzione etichetta a ogni nodo.
- **Label stacking**: impilamento di etichette (fino a 20).

### 🔌 Protocolli usati

- Per l’assegnazione delle label: **BGP**, **RSVP-TE**
- Di routing: **OSPF**, **IS-IS**, **BGP**
- Estensioni **-TE** (Traffic Engineering) per vincoli specifici.

### 🌐 MPLS VPN e 6PE

- **CE**: customer edge router.
- **PE**: provider edge router, gestisce le etichette.
- **P**: router interni alla backbone MPLS.
- **6PE**: supporta IPv6 su backbone IPv4 con doppia etichetta (esterna IPv4, interna IPv6).

### ❓ Quiz – MPLS

| Domanda | Risposta corretta |
|--------|--------------------|
| Importanza MPLS (1899) | D) Traffic engineering |
| Motivazione edge labeling | B) End host non devono conoscere MPLS |
| Forwarding meno complesso | B) Match esatto su label |
| Traffic engineering in MPLS | D) Forwarding data plane ≠ routing control plane |
| Tecnologie multipath | C) Label Swapping |
| LSP creati da | D) Nodi di rete con mapping |
| LSP = FEC | C) ✅ |
| Setup LSP (2160) | C) Mapping |
| Layer2 + MPLS | D) Label nel livello 2 |
| VPN livello 3 su MPLS | C) Automazione e integrazione |
| Layer 3 VPN MPLS | A) Automazione e integrazione |
| VPN L3 e scalabilità | C) Elevata scalabilità |
| Due etichette in MPLS | B) Etichetta esterna verso router PE |
| VPN con MPLS peer model | D) BGP modificato |
| 6PE – etichetta esterna | A) Verso PE |
| 6PE – architettura | A) PE dual stack |
| Protocollo assegnazione label | D) RSVP-TE |
| Distribuzione label | A) BGP |
| Label distribution protocol | D) BGP |
| RSVP e distribuzione | B) RSVP |
| Label binding control-driven | C) LSP per ogni destinazione |
| OSPF-TE / IS-IS-TE | C) Constraint-based routing |

## 9. VPN (Virtual Private Network)

### Tipologie di VPN
- **Site to Site (S2S):** tunnel tra due reti remote.
- **End to End (E2E):** tunnel tra terminali remoti.
- **Accesso / Virtual Dial-up:** terminale ↔ rete (es. smart working).

### Protocolli
- **PPTP:** livello 2, crittografia debole.
- **L2TP:** migliorato rispetto a PPTP, usa IPsec per sicurezza.

### Distribuzione
- **Intranet:** uffici remoti di una stessa azienda.
- **Extranet:** interconnessione tra aziende diverse.

### Modelli
- **Overlay:** rete parallela, meno costosa, routing peggiorato.
- **Peer:** routing migliorato, sicurezza inferiore.

### Gestione
- **Customer Provisioned:** il cliente gestisce i tunnel (CE).
- **Provider Provisioned:** il provider gestisce la VPN (PE).

### GRE
- Protocollo di tunneling di livello 3 (incapsula qualsiasi pacchetto).
- Supporta due intestazioni IP (esterna + interna).

### IPsec
- Autenticazione + cifratura tra VPN gateway.
- Modalità:
  - **Transport:** header IP solo autenticato.
  - **Tunnel:** tutto il pacchetto interno è cifrato e incapsulato.

### SSL
- Livello 4 (TCP/UDP), opera in user space.
- Più semplice e sicuro rispetto a IPsec ma vulnerabile ai DoS.

### Quiz VPN
- VPN → A
- Accesso centralizzato routing non ottimale → A
- Messaggi esterni passano dal sito VPN → A
- Traffico passa da VPN gateway → D
- VPN Accesso più diffuso: tunneling su IP → A
- PPTP → D
- L2TP no crittografia nativa → D
- Overlay: ISP ignora la VPN → D
- Extranet → C
- Customer provisioned → D
- Cifratura non sempre → B
- VPN end-to-end ≠ sempre meglio → A
- GRE → A, B, D
- Due intestazioni IP → B
- Pacchetto in tunnel GRE → D
- IPsec: tunnel su rete pubblica → D
- IPsec e NAT → C
- IPSec VPN → C

---

## 10. Quality of Service (QoS)

### Meccanismi
- **Traffic Shaping / Policing:** Token Bucket, WFQ.
- **RSVP:** prenotazione risorse a livello IP.
- **Scheduling:** ordine pacchetti nei router.

### Token Bucket
- Accumula token a rate `r` fino a `B`.
- Controlla burst massimo e data rate.

### DiffServ vs IntServ

| Caratteristica | IntServ | DiffServ |
|----------------|---------|----------|
| Complessità    | Alta    | Bassa    |
| Efficienza     | Alta    | Bassa    |
| Scalabilità    | Bassa   | Alta     |
| Prenotazione   | Sì (RSVP) | No     |

### Quiz QoS
- Policing → A
- RSVP → A
- Scheduling → D
- QoS: nodi applicano regolazioni → B
- Token Bucket → D
- Token Bucket (r, B) → A
- Token Bucket + WFQ → B
- IntServ → A
- DiffServ → C
- DiffServ ≠ QoS → B
- DiffServ + TE → D

---

## 11. CDN (Content Delivery Network)

### Concetti
- **Overlay Network** su Internet.
- **Content Server**: cache distribuite.
- **Reindirizzamento:**
  - **IP Anycast**
  - **DNS Selection**
  - **HTTP Redirect**

### Quiz CDN
- Edge caching → C
- Cache: riducono latenza → A
- Proxy caching ≠ CDN → D
- CDN ≠ proxy caching → B
- Proxy trasparente e SSL incompatibile → B
- Motivi uso CDN → D
- CDN ≠ server farm → B
- Replica CDN → B
- Selezione server → D
- Reindirizzamento client → D
- DNS-based CDN → A
- Limiti DNS → D
- Akamai routing: DNS → A
- Akamai suddivide URL → A
- Akamaizzazione → B
- CDN nota → A
- Load Balancer (SLB) → A
- SLB in server farm → B
- Sfida replica → A

---

## 12. Vari

### Latenza in reti congestionate
- **Causa principale:** ritardo di accodamento (queueing delay) → **D**

### Cloud Delivery Models
- **IaaS:** massimo controllo utente → **B**
- **PaaS:** utente gestisce solo applicazioni.
- **SaaS:** tutto gestito dal provider.
