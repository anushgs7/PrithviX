# PrithviX

**AI-driven land document digitization and intelligent validation.**

PrithviX is a proposed platform for transforming heterogeneous Indian land documents into structured, validated, GIS-aware digital land records. Rather than treating OCR output as a final record, it preserves the source evidence, measures uncertainty, applies validation checks, and routes exceptions to authorized human reviewers.

Built for Smart India Hackathon 2026 - Problem Statement **SIH26018**, *Intelligent Land Record Digitization and Validation System* (Smart Automation, Software).

## The problem

Land information is often fragmented across Records of Rights (RoR), sale deeds, mutation records, encumbrance certificates, property certificates, survey records, legacy registers, and handwritten supporting documents. These records vary by department, district, period, language, script, layout, and quality. Manual transcription is slow and error-prone, while OCR alone cannot establish whether a parcel reference, ownership detail, area, or location is correct.

PrithviX addresses this by creating a traceable workflow from an uploaded document to a verified, explainable digital record.

## What PrithviX does

- Accepts scanned documents, PDFs, and approved image formats.
- Classifies document type and identifies language/script indicators.
- Enhances degraded scans through deskewing, alignment, contrast improvement, and noise reduction.
- Extracts multilingual text, tables, and layout context.
- Identifies and normalizes land-record fields such as owners, survey or plot numbers, Khata numbers, location, area, registration and mutation references.
- Validates records with field rules, logical-consistency checks, cross-record comparisons, and authorized GIS layers.
- Assigns document- and field-level confidence scores.
- Sends low-confidence, conflicting, incomplete, or suspicious records to human reviewers.
- Stores verified data with source links, validation findings, reviewer actions, and a time-stamped audit trail.

## Processing workflow

```text
Document upload or scan
  -> Classification and metadata capture
  -> Image preprocessing
  -> Multilingual OCR and layout analysis
  -> Field extraction and normalization
  -> Intelligent validation
  -> Confidence scoring
  -> Human verification for exceptions
  -> Verified, GIS-linked digital record
```

## Validation-first approach

Successful text extraction is not enough to make a record trustworthy. PrithviX is designed to layer several checks:

| Validation layer | Examples |
| --- | --- |
| Basic field validation | Required fields, identifier/date formats, numeric values, compatible area units |
| Logical consistency | Agreement among village, taluk, district, survey number, ownership, registration, and mutation information |
| Cross-record validation | Duplicate references, contradictory ownership, inconsistent parcel history, or mismatched area values |
| GIS-assisted validation | Parcel-location alignment, area plausibility, overlaps, boundary consistency, and potential restricted-area flags |

GIS results support authorized decision-making; final legal or administrative determination remains with the appropriate officials.

## Human-in-the-loop review

Automation reduces repetitive work without removing accountability. Records are routed to a reviewer when OCR confidence is low, mandatory fields are missing, a document is damaged, validation identifies conflicts, or possible manipulation is detected.

The review workspace is intended to show the original and enhanced documents, extracted fields, confidence values, validation findings, related-record comparisons, GIS context, and an auditable decision history. Authorized reviewers can correct and verify fields, request additional evidence, return records for correction, reject records, or escalate suspicious cases.

## Digital Parcel Card

After a parcel is verified, PrithviX can issue a Digital Parcel Card with a secure QR code. Subject to role-based access, the card can link an authorized user to the parcel's ULPIN or unique identifier, survey/plot number, location, area, GIS reference, verification status, update date, and supporting-document references. This provides a practical way to verify a record in the field while retaining the underlying audit trail.

## Proposed architecture

PrithviX follows a modular microservices design so individual components can be built, tested, and scaled independently.

```text
User applications / authorized integrations
                  |
              API gateway
                  |
  +---------------+----------------+
  |                |                |
Ingestion -> Image processing -> OCR -> NLP / field extraction
                                        |
                          Intelligent validation <-> GIS / PostGIS
                                        |
                          Human verification and audit trail
                                        |
                       Structured, verified land-record database
```

### Services

| Service | Responsibility |
| --- | --- |
| Ingestion | Receives documents, validates files, captures metadata, and selects a processing profile |
| Image processing | Prepares scans for analysis while retaining the original source |
| OCR | Extracts text, tables, layout, and OCR-confidence signals from multilingual documents |
| NLP | Maps OCR output into a normalized land-record schema with field confidence |
| Intelligent validation | Runs rules, cross-record checks, GIS-assisted checks, confidence scoring, and exception flags |
| Human verification | Manages review queues, corrections, decisions, escalations, and reviewer audit history |
| Database | Stores records, parcel/GIS metadata, source links, validation results, and audit data |
| API gateway | Provides secure upload, status, search, and authorized integration endpoints |

## Repository layout

```text
webapp/
  frontend/                 # Planned citizen and official interfaces
  backend/                  # Planned application backend
pipeline/
  ingestion/                # Document intake and metadata
  img_processing/           # Scan enhancement
  ocr/                      # Multilingual OCR and layout extraction
  nlp/                      # Field extraction and normalization
  validation/               # Rules, comparison, GIS, and confidence scoring
  hum_verification/         # Exception review and audit workflow
  database/                 # Structured and spatial record storage
  api/                      # API gateway/orchestration
```

## Proposed technology stack

- **Frontend:** React
- **Backend/API orchestration:** Python and Flask
- **Image preprocessing:** OpenCV
- **OCR:** PaddleOCR and BharatOCR
- **Language understanding:** IndicBERT-oriented extraction logic
- **Data storage:** PostgreSQL
- **Spatial data and queries:** PostGIS
- **Deployment:** Docker-based containerization

## Roadmap

1. **MVP** - support priority document types, upload/status workflows, preprocessing, multilingual OCR, an initial record schema, reviewer dashboard, and basic field checks.
2. **Intelligent validation** - add document-specific extraction, normalization rules, confidence scoring, exception routing, and review auditability.
3. **Record and GIS integration** - connect authorized historic records and GIS layers; enable cross-record, parcel, overlap, and area checks.
4. **Scale and improve** - extend document and language coverage, tune thresholds from reviewer feedback, add analytics, and expand authorized integration APIs.

## Status

This repository currently contains the initial service-oriented project structure. The architecture and stack above describe the intended implementation; service code, user interfaces, infrastructure configuration, and runnable setup instructions are still in progress.

## Expected value

PrithviX aims to reduce repetitive data entry, improve consistency and transparency, and make verified land information easier to access. Its validation-first, GIS-aware, and human-accountable design supports citizens, government departments, surveyors, field officials, and authorized due-diligence stakeholders.

