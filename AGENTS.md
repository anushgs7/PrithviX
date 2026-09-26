# AGENTS.md

## Project Overview

PrithviX is an AI-assisted, end-to-end land record digitization and validation system developed for Smart India Hackathon 2026.

The system processes heterogeneous land documents and converts them into structured, validated digital records while retaining human verification for uncertain or conflicting cases.

The repository is divided into two major areas:

- `pipeline/` — document processing, extraction, validation, and data services.
- `webapp/` — user-facing application and application backend.

Keep these concerns clearly separated.

---

## Core Processing Pipeline

The intended processing flow is:

```text
Document Upload
    ↓
Document Classification
    ↓
Image Preprocessing
    ↓
Multilingual OCR + Layout Analysis
    ↓
NLP / Field Extraction + Standardization
    ↓
Intelligent Validation
    ↓
Confidence Scoring
    ↓
Human Verification
    ↓
Verified Digital Record
    ↓
GIS-linked Storage
```

Do not bypass processing stages without a clear reason.

OCR output must not be treated as ground truth. Extraction and validation are separate concerns.

---

## Repository Structure

The project follows this high-level organization:

```text
/
├── AGENTS.md
├── README.md
├── LICENSE
├── docs/
│
├── pipeline/
│   ├── ingestion/
│   ├── img_processing/
│   ├── ocr/
│   ├── nlp/
│   ├── validation/
│   ├── hum_verification/
│   ├── api/
│   └── database/
│
└── webapp/
```

The exact structure may evolve as implementation progresses. Do not create new top-level directories when an existing project area is appropriate.

---

## Architecture Principles

### Separation of concerns

Each service should have one clear responsibility.

- Ingestion handles receiving and preparing documents.
- Image processing improves documents for downstream processing.
- OCR extracts text and layout information.
- NLP converts extracted content into structured fields.
- Validation checks correctness and consistency.
- Human verification handles uncertain or conflicting records.
- Database components handle persistence.
- API components expose controlled interfaces.
- `webapp/` handles application and user-interface concerns.

Do not place unrelated business logic into API routes, UI components, or infrastructure code.

### Modularity and Microservice Architecture

PrithviX is intentionally designed around a **microservice-based architecture**, and the project is intended to be submitted and presented as a microservice-based system.

Major processing and application responsibilities should maintain clear, independently testable boundaries and should not be unnecessarily coupled. Components may be deployed together when appropriate for the current development stage.

The intended architectural separation includes services such as:

- Ingestion Service
- Image Processing Service
- OCR Service
- NLP / Field Extraction Service
- Validation Service
- Human Verification Service
- Database / Data Service
- API Gateway
- Web Application

Each service should have:

- a clearly defined responsibility,
- explicit input and output contracts,
- well-defined interfaces,
- minimal knowledge of other services' internal implementations,
- independent testability,
- the ability to evolve without unnecessarily coupling it to other services.

Services should communicate through clearly defined APIs, contracts, and data structures rather than directly depending on each other's internal code.

### Architectural Boundary

Maintain a clear separation between:

```text
pipeline/
    → processing and intelligence services

webapp/
    → user-facing application and application-layer services
```

Avoid tight coupling between `pipeline/` and `webapp/`.

Do not collapse multiple pipeline responsibilities into a single monolithic service merely for convenience.

Similarly, do not introduce artificial microservices for components that have no meaningful independent responsibility or boundary.

The goal is **meaningful service separation**, not maximizing the number of services.


### Security

Treat uploaded documents and extracted land-record information as sensitive application data.

Never:

- commit credentials or API keys,
- hard-code secrets,
- log sensitive document contents unnecessarily,
- expose database credentials,
- disable security checks merely to make development easier,
- trust uploaded filenames or MIME types blindly.

Validate uploaded files and constrain filesystem operations to intended locations.

Authentication and authorization must be enforced server-side, not only in the frontend.


### Testing

New functionality should include appropriate tests.

Prioritize tests for:

- field extraction,
- normalization,
- validation rules,
- confidence calculations,
- schema transformations,
- API behavior,
- database interactions,
- edge cases involving malformed or incomplete documents.

When fixing a bug, prefer adding a regression test that reproduces the problem.

