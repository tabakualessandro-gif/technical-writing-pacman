## 📄 PRD: Simulazione Web di Pac-Man

### 1\. Introduzione

  * **Nome del Progetto:** Pac-Man Web Clone
  * **Obiettivo:** Creare una simulazione del classico gioco arcade *Pac-Man* utilizzando le tecnologie web standard (HTML, CSS, JavaScript). L'obiettivo è replicare fedelmente l'esperienza di gioco originale, inclusi i movimenti dei personaggi, la logica del labirinto e l'Intelligenza Artificiale (AI) di base dei fantasmi.
  * **Destinatari:** Sviluppatori (per la comprensione del progetto), Utenti finali (giocatori).
  * **Tecnologie:** **HTML5**, **CSS3**, **Vanilla JavaScript** (senza framework esterni complessi).

-----

### 2\. Obiettivi e Risultati Chiave

| Obiettivo | Risultato Chiave (Success Criteria) |
| :--- | :--- |
| **Fedeltà di Gioco** | Implementare il ciclo di gioco completo (Mangia, Caccia, Punteggio, Vita). |
| **Funzionalità Base** | Pac-Man si muove correttamente nel labirinto in base all'input dell'utente. |
| **Implementazione AI** | I quattro fantasmi mostrano comportamenti distinti (**Inseguimento**, **Ambuscading**, **Fuga**). |
| **Esperienza Utente** | L'interfaccia utente mostra Punteggio, Vite e Messaggi di Stato (es. "Game Over"). |

-----

### 3\. Funzionalità Dettagliate

#### 3.1. Labirinto e Elementi di Gioco

  * **Struttura del Labirinto:** Il labirinto deve essere rappresentato da una **griglia** (matrice) in JavaScript per facilitare la logica di movimento e collisione.
  * **Punti e Energia:**
      * **Pac-Dots:** Punti normali (Score: 10 punti).
      * **Power Pellets (Palline Energia):** Punti grandi che consentono a Pac-Man di mangiare i fantasmi (Score: 50 punti). Attivano la modalità "Frightened" (Fuga) dei fantasmi.
  * **Frutta/Bonus:** Elementi bonus che appaiono in base al punteggio o al tempo (Score: Variabile, es. 100-5000 punti). (Funzionalità **Opzionale** in una Fase 1).

#### 3.2. Movimento e Collisioni

  * **Pac-Man:**
      * Movimento controllato dall'utente tramite i tasti freccia o **WASD**.
      * Deve rispettare la griglia e non può attraversare i muri.
      * Deve consumare i Pac-Dots e le Power Pellets al passaggio.
  * **Fantasmi:**
      * Movimento autonomo basato sull'AI.
      * Devono rispettare la griglia e non possono attraversare i muri.
      * **Tunnel:** Implementare i tunnel laterali che consentono ai personaggi di teletrasportarsi da un lato all'altro del labirinto.

#### 3.3. Logica dei Fantasmi (Intelligenza Artificiale)

Ogni fantasma deve avere un proprio target/comportamento, passando attraverso tre modalità principali:

1.  **Chase (Inseguimento):**
      * **Blinky (Rosso):** Insegue direttamente Pac-Man.
      * **Pinky (Rosa):** Tenta di posizionarsi davanti a Pac-Man (ambush/imboscata).
      * **Inky (Ciano):** La sua destinazione è basata sulla posizione di Pac-Man *e* Blinky (più complesso, coordinato).
      * **Clyde (Arancione):** Insegue Pac-Man quando è lontano, ma ritorna casualmente all'angolo inferiore sinistro quando è vicino (comportamento "dispersivo").
2.  **Scatter (Dispersione):** I fantasmi si ritirano nei rispettivi angoli del labirinto per brevi periodi.
3.  **Frightened (Fuga):** Attivata dalle Power Pellets. I fantasmi diventano blu e si muovono casualmente o fuggono da Pac-Man.

<!-- end list -->

  * **Collisioni Fantasma:**
      * **Chase/Scatter:** Se un fantasma tocca Pac-Man, Pac-Man perde una vita e i personaggi vengono resettati (respawn).
      * **Frightened:** Se Pac-Man tocca un fantasma, il fantasma viene "mangiato" e torna alla **Ghost House** centrale per rigenerarsi. Il punteggio aumenta (200, 400, 800, 1600...).

#### 3.4. Gestione del Gioco (Core Game Loop)

  * **Punteggio:** Sistema di punteggio funzionante.
  * **Vite:** Pac-Man inizia con 3 vite.
  * **Livello:** Un livello è completato quando tutti i Pac-Dots e le Power Pellets sono stati mangiati.
  * **Game Over:** Il gioco termina quando Pac-Man perde la sua ultima vita.
  * **Start/Pause:** Gestione dell'avvio e della pausa del gioco.

-----

### 4\. Interfaccia Utente (UI) e Grafica (CSS)

  * **Design:** Stile grafico in pixel art/retrò fedele all'originale.
  * **Display:** L'interfaccia deve mostrare chiaramente:
      * Punteggio Attuale.
      * Punteggio Migliore (High Score).
      * Vite rimanenti (icone Pac-Man).
      * Messaggi di Stato ("READY\!", "GAME OVER").

-----

### 5\. Requisiti Tecnici

  * **Mappatura:** Utilizzare una **matrice bidimensionale** in JavaScript (es. `0` per un muro, `1` per un Pac-Dot, `2` per uno spazio vuoto, etc.) per rappresentare il labirinto.
  * **Loop di Gioco:** Un loop di gioco basato su `requestAnimationFrame` per una grafica fluida e un controllo preciso della logica di gioco a intervalli fissi (tick di gioco).
  * **Rendering:** Utilizzo dell'**elemento Canvas** o una griglia di elementi **DIV/Grid CSS** per il rendering grafico del labirinto e dei personaggi.

-----

### 6\. Fase 1: MVP (Minimum Viable Product)

L'MVP si concentra sulla funzionalità di base:

1.  Rendering del **labirinto statico** (HTML/CSS).
2.  **Pac-Man in movimento** nel labirinto (JS) e gestione delle collisioni con i muri.
3.  **Raccolta dei Pac-Dots** (punti semplici) e aggiornamento del punteggio.
4.  Implementazione di **un solo fantasma** con AI di base (solo **Blinky** in modalità **Chase**).
5.  Gestione della **perdita di una vita** e del *respawn* di base.
