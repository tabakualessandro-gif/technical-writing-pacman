📅 Verbale di Riunione

### 📄 Dettagli della Riunione

* **Progetto:** Simulazione Web di Pac-Man (Clone Arcade)
* **Data:** 10/11/2025
* **Ora:** 15:00 CET
* **Partecipante:** Alessandro
* **Oggetto:** Revisione PRD e Analisi Tecnica; Definizione delle Priorità e Architettura.

---

### 💡 Punti Discusse

La riunione si è concentrata sulla validazione dei documenti di analisi tecnica e funzionale precedentemente preparati e sulla definizione della strategia di implementazione per l'MVP (Minimum Viable Product).

1.  **Validazione Documenti:** I documenti PRD, Analisi Funzionale e Analisi Tecnica sono stati approvati come base per lo sviluppo.
2.  **Scelta Tecnologica:** Confermata la scelta di utilizzare **Canvas API** in HTML5 per il rendering del labirinto e dei personaggi, al posto di una griglia DOM/CSS, per garantire fluidità e performance.
3.  **Priorità MVP (Fase 1):** È stato stabilito che l'MVP si concentrerà rigorosamente sui componenti fondamentali del gioco per una rapida prototipazione. 
4.  **Architettura del Codice:** Confermata la struttura **Modulo-Centrica** con l'uso di classi ES6 e il pattern **Game Loop** basato su `requestAnimationFrame()`.

---

### ✅ Decisioni Prese

| ID Decisione | Decisione | Impatto/Motivazione |
| :--- | :--- | :--- |
| **D.01** | **Rendering via Canvas API:** Abbandonata l'opzione DOM/CSS Grid. | Migliore performance e flessibilità per animazioni sub-pixel e rendering fluido. |
| **D.02** | **Definizione MVP Core:** L'MVP deve includere: Labirinto, Pac-Man con movimento e raccolta Punti Standard, **un solo fantasma** (Blinky) con AI in modalità **Chase**, e logica di perdita vita/respawn. | Focalizzazione sulla funzionalità di base prima di affrontare l'AI complessa e le dinamiche Power Pellet. |
| **D.03** | **Struttura Dati Mappa:** La matrice del labirinto (`MAP`) sarà l'unica fonte di verità per la presenza di Muri, Punti e Power Pellets. | Semplificazione della logica di collisione e raccolta. |
| **D.04** | **Input/Output:** L'input utente sarà gestito tramite un **buffer di direzione** (`nextDirection`) in `pacman.js` per implementare correttamente la meccanica del *cornering*. | Necessario per replicare la sensazione arcade originale di pre-programmazione della svolta. |

---

### 🛠️ Action Items

Gli *action item* sono assegnati ad Alessandro per l'inizio della fase di sviluppo.

| ID Task | Action Item | Assegnatario | Deadline Target |
| :--- | :--- | :--- | :--- |
| **A.01** | **Setup del Progetto:** Creare la struttura base delle cartelle e i file (index.html, style.css, main.js, game\_data.js, renderer.js). | Alessandro | 11/11/2025 |
| **A.02** | **Implementazione Rendering Statico:** Scrivere il modulo `renderer.js` e `game_data.js` per disegnare il **labirinto statico** (solo muri) sul Canvas utilizzando la matrice `MAP`. | Alessandro | 12/11/2025 |
| **A.03** | **Implementazione Pac-Man:** Creare la classe `PacMan` in `pacman.js` e integrarla nel `Game Loop` per gestire il movimento basato sull'input utente e la logica di collisione con i muri (D.04). | Alessandro | 13/11/2025 |
| **A.04** | **Raccolta Punti:** Estendere la logica di `PacMan.update()` per rilevare e rimuovere i **Pac-Dots** dalla matrice `MAP` e aggiornare lo score tramite `Renderer.updateHUD()`. | Alessandro | 14/11/2025 |