Do not remove failing tests merely to make the test suite pass.


### Error Handling

Errors should be handled at the appropriate layer.

Distinguish between:

- invalid user input,
- invalid documents,
- processing failures,
- external-service failures,
- validation failures,
- system/infrastructure failures.

Do not use exceptions as ordinary control flow when a clearer result type or validation result is appropriate.

Errors intended for users should be understandable without exposing internal implementation details.


### Logging

Logs should help diagnose processing and system failures without unnecessarily exposing sensitive data.

Prefer structured, meaningful log messages.

Include useful identifiers such as document or processing IDs where appropriate.

Avoid logging:

- passwords,
- tokens,
- API keys,
- full sensitive documents,
- unnecessary personal information.

---

## Working With the Repository

Before making substantial changes:

1. Inspect the existing implementation.
2. Identify the relevant service/module.
3. Read related documentation.
4. Check existing tests.
5. Understand existing interfaces and schemas.
6. Make the smallest coherent change.
7. Run relevant tests and checks.
8. Update documentation when necessary.

Do not invent existing functionality, APIs, schemas, or directories.

If the repository's implementation differs from the intended architecture documented here, inspect the code and existing documentation before changing it.

---

## Documentation and Documentation Update Rules

Detailed project information belongs in `docs/`, not in this file.
The architecture and development pipeline documented in `docs/` represent the agreed direction of the project. 
Browse the `docs/00-contents.md` to understand how documentation is structured.

Use documentation for:

- architecture and system structure,
- processing pipeline and development workflow,
- data models,
- API documentation,
- validation rules,
- deployment,
- development setup,
- major design decisions,
- other technical specifications and project-level references.

Documentation should remain consistent with the actual implementation, but documentation changes must be intentional and scoped to the task.

### General Rules

- Update documentation when a code change makes existing documentation inaccurate or materially incomplete.
- Keep documentation concise and focused on information that will remain useful to future contributors and agents.
- Do not duplicate the same information across multiple documentation files unless there is a clear reason.
- Prefer linking to the authoritative document rather than maintaining multiple copies of the same specification.
- Do not create documentation solely to describe trivial implementation details that are obvious from the code.

### When Documentation Must Be Updated

Update the relevant documentation when a change affects:

- architecture,
- service responsibilities,
- processing or development workflow,
- public APIs,
- data models or schemas,
- validation behavior,
- major configuration,
- deployment procedures,
- developer setup,
- important dependencies,
- significant design decisions.

Bug fixes and small internal changes generally do not require documentation updates unless they change documented behavior.

### Architecture and Development Pipeline

Documentation describing the project's architecture or development pipeline represents the agreed direction of the project.

Agents must not modify these documents independently to justify or accommodate an implementation.

If the implementation requires a change to the documented architecture or development pipeline:

1. Identify the required change and why it is necessary.
2. Discuss the change before modifying the relevant documentation.
3. Once the change is agreed upon, update the documentation to reflect the new direction.
4. Ensure the implementation remains consistent with the updated documentation.

Do not silently change the architecture documentation simply because a different implementation appears easier.

### Design Decisions

Major design decisions should be recorded in:

```text id="4w7x2f"
docs/decisions/
```
Decision documents should explain, where appropriate:

- the problem or context,
- the decision that was made,
- alternatives considered,
- the reasoning behind the decision,
- important trade-offs,
- consequences or limitations.

Do not silently reverse or replace a previously documented major design decision. If a significant decision needs to change, document the new decision and the reasoning behind the change.

When a significant decision is made, add or update a decision document describing the context, decision, alternatives, reasoning, trade-offs, and consequences where appropriate.


### Keeping Documentation Consistent

When modifying documented behavior:

- Search for references to the affected concept before making changes.
- Update dependent documentation when necessary.
- Remove obsolete instructions rather than leaving contradictory information.
- Ensure examples, diagrams, directory structures, and commands still match the implementation.
- Do not claim functionality exists unless it is actually implemented.

Documentation should describe the **current agreed state** of the project, while decision records may preserve the history and reasoning behind important changes.


### Decision-Making Rules

When multiple implementations are possible:

