# Architecture

## Overview

PrithviX is structured as two distinct and independently developed applications:

```text
prithvix/
├── pipeline/
└── webapp/
```

The `pipeline/` application contains the document intelligence and validation workflow, while the `webapp/` application contains the user-facing application.

The architecture is modular. The document-processing workflow is divided into distinct services, with each service responsible for a specific stage of processing. 

The two applications are also independent of one another and communicate through HTTP. The `webapp/` does not depend directly on the internal implementation of the `pipeline/`.

At a high level, the processing workflow is:

```text
Document Upload / Scan
        ↓
Document Classification
        ↓
Image Preprocessing
        ↓
Multilingual OCR
        ↓
Field Extraction & Standardization
        ↓
Intelligent Validation
        ↓
Confidence Scoring
        ↓
Human Verification for Exceptions
        ↓
Verified Digital Record
        ↓
GIS-Linked Storage
```

The system treats a land document as evidence that must be interpreted and validated, rather than simply transcribed.

For a processed record, the system can maintain the original source-document reference, document type and language information, extracted structured fields, field-level extraction confidence, validation outcomes and reasons, GIS-validation status, reviewer actions, final verification status, and a time-stamped audit trail.

---

## pipeline/

The `pipeline/` application contains the complete document-processing workflow.

The pipeline is divided into services corresponding to the major stages of document processing. During the early stages of development, these services may be developed together as a modular monolith while maintaining their logical boundaries. As development progresses, the services will be separated into independently running microservices, communicating with each other through HTTP.

The pipeline is responsible for transforming heterogeneous land documents into structured, validated and GIS-aware digital land records.

### Service Structure

Each backend service should follow a consistent structure, with its API entry point kept separate from the service implementation:

```text
service-name/
├── api.py
└── service/
```

---

### Ingestion Service (pipeline/ingestion)

The Ingestion Service receives documents and prepares them for processing.

**Purpose**

Receive documents and establish the initial processing context for each document.

**Takes in**

- Uploaded scans.
- PDFs.
- Approved image formats.
- Basic document/source metadata.

**Responsibilities**

- Validate the file type.
- Perform basic quality checks.
- Detect the probable document type.
- Identify language and script indicators.
- Capture metadata such as source, upload time, document identifier and processing status.
- Establish the processing profile for the document.
- Route the document into the appropriate processing workflow.

**Gives out**

- A registered document.
- Document metadata and processing status.
- Probable document type and language information.
- A document prepared for image processing.

Different land-document types may use different field schemas and validation rules.

---

### Image Processing Service (pipeline/img_processing)

The Image Processing Service prepares document images for downstream OCR and analysis.

**Purpose**

Improve poor-quality scans while preserving the original document.

**Takes in**

- The original document and its pages from the Ingestion Service.

**Responsibilities**

- Deskew tilted pages.
- Align document images.
- Improve contrast and text visibility.
- Reduce unnecessary background noise.
- Enhance faded or low-quality scans.
- Produce a processing-ready copy.
- Preserve the original source document separately.

**Gives out**

- Enhanced, processing-ready document images.
- Image information suitable for OCR and layout analysis.

The expected result is improved OCR accuracy, easier visual review, and reduced errors caused by unreadable or distorted scans.

---

### Multilingual OCR and Layout Analysis Service (pipeline/ocr)

The OCR Service converts visible document content into machine-readable text and structure.

**Purpose**

Extract text and structural information from heterogeneous land documents while retaining relevant layout context.

**Takes in**

- Processed document images from the Image Processing Service.

**Responsibilities**

- Detect text blocks.
- Detect tables, labels and form-like fields.
- Extract text and tabular content.
- Handle multi-column layouts and structured record formats.
- Support multilingual Indian-language documents.
- Capture OCR confidence.
- Preserve relevant layout information rather than producing only raw text.

**Gives out**

- Extracted text.
- Extracted tables and structured content.
- Layout information.
- OCR confidence information.
- Machine-readable document content for the NLP stage.

The service is intended to support regional-language records and complex document layouts.

---

### NLP-Based Field Extraction Service (pipeline/nlp)

The NLP Service converts OCR output into standardized land-record information.

**Purpose**

Identify relevant land-record fields and transform unstructured OCR output into structured data.

**Takes in**

- OCR text.
- Document layout and context information.

**Fields that may be extracted**

- Landowner name.
- Parent or guardian name, where available.
- Survey number.
- Plot number.
- Khata or record number.
- Village.
- Taluk or tehsil.
- District.
- Land area.
- Land-use details.
- Registration number.
- Registration date.
- Mutation reference.
- Document number.
- Encumbrance details.
- ULPIN-related identifiers, where applicable.

**Responsibilities**

- Recognize fields even when their labels differ across document formats.
- Normalize date formats.
- Normalize number formats.
- Normalize measurement units.
- Convert extracted information into a structured record.
- Associate extracted fields with confidence values and document context.
- Support Indian-language understanding.

**Gives out**

- Structured land-record data.
- Standardized field values.
- Field-level confidence information.
- Information suitable for validation.

The expected result is a consistent data representation across varied land documents, with visibility into uncertain or missing fields.

---

### Intelligent Validation Service (pipeline/validation)

The Intelligent Validation Service determines whether extracted information is complete, logically consistent, and spatially valid.

**Purpose**

Validate extracted records beyond the correctness of OCR alone.

**Takes in**

- Structured land-record data from the NLP Service.
- Related authorized records, where available.
- Authorized GIS information, where available.

