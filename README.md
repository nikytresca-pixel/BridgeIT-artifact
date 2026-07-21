# BridgeIT

![Status](https://img.shields.io/badge/status-active%20development-brightgreen)
![Python](https://img.shields.io/badge/python-3.11%2B-blue)
![License](https://img.shields.io/badge/license-Apache%202.0-lightgrey)

> AI-Supported Requirements Engineering Platform — University of Bologna Software Engineering Project (A.Y. 2025/2026)

## Project Overview

BridgeIT is a **Requirements Engineering platform** that helps business stakeholders and software engineers transform natural-language requirements into structured software artifacts, while preserving complete traceability between a requirement and everything derived from it.

Requirements Engineering is one of the most critical and error-prone disciplines in software development: requirements originate as informal, ambiguous natural-language statements, and translating that informal intent into structured, unambiguous, traceable engineering artifacts is a well-documented source of project failure. BridgeIT uses Artificial Intelligence to assist this translation — flagging ambiguity, proposing structure, and suggesting revisions — but AI in BridgeIT never decides autonomously. Every AI-generated suggestion is a proposal that requires explicit human validation before it can affect the authoritative state of a requirement.

This distinction is what separates BridgeIT from a generic AI chatbot: BridgeIT is built around an explicit domain model, a defined workflow, and an architecture that keeps every AI-assisted suggestion reviewable, attributable, and traceable to its origin.

The platform is designed around four cardinal engineering principles:

- **Domain-Driven Design (DDD)** — an explicit domain model (Requirement, Artifact, AI Analysis, Traceability Link) expressed in terms meaningful to the Requirements Engineering domain, not to any particular storage or delivery technology.
- **Hexagonal Architecture (Ports and Adapters)** — the domain and application logic are isolated from external technical concerns (web framework, persistence, AI provider) behind explicit ports, so the domain remains independently testable and technology-agnostic.
- **SOLID principles**, most notably the **Dependency Inversion Principle** — dependencies always point inward, toward the domain; adapters depend on abstractions defined by the layers they serve, never the reverse.
- **AI isolated through an AI Gateway** — access to the AI provider (the Gemini API) is mediated entirely through a dedicated gateway abstraction invoked by the application layer, so the domain has no dependency on any AI provider, and the provider itself remains replaceable in principle.

## Current Status

**BridgeIT's core backend, testing, and CI/CD infrastructure are complete and verified.** The project currently has **26 automated tests at 95% coverage**. What remains is the AI Gateway integration and the human validation workflow (FR-05) — the mechanism that makes the project's central "AI suggests, the human validates" principle real — currently in progress.

**Completed:**
- Domain layer: the `Requirement` entity and its value objects, with tested lifecycle transitions.
- Persistence: a SQLite-based repository (via Python's standard `sqlite3` module — see [ADR-0001](https://github.com/unibo-dtm-se-2526-bridgeit/report/blob/master/docs/adr/0001-sqlite-persistence-without-orm.md)), with integration tests confirming real on-disk persistence.
- APIs: `POST /requirements` and `GET /requirements/{id}` (FR-01), with a shared structured error format across the API.
- Frontend: a minimal web application in plain HTML/CSS/JavaScript (see [ADR-0002](https://github.com/unibo-dtm-se-2526-bridgeit/report/blob/master/docs/adr/0002-vanilla-html-css-js-frontend.md)), with working create/view requirement pages connected to the real backend.
- CI/CD: a fully functional pipeline (GitHub Actions), including containerization (Docker, Docker Compose) and automatic releases via `semantic-release`.

**Not yet implemented:**
- AI Gateway integration (Gemini API) and AI-assisted requirement analysis (FR-02, FR-04).
- Human validation of AI suggestions (FR-05) — in progress.
- Traceability links and derived artifacts (FR-06, FR-07) — planned as a stretch goal if time allows.
- Frontend pages for AI analysis and human validation (planned once the corresponding backend endpoints exist).

The full, up-to-date status is maintained in [`docs/RoadMap.md`](./docs/RoadMap.md) and [`docs/report.md`](./docs/report.md#current-development-status), and is expected to evolve incrementally, milestone by milestone, alongside this README.

## Repository Organization

This repository (`artifact`) is part of the [`unibo-dtm-se-2526-bridgeit`](https://github.com/unibo-dtm-se-2526-bridgeit) GitHub organization, consistent with the structure recommended for the University of Bologna Software Engineering course: this repository holds the implementation, while the documentation (Project Report, Architecture, Domain Model, and Roadmap) lives in the dedicated [`report`](https://github.com/unibo-dtm-se-2526-bridgeit/report) repository.

## Project Documentation

The project's documentation lives in the dedicated [`report`](https://github.com/unibo-dtm-se-2526-bridgeit/report) repository, including the Architecture Decision Records under `docs/adr/`. A working copy is also kept locally under [`docs/`](./docs) in this repository for convenience during development; the two are kept in sync.

## Architecture & Documentation

Full project documentation lives under [`docs/`](./docs). Each document has a distinct, non-overlapping scope:

| Document | Summary |
|---|---|
| [**Report**](./docs/report.md) | Project vision, problem statement, functional and non-functional requirements, user stories, workflow, methodology, roadmap, and current development status. |
| [**Architecture**](./docs/architecture.md) | The Hexagonal Architecture layering (driving adapters → application layer → domain / AI Gateway → driven adapters), the dependency rules that govern it, the AI Gateway's isolation from the domain, the proposed package structure, and the illustrative API design. |
| [**Domain Model**](./docs/domain-model.md) | The Domain-Driven Design model: entities (Requirement, Artifact, AI Analysis, Traceability Link), value objects, domain rules, the Requirement aggregate root and the invariants it protects, and the project's ubiquitous language. |

All three documents are kept consistent with one another and are updated incrementally as the project progresses, following the same Conventional Commits discipline used for the codebase.

## Development Setup

BridgeIT is built in Python and managed with [Poetry](https://python-poetry.org/).

```bash
# Clone the repository
git clone <repository-url>
cd bridgeit

# Install dependencies (creates/uses the project's virtual environment)
poetry install
```

**Alternatively, using Docker** (no local Python or Poetry installation required):

```bash
docker compose up
```

Once dependencies are installed, the FastAPI service and the minimal frontend (under [`web/`](./web)) can be run locally; see [`docs/report.md` — Roadmap](./docs/report.md#roadmap) for current milestone status.

## Quality Assurance

Project tasks are run through [`poe`](https://github.com/nat-n/poethepoet) (Poe the Poet), configured as the project's task runner on top of Poetry.

```bash
# Run the automated test suite (Pytest)
poetry run poe test

# Run static analysis (Ruff for linting, Mypy for type checking)
poetry run poe static-checks
```

Both commands run automatically in the project's CI/CD pipeline (GitHub Actions) on every commit, alongside automatic releases via `semantic-release` (see [`docs/report.md` — Continuous Integration and Continuous Delivery](./docs/report.md#continuous-integration-and-continuous-delivery)).

## License

This project is licensed under the **Apache License 2.0** (see [`docs/report.md` — License](./docs/report.md#license) for details on the current licensing status).
