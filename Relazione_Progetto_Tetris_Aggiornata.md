# Relazione di Progetto Aggiornata: Tetris 2D (Unity 6)

## 🎮 Stato del Gameplay e Logica di Gioco
* **Ciclo di Gioco (Game Loop)**: Completato al 100%. Il gioco ha un inizio (Menu), una progressione dinamica e una gestione della sconfitta fluida, centralizzata e priva di bug.
* **Controlli e Input standard**: Sistema di movimento reattivo tramite tastiera (Frecce Sinistra/Destra per spostare, Freccia Su per ruotare di 90°, Freccia Giù per accelerare la discesa). Gli input vengono congelati istantaneamente in caso di Game Over o Pausa.
* **Hard Drop (Discesa Istantanea)**: Nuova meccanica integrata tramite la **Barra Spaziatrice**. Permette al giocatore di teletrasportare istantaneamente il pezzo sul fondo della griglia, accelerando il ritmo di gioco e simulando il comportamento del Tetris moderno.
* **Rotazione a Muro (Wall Kick)**: Implementato un sistema di recupero della rotazione. Se un pezzo si trova adiacente ai muri o ad altre pile di blocchi e la rotazione standard fallisce, lo script effettua micro-spostamenti correttivi (destra, sinistra o alto) per incastrare il pezzo ed evitare il blocco degli input.
* **Sistema di Punteggio e Difficoltà**: Sistema progressivo stile *Tetris Classico* (1 riga = 100pt, 2 righe = 300pt, 3 righe = 500pt, 4 righe/Tetris = 800pt). La velocità aumenta dinamicamente riducendo il tempo di caduta globale da 1.0s fino a un minimo di 0.2s.
* **Meccanica Anteprima (Next Piece)**: Lo spawner gestisce in anticipo il blocco successivo, mostrandone una copia statica nell'area di anteprima a destra prima di immetterla nel tabellone di gioco.

## 🎨 Asset Grafici e Interfaccia Utente (UI)
* **Flusso delle Scene**: Il gioco è ora strutturato su più scene collegate tramite il `SceneManager`:
  * **`MainMenuScene`**: Scena iniziale con titolo e pulsante "GIOCA" funzionante per avviare la partita.
  * **`SampleScene`**: La scena di gioco principale in cui si svolge il gameplay.
* **Interfaccia UI (TextMeshPro)**: Canvas configurata con testo in tempo reale per i Punti e per l'indicatore del pezzo successivo.
* **Schermata di Pausa**: Nuovo pannello UI oscurato attivabile in partita tramite il tasto **ESC** o **P**. Interrompe il gameplay congelando il tempo di gioco (`Time.timeScale = 0`) e permette di riprendere o tornare al menu.
* **Schermata di Game Over**: Pannello UI oscurato con scritta "GAME OVER" e pulsante "RIPROVA" funzionante, che azzera il punteggio, ripristina il tempo di gioco (`Time.timeScale = 1`) e ricarica la scena da capo.

## 💻 Architettura del Codice (I 5 Script Principali)
1. **GridManager.cs**: Gestisce la matrice logica 10x20 del tabellone. Controlla i bordi, rileva le righe piene, le distrugge, applica lo slittamento dei blocchi superiori e calcola il punteggio combo incrementando la velocità globale.
2. **Spawner.cs**: Gestisce la coda di generazione dei pezzi (pezzo corrente e pezzo in anteprima). Comunica direttamente con il `GameManagerScript` se la posizione di spawn iniziale risulta occupata da altri blocchi.
3. **Tetromino.cs**: Gestisce il ciclo di vita del singolo blocco attivo (gravità, rotazioni con Wall Kick, spostamenti laterali e Hard Drop). Si disattiva automaticamente non appena il pezzo si ancora stabilmente al terreno o alla griglia.
4. **GameManagerScript.cs**: Implementato tramite pattern **Singleton** per l'accesso globale. Centralizza lo stato del gioco (Gameplay, Pausa, Game Over). Gestisce l'attivazione dei pannelli UI, il ripristino del tempo e il reset del punteggio all'avvio della partita.
5. **MainMenu.cs**: Script leggero assegnato alla scena del menu principale. Gestisce il caricamento della scena di gioco principale (`SampleScene`) tramite la pressione del pulsante UI e si occupa della chiusura dell'applicazione.