1. Prefer the simplest implementation that satisfies the requirement.
2. Prefer existing project abstractions over introducing duplicates.
3. Prefer explicit and testable behavior over implicit magic.
4. Preserve document provenance and auditability.
5. Preserve separation between extraction, validation, and presentation.
6. Avoid premature abstraction.
7. Avoid adding dependencies without a demonstrated need.
8. Favor maintainability over cleverness.

When uncertain about an architectural change, inspect the existing code and documentation before introducing a new pattern.

---

## Coding Rules

- Follow existing project structure and conventions before introducing new patterns.
- Prefer simple, readable, maintainable code over clever or unnecessarily abstract solutions.
- Keep functions, classes, and modules focused on a clear responsibility.
- Avoid unnecessary duplication, but do not introduce abstractions prematurely.
- Do not add dependencies unless there is a clear justification.
- Do not perform unrelated refactoring or silently change unrelated behavior.
- Keep configuration separate from application logic.
- Never hard-code secrets, credentials, or environment-specific configuration.
- Add or update tests when changing non-trivial behavior.
- Preserve existing interfaces and contracts unless changing them is explicitly required.
- Prefer explicit error handling over silently ignoring failures.
- Do not leave debugging code, temporary files, unused imports, or experimental code in the final implementation.

### Working With Existing Code

Before creating something new, search the repository for existing functionality that may already solve the problem.

Prefer:

```text
reuse existing implementation
        ↓
extend existing implementation
        ↓
refactor existing implementation if necessary
        ↓
create a new implementation only when justified
```

Do not create duplicate utilities, services, schemas, or abstractions without first checking whether an equivalent already exists.

---

## Requirement and Ambiguity Handling

Agents must not assume implementation details, requirements, or behavior that have not been specified by the user or established by the project's documentation and existing architecture.

### Explicit Requirements

Before implementing a request, determine whether it provides enough information to establish:

- what should be implemented,
- what behavior is expected,
- where the change belongs,
- and any important constraints.

If a request is vague, underspecified, or has multiple materially different interpretations, **do not choose an implementation arbitrarily**.

Instead:

1. Stop before making code changes.
2. Identify what is unclear.
3. Ask the user for the missing information.
4. When useful, present a small number of possible approaches.
5. Wait for the user's response before implementing.

**Do not treat a vague request as permission to design the feature yourself.**

For example, if the user says:

```text
"Add validation."
```

Do not immediately implement a validation system. Ask what should be validated, what constitutes valid/invalid data, where validation should occur, and what should happen when validation fails.


### When the Agent May Decide

The agent may independently decide **minor implementation details** when the intended behavior is already clear and the decision does not materially affect architecture or behavior.

Examples include:

- variable and function names,
- internal helper structure,
- formatting,
- test organization,
- straightforward error handling,
- equivalent implementations of explicitly defined behavior.

The distinction is:

> **Implementation detail → agent may decide.**  
> **Requirement, behavior, or implementation direction → user must decide.**


### Significant Decisions Require Agreement

Explicit user agreement is required before introducing or changing:

- architecture,
- service boundaries,
- public APIs,
- database schemas,
- major dependencies,
- processing workflows,
- security models,
- data models,
- significant user-facing behavior,
- or other significant project-level decisions.

If completing a task conflicts with an established architectural or design decision:

1. Stop before making the conflicting change.
2. Explain the conflict.
3. Propose the smallest reasonable change.
4. Discuss it with the user.
5. Implement only after agreement.

Do not use implementation convenience as sufficient justification for changing established project direction.

### Planning Before Implementation

For small, well-defined changes, implementation may begin directly.

For larger changes or changes involving significant behavior or design decisions, first provide a concise plan covering:

- intended approach,
- affected components,
- important decisions,
- expected files to change,
- tests to add or modify.

When the change involves a significant decision, wait for agreement before making substantial modifications.

Once an approach has been agreed upon, do not materially change it without discussing the deviation first.

---

## Agent Autonomy

Agents are expected to be autonomous in **execution**, not in **project direction**.

They should independently handle routine engineering decisions and carry out clearly specified work.

They should seek explicit user agreement when requirements are unclear or when a decision could materially affect the project's architecture, behavior, or long-term direction.

**High autonomy for implementation; low autonomy for changing requirements or project direction.**