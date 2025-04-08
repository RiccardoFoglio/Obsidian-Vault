Sistemi particellari gestiscono tanti oggetti, devono essere di forma semplice

- forme semplici
- motion semplice
- ignora collisioni tra particelle

Calcoli per frame
1. importante terminare le particelle scadute
2. aggiornare gli attributi secondo le procedure di controllo
3. creare nuove particelle e assegnare gli attributi
4. renderizzare le particelle

![[Screenshot 2025-04-08 at 5.58.00 PM.png|500]]

Generazione delle particelle
- processo randomico controllato
- distribuzione uniforme nello spazio e tempo
- sincronizzazione della generazione e terminazione per tenere il numero di particelle vive circa costante

Attributi delle particelle:
- posizione
- velocità
- forma
- display attributes
- life expectancy

Sistema particellare = array di posizione, velocità, forza accumulata, massa

Aggiornamento del sistema particellare:
- pulisce le forze
- genera nuova forza cumulata
- calcola accelerazione e velocità
- aggiorna posizione

FIsica delle particelle:
- gravità, viscosità
- ambientali: forze di repulsione, reazione alle collisioni (kill al contatto con qualcosa, tipo la pioggia)


meglio integrare con eulero, assumere che l'accelerazione è costante semplifica il problema

"che cos'è un sistema particellare" : simula la fisica di molti membri (1000+). La fisica è semplice newtoniana e poco quella browniana (polline), non c'è conoscenza reciproca del sistema particellare (tra loro possono collidere), la geometria se c'è è minimale. non si riesce a identificare un comportamento locale delle singole particelle, ma solo quello globale

## Flocking

oggetti geometrici, molti oggetti, motion semplice, considera altri membri
FOV limitato, modificato dalla velocità
importanza di distanza, prossimità e angolo di approccio
evitare di scontrarsi coi vicini
stare vicino ai vicini
matchare la velocità dei vicini per evitare collisioni

Impulso migratorio --> lo stormo andrà in una direzione
si designa un centro 
si segue il leader
non realistico ma facilita il controllo

![[Screenshot 2025-04-08 at 6.42.54 PM.png]]

