# Tech Stack

## !!Platform Independence!!

- **Platform Independence Rule** — All technologies, dependencies, configurations, and development workflows should remain platform-independent wherever reasonably possible; avoid OS-specific implementations, hardcoded paths, or environment assumptions unless strictly required, and isolate unavoidable platform-specific behavior behind configurable abstractions.


## Frontend

- **React** — Builds the web application interface for document ingestion, processing status, extracted data visualization, validation results, human verification, and land-record management.
- **JavaScript** — Primary frontend programming language used to implement application logic, UI behavior, API integration, and client-side workflows.

## Backend

- **Python** — Primary backend and AI/ML development language, providing the foundation for document processing, NLP, OCR orchestration, validation, and API services.
- **Flask** — Lightweight Python web framework used to expose backend APIs and coordinate communication between the frontend and processing services.

## Document Processing

- **OpenCV** — Performs image preprocessing operations such as denoising, resizing, rotation correction, contrast enhancement, thresholding, and document-quality improvement before OCR.
- **PaddleOCR** — OCR framework used for extracting text and document-level information from scanned land records, particularly during the initial OCR implementation.
- **BharatOCR** — Alternative/target OCR technology for improving recognition of Indian-language and multilingual government documents where conventional OCR performance is insufficient.

## NLP & AI

- **IndicBERT** — Indic-language transformer model used for multilingual natural-language understanding, classification, entity extraction, and downstream land-record NLP tasks.
- **IndicNER** — Named Entity Recognition component used to identify structured entities such as person names, locations, survey numbers, administrative areas, and other land-record-specific fields from OCR text.
- **Rule-Based NLP/Processing** — Deterministic text-processing logic used alongside ML models to normalize, parse, and interpret domain-specific land-record terminology and formats.
- **Confidence Scoring** — AI-assisted confidence estimation used to identify uncertain OCR, NLP, and validation results and prioritize records requiring human verification.

## Database & Geospatial Data

- **PostgreSQL** — Primary relational database used for storing structured land-record data, users, processing metadata, validation results, and system state.
- **PostGIS** — PostgreSQL spatial extension used to store, query, and validate geographic information associated with land parcels and boundaries.
- **ULPIN** — Unique Land Parcel Identification Number used as a standardized identifier for associating records, parcel information, and related land-record data.

## Development & Infrastructure

- **Git** — Version-control system used to track source-code changes and maintain the project's development history.
- **GitHub** — Repository and collaboration platform used for source control, pull requests, branch management, issue tracking, and team development workflows.
- **Docker** — Containerization platform used to package services and their dependencies consistently across development, testing, and deployment environments.
- **REST API** — Primary communication interface between the frontend, backend, and independently deployed processing services.
- **JSON** — Standard data-exchange format used for API communication, structured processing results, configuration, and service-to-service data transfer.

## Supporting Technologies

- **PDF Processing Libraries** — Used for reading, rendering, validating, and extracting metadata from uploaded PDF-based land documents.
- **NumPy** — Numerical computing library supporting image processing, data manipulation, and ML-related operations where required.
- **Pandas** — Data-processing library used for structured tabular data manipulation, analysis, preprocessing, and validation workflows.
- **Python Virtual Environments** — Isolates Python dependencies between development components and services to maintain reproducible environments.

## Technology Direction

The stack is intentionally designed around **modularity, multilingual AI, explainable validation, confidence-driven human verification, geospatial consistency, and progressive migration from a development-friendly monolith to production-oriented microservices**.