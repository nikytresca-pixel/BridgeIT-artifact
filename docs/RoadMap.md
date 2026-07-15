# Roadmap del Progetto e Assegnazione Task

Questa roadmap definisce gli obiettivi settimanali per il team e **opera concretamente le Milestone già definite in `report.md` (sezione Roadmap)**: non è un piano alternativo, ma la traduzione di quelle milestone in task settimanali assegnati a persona, con sotto-task a livello principiante.

**Squadra:**
- `@nikytresca` — Sviluppatrice di moduli e IA (Domain Layer, AI Gateway)
- `@marthinaf03` — Sviluppatore di dati e API (Persistenza, FastAPI, Frontend)

**Durata:** 4 settimane.

---

## Nota di revisione (rispetto alla bozza proposta da Gemini)

Ho corretto/integrato alcuni punti per allinearli a `report.md`, `architecture.md` e `domain-model.md`:

1. **Refuso corretto:** "Aggregate Root Requiremente" → **Requirement**.
2. **Provider AI:** la documentazione attuale prevede **un solo provider AI (Gemini)** dietro l'AI Gateway, non "Gemini/OpenRouter". Se volete davvero prevedere un secondo provider come fallback, andrebbe prima aggiunto come requisito esplicito in `report.md` (nuovo NFR) — per ora l'ho tenuto fuori, coerentemente con il principio del progetto di non implementare nulla che non sia prima documentato.
3. **Cache per l'AI:** non è attualmente un requisito documentato (non compare tra gli FR/NFR). L'ho lasciato come task **opzionale/stretch** in Settimana 2, segnalato chiaramente come tale, non come task core.
4. **Assente nella bozza originale, aggiunto qui:** il task per **FR-05 (Human Validation)** in Settimana 3. È un'omissione importante: FR-05 è il meccanismo che rende vero il principio cardine del progetto ("l'AI suggerisce, l'umano valida" — vedi `report.md` — AI Philosophy). Senza di esso, il prototipo non rispetterebbe l'idea centrale di BridgeIT, per quanto tutto il resto funzioni.
5. **Settimana 4 corretta:** la bozza originale presumeva che il Report andasse "scritto" da zero. In realtà `report.md`, `architecture.md` e `domain-model.md` sono **già scritti in dettaglio** e disponibili nel repository di documentazione (https://github.com/nikytresca-pixel/report). Il lavoro reale della Settimana 4 non è scrivere, ma **aggiornare** questi documenti per riflettere ciò che è stato effettivamente implementato (in particolare le sezioni "Current Development Status" e "Current Limitations and Future Challenges").
6. Ogni task ora rimanda esplicitamente agli **FR-xx / NFR-xx** e agli **endpoint** già definiti in `architecture.md` — così ogni sotto-task è tracciabile a un requisito già scritto, non inventato al momento.

---

## Aggiornamento 1 — Organizzazione GitHub e nuove richieste del professore

1. **L'organizzazione GitHub esiste già**: [`unibo-dtm-se-2526-bridgeit`](https://github.com/unibo-dtm-se-2526-bridgeit). Contiene (o conterrà) sia il repository `artifact` (codice) sia il repository `report` (documentazione). Il compito non è più "creare l'organizzazione", ma **migrare la documentazione già scritta in `artifact/docs/` dentro il repository `report`**, rispettandone la struttura ufficiale a 12 sezioni numerate.
2. **Richiesta preliminare del professore** durante la revisione della sezione Concetto: (a) includere la gestione utenti (user management) nello scope del progetto; (b) specificare SQLite come tecnologia di persistenza concreta. Il punto (b) è stato **confermato e reso ufficiale** (vedi Aggiornamento 2). Il punto (a), lo user management, **non è comparso nella comunicazione ufficiale di approvazione** — resta quindi un'ipotesi aperta, non un requisito confermato allo stato attuale. Non è stato aggiunto a `domain-model.md`/`architecture.md`/`report.md` per questo motivo: se il professore lo confermerà in seguito, andrà ripreso separatamente.

### A. Migrazione della documentazione nel repository `report`

- [ ] **01-concetto** — ✅ Contenuto già pronto: `Concept.md`, già rifinito con le indicazioni del professore (user management non incluso, persistenza SQLite esplicitata).
- [ ] **02-requisiti** — ✅ Contenuto pronto: da `report.md` (Problem Statement, Domain Terminology, Project Objectives/FR/NFR, User Stories, Scope, Stakeholders) — ora aggiornato con persistenza e frontend nello Scope.
- [ ] **03-design** — ✅ Contenuto pronto: da `architecture.md` e `domain-model.md`, ora aggiornati con SQLite e con il frontend come client esterno all'esagono.
- [ ] **04-sviluppo** — ⚠️ Parzialmente pronto: `report.md` (Development Methodology) e `architecture.md` (Proposed Package Structure, ora comprensiva della cartella `frontend/`) coprono i principi; vanno integrati con le istruzioni pratiche di setup del README.
- [ ] **05-validazione** — ✅ Contenuto pronto: `report.md` — Testing Strategy.
- [ ] **06-release** — ✅ Contenuto pronto: `report.md` — Version Control Convention e License.
- [ ] **07-dispiegamento** — ❌ Da scrivere: nessun deployment reale è ancora pianificato.
- [ ] **08-cicd** — ✅ Contenuto pronto: `report.md` — Continuous Integration and Continuous Delivery.
- [ ] **09-guida utente** — ❌ Da scrivere: richiede che almeno il frontend minimo (Milestone 7) sia implementato.
- [ ] **10-devguide** — ⚠️ Parzialmente pronto: il README del repository `artifact` contiene già le istruzioni di setup backend; andranno aggiunte quelle per il frontend una volta scelto il framework.
- [ ] **11-autovalutazione** — ❌ Da scrivere a fine progetto.
- [ ] **12-futuro** — ✅ Contenuto pronto: `report.md` — Current Limitations and Future Challenges.

### B. Modifiche di merito — stato aggiornato

- [x] **`report.md`** — Scope, Technologies, Roadmap e Current Development Status aggiornati con SQLite e con il frontend minimo. *(Fatto)*
- [x] **`architecture.md`** — Repository Pattern, Adapter Responsibilities e Proposed Package Structure ora nominano esplicitamente SQLite (via `sqlite3`); aggiunta la cartella `frontend/` come client esterno all'esagono. *(Fatto)*
- [ ] **`domain-model.md`** — Nessuna modifica necessaria: né la persistenza né il frontend introducono nuovi concetti di dominio (esattamente il beneficio dell'Architettura Esagonale). Resta valido **solo se e quando** lo user management verrà confermato dal professore (in quel caso servirebbe una nuova entità `User`).

---

## Aggiornamento 2 — Approvazione ufficiale: persistenza e frontend

Il progetto ha ricevuto l'approvazione ufficiale del professore, con due requisiti tecnici aggiuntivi legati alla crescita del team da una a due persone:

1. **Persistenza**: SQLite, con **SQLAlchemy** come ORM, dietro il pattern Repository.
2. **Frontend**: una web application minimale, che consuma le REST API esposte da FastAPI — nessuna pagina o funzionalità specifica oltre a quanto già implicito nei requisiti (FR-01 → FR-07).

Questi due punti sono già stati integrati in `report.md` e `architecture.md` (vedi checklist B sopra). Quello che segue è l'impatto pratico sulla roadmap operativa: la Milestone unica "Requirement Management" è stata divisa in due Milestone distinte (Persistenza e API), ed è stata aggiunta una Milestone per il Frontend — che è lavoro **nuovo**, non solo una re-invenzione di task già pianificati.

**Nota onesta sul carico di lavoro:** aggiungere un intero frontend, mantenendo il vincolo di un mese, è ambizioso per un team di due persone alle prime armi. Il piano sotto lo gestisce facendo partire il frontend in Settimana 3 (non in Settimana 4), in parallelo al resto, invece che ammassarlo tutto alla fine. Se il tempo dovesse stringere, la prima cosa da tagliare è l'ampiezza del frontend stesso (una singola pagina che copre l'intero flusso, senza stile o pagine multiple) — non le sue fondamenta architetturali (deve comunque restare un client esterno che consuma solo la REST API, mai un accesso diretto al backend).

---

## Aggiornamento 3 — Feedback ufficiale del professore dopo l'approvazione

Il progetto è stato **approvato ufficialmente** dal docente. Con l'ingresso della seconda persona nel team, il professore ha richiesto esplicitamente due requisiti tecnici aggiuntivi:

1. **Un DBMS** — SQLite è sufficiente. Era già la scelta fatta in Aggiornamento 2; qui viene confermata come requisito esplicito del professore, non solo una nostra decisione tecnica.
2. **Un frontend** (web o desktop) — anche questo già pianificato in Aggiornamento 2; il professore lo rende ora un requisito esplicito.

**Questo aggiornamento supera quanto scritto in Aggiornamento 1, punto 2:** lo **user management** (gestione utenti minimale) non era più "un'ipotesi aperta" — è ora **confermato** come parte del lavoro di persistenza richiesto. Di conseguenza, `domain-model.md` avrà bisogno di una nuova entità `User` (si veda la checklist B aggiornata sotto); non la aggiungo qui perché questa roadmap, per queste istruzioni, aggiorna solo se stessa, non gli altri documenti.

Il feedback si traduce in questi nuovi pacchetti di lavoro, distribuiti nelle settimane esistenti (dettaglio nelle sezioni di ogni settimana più sotto):

- **Persistenza:** repository SQLite via `sqlite3` per `Requirement` (già implementato — vedi PR #6 e ADR-0001 in Aggiornamento 4), **+ persistenza utenti** (autenticazione minimale), **+ persistenza backlog** (i `Derived Artifact`), con relativi test di integrazione per ciascuno.
- **Frontend:** confermato **React + TypeScript** come stack (nessuna ragione tecnica per scegliere altro, dato che il frontend resta un client esterno leggero — vedi `architecture.md` — Adapter Responsibilities). Il lavoro si scompone ora esplicitamente in: setup di progetto, comunicazione con le API FastAPI, invio requisiti, visualizzazione requisiti, visualizzazione AI Analysis, workflow di validazione umana, visualizzazione tracciabilità, e rifinitura dell'interfaccia nell'ultima settimana.

### Checklist B — aggiornamento allo stato di `domain-model.md`

- [ ] **`domain-model.md`** — **Ora richiede una modifica** (non più "nessuna modifica necessaria"): va aggiunta una nuova entità `User` (identità, ruolo), dato che lo user management è stato confermato dal professore (vedi sopra). Questa modifica non è inclusa in questa roadmap: qui viene solo tracciata come lavoro da fare, coerentemente con l'istruzione di aggiornare solo la roadmap in questo passaggio.

---

## Aggiornamento 4 — Decisione finale sulla persistenza: `sqlite3` puro, non SQLAlchemy

Durante l'implementazione del primo adapter di persistenza (`SQLiteRequirementRepository`, PR #6), il lavoro è stato svolto usando direttamente il modulo standard `sqlite3` di Python, con SQL scritto a mano — **non SQLAlchemy**, come invece pianificato in `report.md`, `architecture.md` e nelle settimane precedenti di questa roadmap.

Dopo revisione, questa scelta è stata **confermata come definitiva**: la porta `RequirementRepository` isola comunque dominio e Application Layer da qualunque tecnologia concreta, quindi `sqlite3` puro è pienamente coerente con l'architettura, semplicemente diverso dalla tecnologia originariamente pianificata. Questa è esattamente il tipo di decisione che `architecture.md` — Architecture Decision Records prometteva di registrare "una volta presa davvero": è stato quindi creato il primo ADR reale del progetto, [`docs/adr/0001-sqlite-persistence-without-orm.md`](https://github.com/unibo-dtm-se-2526-bridgeit/report/blob/master/docs/adr/0001-sqlite-persistence-without-orm.md).

Di conseguenza:
- `report.md`, `architecture.md` e questa roadmap sono stati aggiornati per parlare di **SQLite tramite `sqlite3`**, non più di SQLAlchemy.
- Ogni task **non ancora svolto** che menzionava SQLAlchemy (persistenza `User` in Settimana 2, persistenza `Artifact`/backlog in Settimana 3) è stato aggiornato di conseguenza qui sotto, per dare istruzioni corrette a chi li svolgerà.
- Nessun impatto su `domain-model.md`: la scelta tra `sqlite3` e SQLAlchemy è un dettaglio del driven adapter, non tocca il dominio.

---

## Aggiornamento 5 — Decisione finale sul frontend: HTML/CSS/JavaScript, non React + TypeScript

Durante l'implementazione del primo pezzo di frontend (PR #7, pagina di controllo dello stato del backend), il lavoro è stato svolto con HTML, CSS e JavaScript semplice, senza framework né sistema di build — **non React + TypeScript**, come invece dichiarato "confermato" in Aggiornamento 3.

Dopo revisione, questa scelta è stata **confermata come definitiva**: il frontend resta comunque un client esterno che parla con il backend solo tramite `fetch()` verso la REST API — esattamente il confine richiesto da `architecture.md` — indipendentemente da React o TypeScript. Per un progetto che il professore vuole "intenzionalmente leggero" sul frontend, un sito statico senza build system è coerente quanto (se non più) di un'intera toolchain React + TypeScript. Come per la persistenza, è stato creato un secondo ADR: [`docs/adr/0002-vanilla-html-css-js-frontend.md`](https://github.com/unibo-dtm-se-2526-bridgeit/report/blob/master/docs/adr/0002-vanilla-html-css-js-frontend.md).

Di conseguenza:
- `report.md` — Technologies aggiornato per descrivere HTML/CSS/JavaScript invece di "framework da selezionare".
- Ogni task **non ancora svolto** che menzionava React + TypeScript è stato aggiornato qui sotto.
- Nessun impatto su `domain-model.md`.
- **Nota a margine, non ancora risolta:** il frontend vive nella cartella `web/`, mentre `architecture.md` — Proposed Package Structure nomina ancora `frontend/`. Non è stata toccata in questo aggiornamento (riguarda solo `report.md`/`RoadMap.md`); se si conferma `web/` come nome definitivo, andrà allineata anche lì in un passaggio successivo.

---

## Aggiornamento 6 — Rimozione dello User Management: la fonte non era mai stata verificata

Dopo un confronto più approfondito nel team, è emerso che la "conferma" dello user management citata in Aggiornamento 3 non ha mai avuto una fonte verificabile — nemmeno nel messaggio originale che la riportava. Aggiornamento 1, scritto prima, trattava già questo punto correttamente come "ipotesi aperta, non requisito confermato"; Aggiornamento 3 lo ha capovolto senza che una conferma reale sia mai arrivata da una comunicazione ufficiale del professore.

Pesando pro e contro con cura: il pro principale — evitare una bocciatura per requisiti mancanti — dipende interamente dal chiarire una fonte che non si è mai chiarita. I contro sono invece concreti e immediati: carico di lavoro reale in un mese già stretto per due persone (nuova entità, hashing password, endpoint, test); rischio di implementare un'autenticazione frettolosa, percepita peggio in sede di valutazione di un'omissione onestamente dichiarata; e soprattutto tempo tolto al nucleo reale del progetto (Requirement → AI Analysis → Human Validation), che resta il criterio di valutazione più probabile (vedi `report.md` — AI Philosophy).

**Decisione:** lo User Management è rimosso dalla roadmap operativa. Tutti i task che lo riguardavano (entità `User` in Settimana 1, persistenza utenti ed endpoint `/users`/`/login` in Settimana 2) sono stati tolti dalle settimane sottostanti.

**Azione concreta, non rimandabile:** verificare esplicitamente e per iscritto con il professore se lo user management è davvero richiesto, prima di dedicarci qualunque tempo di sviluppo. Se arriverà una conferma reale, andrà trattato come un nuovo pacchetto di lavoro a partire da quel momento — non recuperato retroattivamente da questo aggiornamento.

Questo riporta valida la riga già scritta in **Checklist B** più sopra ("nessuna modifica necessaria a `domain-model.md`... resta valido solo se e quando lo user management verrà confermato") — quella cautela era corretta fin dall'inizio.

---

## Mappa Settimane → Milestone (da `report.md`, versione a 8 milestone)

| Settimana | Milestone coperte |
|---|---|
| 1 ✅ | Milestone 2 (Domain Model) — completata |
| 2 | Milestone 3 (Persistence Layer) + Milestone 5 (AI Gateway Integration) + **inizio** Milestone 7 (Frontend) |
| 3 | Milestone 4 (Requirement Management APIs, incluso FR-05) + sviluppo principale del Frontend + **eventuale** Milestone 6 (Traceability & Derived Artifacts, se il tempo lo consente) |
| 4 | Milestone 8 (Testing, CI/CD e Release) + rifinitura Frontend |

---

## Settimana 1 — Dominio e Setup ✅ (completata)

*(Stato reale, non più pianificazione: questa settimana risulta conclusa.)*

### `@nikytresca`

- [x] **Migrare la documentazione nel repository `report`**, sotto l'organizzazione [`unibo-dtm-se-2526-bridgeit`](https://github.com/unibo-dtm-se-2526-bridgeit), seguendo la checklist *A. Migrazione della documentazione* qui sopra.
- [x] **Sviluppare l'intero Domain Layer in Python** (vedi `domain-model.md` — Domain Entities, Value Objects, Aggregate Boundary).
  - [x] Value object: `RequirementText`, `RequirementStatus` (enumerazione: `Submitted`, `Analyzed`, `Clarified`, `Validated`, `Rejected`).
  - [x] Entità `Requirement` (identità, testo, stato), con le transizioni di stato consentite come metodi — solo Python puro.
  - [x] Unit test per entità e value object (creazione, transizioni valide/non valide, uguaglianza tra value object).

### `@marthinaf03`

- [x] **Setup del progetto FastAPI base con Poetry**, endpoint `/health`.
- [x] Package `application/` con la porta `RequirementRepository` (astratta), e modelli Pydantic (DTO) per request/response, distinti dal dominio.
- [x] Repository in-memory fittizio, con test di conformità alla porta.
- [x] **Refactor `Any` → `Requirement`** una volta integrato il Domain Layer di `@nikytresca` (chiudeva il debito tecnico tracciato nell'issue dedicata).

---

## Settimana 2 — Persistenza SQLite e avvio Frontend

### `@marthinaf03` — Milestone 3: Persistence Layer

- [ ] **Adapter `SQLiteRequirementRepository`**, conforme alla porta già definita. *(Implementato in PR #6 usando `sqlite3` puro — vedi Aggiornamento 4 e ADR-0001.)*
  - [ ] Modulo `infrastructure/persistence/`, gestione connessione al database (file locale).
  - [ ] Test: salvataggio e recupero di un `Requirement`; nessuna perdita/alterazione dei dati tra scrittura e lettura.

### `@nikytresca` — Milestone 5: AI Gateway Integration

- [ ] **Adapter AI (Gemini)**, corrispondente a FR-02.
  - [ ] Modulo `infrastructure/ai/`; porta `AIGateway` (interfaccia astratta) nell'Application Layer.
  - [ ] Implementazione che traduce la risposta Gemini in `AIAnalysis` di dominio.
  - [ ] Test con client Gemini **mockato** (mai l'API reale nei test automatici).

*Se il tempo lo consente:*
- [ ] *(Opzionale/stretch — non è ancora un requisito documentato)* Cache per le chiamate AI, solo se prima documentata come nota tecnica in `report.md`.

### Nuovo — Frontend (avvio, entrambe)

Stack confermato: **HTML, CSS e JavaScript semplice, senza framework né build system** — vedi Aggiornamento 5 e ADR-0002.

- [x] **Cartella `web/` con struttura minima**, CORS lato FastAPI dove necessario, pagina statica di prova che chiama `/health` per confermare il collegamento end-to-end. *(Già implementato in PR #7.)*
- [ ] **Wireframe/bozza delle schermate** AI Analysis + Validazione umana (solo schizzo su carta o strumento leggero — non serve codice), per dare a `@nikytresca` un punto di partenza chiaro prima della Settimana 3.
- [ ] **Contratto dati JSON condiviso tra le due**, per lo scambio tra frontend e backend sugli endpoint di Settimana 3 (forma delle richieste/risposte per analisi e validazione) — concordato insieme, per poter sviluppare le rispettive pagine in parallelo senza bloccarsi a vicenda.

---

## Settimana 3 — Casi d'uso, Integrazione, Frontend

### `@nikytresca` — Application Layer + Frontend (AI Analysis e Validazione)

- [ ] Use case "Sottometti requirement" (FR-01).
- [ ] Use case "Richiedi analisi AI" (FR-02).
- [ ] Use case "Chiarisci requirement" (FR-03).
- [ ] Use case "Ottieni Quality Indication" (FR-04).
- [ ] **FR-05 — Validazione Umana** (fondamentale, non negoziabile — è il meccanismo che rende vero "l'AI suggerisce, l'umano valida").
  - [ ] Caso d'uso `ValidationDecision` (approva / modifica / rifiuta).
  - [ ] Garanzia esplicita, con test dedicati: nessuna `AIAnalysis` cambia lo stato del `Requirement` senza decisione umana.
- [ ] Test di logica applicativa/dominio ad alta copertura.
- [ ] **Frontend — pagine Analisi AI + Validazione Umana**, che chiamano `POST /requirements/{id}/analyse` e `POST /requirements/{id}/validate` (contratto JSON concordato in Settimana 2).

### `@marthinaf03` — API + Frontend (CRUD base)

- [ ] `POST /requirements` (FR-01), `GET /requirements/{id}`.
- [ ] `POST /requirements/{id}/analyse` (FR-02), `POST /requirements/{id}/validate` (FR-05).
- [ ] Verifica manuale del flusso completo via Swagger UI.
- [ ] **Frontend — pagine CRUD base**: crea/visualizza requirement (chiamano `POST /requirements`, `GET /requirements/{id}`), gestione errori HTTP minima, collegamento/navigazione tra le pagine di entrambe.

*Se il tempo lo consente (non core):*
- [ ] `GET /requirements/{id}/traceability-links` (FR-06) e `POST /requirements/{id}/artifacts` (FR-07), con relativo caso d'uso e persistenza per `Artifact`.

---

## Settimana 4 — Report e Rifiniture Finali

### `@nikytresca`

- [ ] **Aggiornare `architecture.md` e `domain-model.md`** — solo scostamenti reali rispetto a quanto pianificato, senza riscrivere; non è più necessaria alcuna voce su `User` (rimosso in Aggiornamento 6).
- [ ] **Configurare/rivedere la pipeline CI/CD (GitHub Actions)**: install dipendenze → `poetry run poe static-checks` → `poetry run poe test`.

### `@marthinaf03`

- [ ] **Aggiornare `report.md`** — Testing Strategy, Current Development Status, Technologies (HTML/CSS/JavaScript confermato). Nessuna voce su user management da aggiungere allo Scope (vedi Aggiornamento 6).
- [ ] Documentare lo stack frontend scelto e la struttura `web/` in `report.md`.

### Insieme

- [ ] **Test end-to-end completo**: Requirement → AI Analysis → Human Validation (+ Traceability/Artifact, se implementati in Settimana 3) — eseguito anche passando dal frontend, non solo da Swagger UI.
- [ ] **Aggiornare `report.md` — "Current Limitations and Future Challenges" e "Conclusion"** con lo stato reale, senza dichiarare implementato nulla che non lo sia — inclusa una nota onesta sullo user management non implementato, in attesa di verifica col professore (Aggiornamento 6).
- [ ] Verificare tutti i link incrociati tra `report.md`, `architecture.md`, `domain-model.md`.
- [ ] **Rifinitura frontend** (leggera, ultima priorità): stati di caricamento/errore minimi, coerenza visiva di base. Non è un esercizio di design — tenerla intenzionalmente leggera.

---

## Copertura dei Functional Requirement dopo l'aggiornamento

Verifica che ogni FR abbia almeno un'attività di roadmap che lo copre, dopo la rimozione dello User Management e la ristrutturazione del frontend:

| FR | Copertura in questa roadmap |
|---|---|
| FR-01 (Requirement Creation) | Settimana 1 ✅ (`Requirement.submit`), Settimana 3 (`POST /requirements`) |
| FR-02 (AI-Assisted Analysis) | Settimana 2 (Adapter Gemini), Settimana 3 (`POST /requirements/{id}/analyse`) |
| FR-03 (Requirement Clarification) | Settimana 1 ✅ (metodo `clarify()`), Settimana 3 (caso d'uso "Chiarisci requirement") |
| FR-04 (Requirement Quality Evaluation) | Settimana 3 (`QualityScore` prodotta dall'adapter Gemini, use case "Ottieni Quality Indication") |
| FR-05 (Human Validation) | Settimana 3 (`ValidationDecision`, `POST /requirements/{id}/validate`) — **fondamentale, non negoziabile** |
| FR-06 (Traceability Link Management) | Settimana 3, **se il tempo lo consente** (non più garantito come core — vedi nota su carico di lavoro) |
| FR-07 (Derived Artifact Creation) | Settimana 3, **se il tempo lo consente** (idem) |

Tutti e sette i Functional Requirement restano coperti da almeno un'attività pianificata. FR-06 e FR-07 sono stati **retrocessi a "se il tempo lo consente"** in questo aggiornamento, in linea con la scelta di dare priorità al ciclo core (Requirement → AI Analysis → Human Validation) rispetto a funzionalità aggiuntive, ora che lo User Management non assorbe più tempo ma il margine resta comunque stretto per un mese a due persone. Nessun FR è stato rimosso.
