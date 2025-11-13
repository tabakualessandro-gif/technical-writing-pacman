🤖 Linee Guida per Codice di Alta Qualità da AI Companion

### 1. Il Ruolo dell'AI Companion

L'AI è un **co-pilota** per la codifica, non un sostituto. Il suo valore risiede nell'accelerare i processi, generare boilerplate e fornire *starting points* complessi. L'obiettivo delle linee guida è trasformare il codice generato (spesso funzionale ma non ottimale) in codice **production-ready**.

---

### 2. Principi Fondamentali di Qualità

Questi principi devono essere applicati a ogni blocco di codice generato dall'AI, sia esso frontend, backend o infrastruttura.

#### 2.1. Chiarezza e Leggibilità

* **Nomenclatura Standard:** Assicurarsi che l'AI utilizzi nomi di variabili, funzioni e classi **descrittivi e autoesplicativi** (es. `calculateOrderTotal` invece di `calc`). Evitare l'uso di nomi generici come `temp`, `data` o `result` senza contesto.
* **Commenti Strategici:** L'AI tende a commentare eccessivamente o inutilmente. Mantenere solo commenti che spieghino la *logica di business* complessa o le scelte di implementazione **non ovvie**.
* **Formattazione Coerente:** Il codice deve aderire rigorosamente agli standard di formattazione del progetto (indentazione, spazi, interruzioni di riga). Integrare strumenti come **ESLint** o **Prettier** per automatizzare la pulizia post-AI.

#### 2.2. Robustezza e Gestione degli Errori

* **Validazione Input:** Tutti i dati provenienti dall'esterno (input utente, richieste API) devono essere **validati** sia nel frontend che nel backend (principio di **Zero Trust**).
* **Gestione Esplicita degli Errori:** Sostituire i blocchi `try...catch` generici (che spesso inghiottono gli errori) con una gestione esplicita. Loggare l'errore con contesto sufficiente e comunicare una risposta utile all'utente o al servizio chiamante.
* **Test di Unità:** Richiedere all'AI di generare test di unità **insieme al codice funzionale**. I test devono coprire i casi *happy path* e i *casi limite* critici (es. valori nulli, stringhe vuote, overflow).

#### 2.3. Manutenibilità e Architettura

* **Single Responsibility Principle (SRP):** Ogni funzione, classe o modulo generato deve avere **una sola ragione per cambiare**. Se l'AI crea una funzione monolitica, suddividerla in componenti più piccoli e focalizzati.
* **Separazione delle Preoccupazioni:** In Full Stack, questo significa assicurare che:
    * **Frontend:** La logica di presentazione sia separata dalla logica di stato (es. **React Components** gestiscono la UI, **Redux/Zustand** gestiscono lo stato).
    * **Backend:** La logica di **Routing**, **Business Logic** e **Data Access** (Repository/ORM) siano in livelli separati (es. architettura a **tre strati**).
* **Evitare Magic Numbers/Strings:** Tutte le costanti, stringhe di configurazione o numeri magici devono essere definite esplicitamente in file di configurazione (`.env`) o in moduli di costanti dedicati.

---

### 3. Linee Guida Specifiche Full Stack

#### 3.1. Backend (Es. Node.js/Express, Python/Django)

* **Sicurezza (OWASP Top 10):**
    * Non consentire mai all'AI di generare codice che esponga credenziali o chiavi API.
    * Verificare sempre la presenza di **Injection Flaws** (SQL, XSS) e di **CORS** mal configurati.
    * Usare **Hashing forte** (es. bcrypt) per le password.
* **Efficienza Database:**
    * Verificare che le query generate dall'AI (sia SQL che ORM) siano **indicizzate** e **ottimizzate** (evitare `SELECT *` inutile).
    * Implementare la **Paginazione** di default per grandi set di dati.
* **API Design:** Rispettare i principi **REST** (risorse, verbi HTTP, codici di stato standard) per tutte le *endpoints* generate.

#### 3.2. Frontend (Es. React, Vue, Angular)

* **Accessibilità (A11y):** Verificare che l'AI utilizzi i tag HTML semantici corretti e includa attributi **ARIA** appropriati, specialmente per elementi interattivi.
* **Ottimizzazione Rendering:**
    * Controllare l'uso di **Hook di Memoization** (`useMemo`, `useCallback` in React) per prevenire re-rendering inutili, ma senza abusarne.
    * Usare il **Lazy Loading** per i componenti grandi.
* **Gestione dello Stato:** Assicurarsi che lo stato sia gestito in modo **centralizzato e prevedibile**, evitando lo *State Lifting* eccessivo o la dispersione dello stato in componenti non collegati.

---

### 4. Il Processo di Revisione (Human-in-the-Loop)

La qualità del codice AI non dipende solo da ciò che genera, ma da come viene rivisto.

1.  **Richiesta di Contesto (Prompting):** Fornire all'AI il contesto tecnico completo (linguaggio, framework, standard di codifica, scopo del modulo).
2.  **Verifica Funzionale Iniziale:** Eseguire immediatamente il codice generato per verificarne la corretta funzionalità (fa ciò che deve fare?).
3.  **Refactoring per Qualità:** Applicare la revisione umana focalizzandosi sulle sezioni 2 e 3 (Sicurezza, SRP, Nomenclatura, Test).
4.  **Integrazione e Test di Regressione:** Integrare il codice nel *codebase* esistente e assicurarsi che non abbia introdotto bug in altre parti del sistema (test di regressione).

> **Principio Guida:** **Mai** accettare il codice AI senza un'attenta revisione umana. Il codice deve essere scritto **da te**, con l'AI come assistente alla digitazione e alla logica.