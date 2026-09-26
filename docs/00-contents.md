### note: 
Each document contains specific rules and boundries, when unsure about look at relevent documentation.


# Documentation Contents

- `docs/architecture.md` — Overall system architecture, components, boundaries, and how the major parts of PrithviX interact.
- `docs/api.md` — API structure, endpoints, request/response contracts, and service communication interfaces.
- `docs/data-model.md` — Core data structures, entities, relationships, and how information flows through the system.
- `docs/tech-stack.md` — Technologies, frameworks, libraries, models, and infrastructure used across the project.
- `docs/development.md` — Development stages, implementation strategy, project progression, and development conventions.
- `docs/user-workflow.md` — End-to-end user workflow and how users interact with the application.

## Pipeline

- `docs/pipeline/ingestion.md` — Document ingestion, upload handling, validation, and initial document processing.
- `docs/pipeline/img_processing.md` — Image preprocessing, enhancement, rotation, cropping, and preparation for OCR.
- `docs/pipeline/ocr.md` — OCR processing, text extraction, layout information, confidence handling, and OCR output.
- `docs/pipeline/nlp.md` — NLP-based field extraction, entity recognition, normalization, context resolution, and structured data generation.
- `docs/pipeline/validation.md` — Rule-based, cross-record, GIS, and consistency validation of extracted land-record data.
- `docs/pipeline/hum_verification.md` — Human-in-the-loop verification workflow for uncertain, missing, or conflicting extracted information.
- `docs/pipeline/database.md` — Pipeline-related database storage, persistence, and interaction with the structured land-record data.

## Decisions

- `docs/decisions/` — Architectural and implementation decisions, including their context, rationale, and consequences.