**Validation stages**

#### Basic Field Validation

Checks include:

- Presence of mandatory fields.
- Valid identifier and date formats.
- Valid numeric formats.
- Compatible measurement units.
- Acceptable value ranges.
- Completeness of required ownership and parcel details.

Examples include checking whether a survey number follows the permitted format for the configured record type and whether an area field contains a valid numeric value and unit.

#### Logical Consistency Validation

Checks whether related fields within the same document agree with each other.

Examples include:

- Village, taluk and district consistency.
- Compatibility between reported area and area unit.
- Consistency of repeated survey-number references.
- Expected relationships between ownership, registration and mutation information.

#### Cross-Record Validation

Compares the current document against authorized existing records.

Checks can include:

- Duplicate document or parcel references.
- Conflicting ownership information.
- Mismatched survey or plot numbers.
- Inconsistent mutation history.
- Repeated or contradictory land-area values.
- Differences between current and linked historical records.

#### GIS-Based Validation

Where authorized GIS layers and spatial data are available, spatial checks can include:

- Whether the stated parcel location aligns with map references.
- Whether the declared area is plausible relative to mapped geometry.
- Possible parcel overlap detection.
- Potential encroachment flags involving protected, public or restricted areas.
- Village, taluk or boundary consistency checks.

GIS validation supports decision-making; final legal or administrative determination remains with authorized officials.

**Gives out**

- Validation results.
- Validation reasons and findings.
- Detected inconsistencies, duplicates or conflicts.
- GIS validation status where applicable.
- A consolidated confidence assessment.
- An indication of whether further human verification is required.

#### Confidence Scoring

Confidence scoring is part of the validation workflow and provides an assessment of the reliability of a processed record.

**Takes in**

- Image quality information.
- OCR confidence.
- Field-extraction confidence.
- Mandatory-field completeness.
- Rule-validation results.
- Cross-record consistency results.
- GIS-validation results.
- Detected conflicts or suspicious indicators.

**Gives out**

- A consolidated confidence assessment.
- Routing information for the next processing step.

Records may be routed according to their confidence and validation outcome:

- **High confidence:** Satisfies defined checks and can proceed for standard verification or storage.
- **Medium confidence:** Requires targeted human review of specific fields.
- **Low confidence:** Requires detailed verification.
- **Suspicious or conflicting:** Requires authorized escalation or reporting.

---

### Human Verification Service (pipeline/hum_verification)

The Human Verification Service handles records that require human review.

**Purpose**

Provide human oversight for uncertain, conflicting, or suspicious processing results.

**Takes in**

- Extracted record data.
- Confidence information.
- Validation findings.
- Original document view.
- Enhanced processing copy.
- Related-record information.
- GIS context, where available.

**Records may be routed here because of**

- Low OCR confidence.
- Missing mandatory fields.
- Unreadable or damaged content.
- Cross-record conflicts.
- GIS inconsistencies.
- Duplicate indicators.
- Suspected manipulation or fraud signals.
- Uncertain document classification.

**Reviewer capabilities**

- Review the original document.
- Review the enhanced processing copy.
- Review extracted fields.
- Review confidence values.
- Review validation findings.
- Review related-record comparisons.
- Review GIS context where available.
- Correct extracted fields.
- Mark values as verified.
- Request additional evidence.
- Return a document for correction.
- Escalate suspicious records.
- Record reasons for acceptance, rejection, or modification.

**Gives out**

- Corrected or verified record data.
- Final verification status.
- Reviewer actions and decisions.
- Review history for the audit trail.

Every human action is retained as part of the record's audit history.

---

### Data Recording and Storage (pipeline/database)

The data-recording layer stores the output of the processing and verification workflow.

**Purpose**

Persist structured and verified land-record information together with the information required to trace and understand the record.

**Takes in**

- Validated land-record data.
- Human-verified corrections where applicable.
- Source-document references.
- Validation results.
- Confidence information.
- Verification information.
- Relevant GIS information.

**Gives out**

- Persisted digital land records.
- Source-document linkage.
- Validation status and reasons.
- Confidence information.
- Verification status.
- Audit history.
- GIS-linked record information where available.

The stored record represents the final output of the document-processing workflow rather than only the raw OCR result.

---

### API Gateway (pipeline/api)

The API Gateway provides the external HTTP entry point to the pipeline.

**Takes in**

- HTTP requests from the `webapp/`.
- Document-processing requests.
- Requests for processing status and results.

**Responsibilities**

- Receive pipeline requests.
- Route requests to the appropriate pipeline service.
- Provide a controlled interface to the internal services.
- Expose processing-related APIs.

**Gives out**

- HTTP responses to the `webapp/`.
- Processing status.
- Processing results.
- Appropriate service responses.

The webapp communicates with the pipeline through the API Gateway rather than directly depending on individual internal services.

---

## webapp/

The `webapp/` application is responsible for the user-facing side of PrithviX.

It is divided into a frontend and backend following the general structure of a conventional web application.

The detailed responsibilities, internal boundaries, and communication design of the frontend and backend are intentionally left open and will be defined as development progresses.

---

## pipeline/ ↔ webapp/

The `pipeline/` and `webapp/` applications are independent applications.

They communicate exclusively through HTTP.

The `webapp/` should interact with the pipeline through the pipeline's defined HTTP interface rather than directly accessing individual pipeline services or depending on their internal implementation.

The detailed communication contract between the two applications will be defined as the API architecture is finalized.