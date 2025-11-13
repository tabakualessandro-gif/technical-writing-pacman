Analisi Tecnica Dettagliata: Architettura, Pattern e Soluzioni

Questa sezione descrive come verrà strutturato e scritto il codice del progetto Pac-Man Web Clone, garantendo manutenibilità e chiarezza.

### 1\. Architettura del Codice: Modulo-Centrica

Il codice sarà suddiviso in moduli JavaScript (ES6 Modules) per separare la logica del gioco (core), i dati (mappa/config) e la presentazione (rendering/personaggi).

| File/Modulo | Ruolo Architetturale | Pattern Associato |
| :--- | :--- | :--- |
| **`main.js`** | **Controller/Orchestratore (Singleton)**. Gestisce l'inizializzazione, lo stato del gioco e il `Game Loop`. | Singleton |
| **`game_data.js`** | **Modello (Data Store)**. Contiene la matrice del labirinto, le costanti e lo stato dei punti mangiati. | Data Object/Config |
| **`pacman.js`** | **Modello/Componente (Component)**. Logica del protagonista, movimento e interazione con i punti. | Component |
| **`ghosts.js`** | **Modello/Componente (Component)**. Logica base di tutti i fantasmi e gestione degli stati (AI). | Component/Factory (se si creano fantasmi diversi) |
| **`renderer.js`** | **Vista (View)**. Gestisce tutti gli aspetti grafici sul Canvas (o DOM). Disegna il labirinto, i personaggi e l'HUD. | View/Drawer |

-----

### 2\. Pattern di Design Implementati

#### 2.1. Pattern Game Loop (Fondamentale)

Il cuore del gioco sarà basato sul pattern **Game Loop**, implementato tramite `window.requestAnimationFrame()`.

  * **Scopo:** Garantire che la logica di aggiornamento (update) e il rendering avvengano a una frequenza costante e sincronizzata con il browser, prevenendo *tearing* o *jutter*.
  * **Struttura (in `main.js`):**
    1.  `handleInput()`: Legge l'input dell'utente.
    2.  `update()`: Aggiorna lo stato del modello (posizione, collisioni, AI, punteggio). Deve essere **tempo-indipendente** (o usare un `deltaTime` se necessario, ma per un gioco a griglia può essere a *tick* fisso).
    3.  `render()`: Disegna la scena basandosi sullo stato aggiornato.

#### 2.2. Pattern Componente/Entità (Character Classes)

I personaggi (Pac-Man e Fantasmi) saranno implementati come **classi ES6** (Componente/Entità).

  * **Vantaggi:** Incapsulamento dello stato e del comportamento (`.move()`, `.checkCollision()`, `.update()`).
  * **Esempio:** La classe `Ghost` avrà metodi come `chase()`, `scatter()`, `frightened()` e una proprietà `targetCell` che guida il suo movimento.

#### 2.3. Pattern State (per i Fantasmi)

Per gestire il comportamento complesso dei fantasmi, si utilizzerà un semplice pattern **State**.

  * Ogni fantasma avrà una proprietà `this.currentState` che può essere `'chase'`, `'scatter'`, `'frightened'`, `'eaten'`.
  * Nel metodo `Ghost.update()`, una logica *switch* o *if/else* chiamerà il metodo corrispondente al suo stato:
    ```javascript
    // All'interno di Ghost.update()
    switch (this.currentState) {
        case 'chase':
            this.calculateChaseTarget();
            break;
        case 'frightened':
            this.moveRandomly(); // Logica specifica per la fuga
            break;
        // ...
    }
    ```

-----

### 3\. Soluzioni Tecniche e Implementazione (Il Codice)

#### 3.1. Gestione del Movimento su Griglia

Il movimento deve essere gestito a livello di cella e interpolato a livello di pixel per la fluidità.

  * **Logica (in `pacman.js` e `ghosts.js`):**
    1.  Il movimento avviene in **incrementi fissi** (es. 2 pixel/frame) lungo il percorso fino al centro della cella successiva.
    2.  Solo quando un personaggio è al **centro di una cella** (`pixelX` e `pixelY` sono multipli esatti della `CELL_SIZE`), può decidere (o essere costretto) a cambiare direzione.
    3.  Questo garantisce che i personaggi non si blocchino mai a metà strada tra le celle e che il *cornering* di Pac-Man (cambiare direzione prima di una curva) funzioni correttamente tramite il buffer `nextDirection`.

#### 3.2. Implementazione AI: Pathfinding basato su Target

Come descritto in precedenza, l'AI utilizzerà la **distanza di Manhattan** per il pathfinding.

  * **Passaggi (in `ghosts.js`):**
    1.  **Definizione del Target:** Il metodo `calculateTarget()` viene implementato in modo diverso per ogni fantasma (Blinky, Pinky, Inky, Clyde) per replicare il comportamento arcade.
    2.  **Calcolo Svolte:** Ad ogni incrocio, la funzione `getBestDirection(targetX, targetY)` in `ghosts.js` valuta le direzioni disponibili (escludendo il muro e l'inversione).
    3.  **Algoritmo di Scelta:** Per ogni direzione, calcola la distanza $D$:
        $$D = |next\_gridX - targetX| + |next\_gridY - targetY|$$
    4.  Sceglie la direzione con il $D$ **minore**.

#### 3.3. Rendering con Canvas

  * **Separazione:** Il modulo **`renderer.js`** sarà l'unico a interagire direttamente con l'API Canvas. Non ci saranno chiamate a `ctx.draw()` all'interno delle classi `PacMan` o `Ghost`.
  * **Efficienza:** Il Canvas deve essere **cancellato completamente** (`ctx.clearRect()`) e **ridisegnato** in ogni frame del `Game Loop` per gestire il movimento e le animazioni.

#### 3.4. Gestione della Griglia e Punti

La matrice `MAP` gestirà anche lo stato dei Pac-Dots.

  * **Raccolta:** Quando Pac-Man occupa una cella con un punto (`MAP[y][x] == 1` o `3`), la cella viene immediatamente aggiornata a spazio vuoto (`MAP[y][x] = 2`).
  * **Vantaggio:** Il rendering del labirinto legge direttamente lo stato di `MAP`, garantendo che i punti mangiati scompaiano dal Canvas al frame successivo.