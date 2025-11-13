📝 Analisi Funzionale: Logica di Gioco e Comportamento Utente

### 1. Interfaccia Utente (Cosa vedrà l'utente)

L'utente visualizzerà l'ambiente di gioco su una singola schermata, strutturata come segue:

#### 1.1. Area di Gioco (Il Labirinto)

* **Labirinto:** Una griglia visivamente distinta, composta da muri (in genere blu scuro o neri) che definiscono i percorsi.
* **Pac-Dots:** Piccoli punti sparsi lungo tutti i percorsi (punti standard).
* **Power Pellets (Palline Energia):** Punti visibilmente più grandi, strategicamente posizionati.
* **Personaggi:**
    * **Pac-Man:** Il protagonista giallo, sempre in movimento (anche se animato sul posto).
    * **Fantasmi:** I quattro nemici di colore distintivo (Rosso - Blinky, Rosa - Pinky, Ciano - Inky, Arancione - Clyde).
* **Tunnel:** Due aperture laterali che collegano il lato sinistro del labirinto con il lato destro (teletrasporto).

#### 1.2. Pannello di Stato (HUD)

Questa area, di solito nella parte superiore o inferiore dello schermo, fornirà informazioni cruciali:

* **Punteggio Attuale:** Visualizzazione dinamica dei punti accumulati.
* **High Score (Punteggio Migliore):** Visualizzazione del miglior punteggio registrato.
* **Vite Rimanenti:** Una serie di icone di Pac-Man che indicano le vite residue.
* **Messaggi di Stato:**
    * **"READY!"**: Mostrato all'inizio di ogni vita o livello.
    * **"GAME OVER"**: Mostrato quando le vite terminano.

---

### 2. Comportamento Utente (Cosa farà l'utente)

L'interazione dell'utente è semplice e si basa sul controllo del movimento del protagonista.

| Azione Utente | Tasto/Input | Funzione |
| :--- | :--- | :--- |
| **Muovi Pac-Man** | Tasti Freccia (↑, ↓, ←, →) o WASD | Controlla la direzione di movimento di Pac-Man. Il movimento è ritardato fino a quando Pac-Man non raggiunge la prossima casella di griglia nella direzione richiesta. |
| **Avvia/Ricomincia** | Tasto Spazio o Invio | Avvia il gioco dalla schermata iniziale o ricomincia dopo un "Game Over". |
| **Pausa** | Tasto 'P' (Opzionale) | Mette in pausa il gioco e i movimenti. |

---

### 3. Logiche di Gioco (Funzionalità e Regole)

#### 3.1. Logica di Movimento e Collisione

| Oggetto | Logica Funzionale |
| :--- | :--- |
| **Pac-Man** | L'utente può pre-programmare una direzione (buffer di input). Pac-Man cambierà direzione solo quando raggiunge il centro di una casella di griglia e la nuova direzione non lo fa scontrare contro un muro. |
| **Fantasmi** | Si muovono autonomamente lungo i percorsi del labirinto. Quando raggiungono un incrocio (dove sono possibili due o più svolte), scelgono il percorso in base al loro obiettivo AI (Chase, Scatter o Frightened). **Non è permesso invertire la direzione** se non specificatamente richiesto dal cambio di modalità (es. da Chase a Frightened). |
| **Muri** | Qualsiasi tentativo di movimento contro un muro (sia di Pac-Man che dei Fantasmi) viene bloccato. |
| **Tunnel** | Se Pac-Man o un Fantasma entra in un'estremità del tunnel laterale, riappare immediatamente all'estremità opposta. |

#### 3.2. Logica di Punteggio e Raccolta

| Oggetto Mangiato | Punti Ottenuti | Effetto sul Gioco |
| :--- | :--- | :--- |
| **Pac-Dot** | 10 Punti | Rimosso dal labirinto. |
| **Power Pellet** | 50 Punti | Rimosso dal labirinto. **Tutti** i fantasmi cambiano immediatamente modalità in **Frightened** (diventano blu) e invertono la direzione. |
| **Fantasma Blu** | 200, 400, 800, 1600... | Il fantasma "mangiato" viene trasformato negli **occhi** e torna rapidamente alla Ghost House centrale per rigenerarsi (respawn). Il contatore dei punti mangiati si resetta allo scadere dell'effetto Power Pellet. |
| **Frutta Bonus** (Opzionale) | Variabile (100 - 5000) | Appare solo per un tempo limitato al centro del labirinto. |

#### 3.3. Logica di Collisione Pac-Man / Fantasma

* **Collisione in modalità Chase/Scatter:** Se Pac-Man tocca un fantasma mentre è in modalità normale (Chase o Scatter), il giocatore perde una vita. I personaggi vengono resettati nelle loro posizioni iniziali.
* **Perdita di Vita:** Quando le vite raggiungono $0$, il messaggio "GAME OVER" appare e il gioco termina.

#### 3.4. Logica dei Fantasmi (Modalità AI)

I fantasmi ciclano attraverso tre modalità principali in base a intervalli di tempo predefiniti, interrotti solo dalle Power Pellets.

1.  **Chase (Caccia):** I fantasmi puntano attivamente verso la posizione target di Pac-Man (o una posizione ad essa correlata).
2.  **Scatter (Dispersione):** I fantasmi si muovono verso i loro angoli di destinazione assegnati, dando al giocatore un breve respiro.
3.  **Frightened (Fuga - Blu):** Questa modalità ha la priorità sulle altre. I fantasmi si muovono casualmente e fuggono se Pac-Man è vicino. La durata è a tempo, e quando scade, i fantasmi tornano alla loro modalità precedente e riacquistano il loro colore originale.

#### 3.5. Logica di Livello

* **Completamento:** Un livello viene completato quando l'utente ha mangiato **tutti** i Pac-Dots e le Power Pellets.
* **Transizione:** All'inizio di un nuovo livello, i personaggi vengono resettati, il labirinto viene ripopolato, e il gioco riprende con una velocità leggermente aumentata o con un ciclo AI modificato per aumentare la difficoltà.