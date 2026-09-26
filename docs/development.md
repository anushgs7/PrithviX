# Development Stages

PrithviX is developed progressively across four major stages. Each stage has a distinct purpose and completion criteria, allowing the project to evolve from a minimal technical foundation into a complete, competition-ready application.

The stages are:

1. **Foundation Stage**
2. **MVP Stage**
3. **Application Stage**
4. **Submission Stage**

The stages are sequential in terms of their primary objectives, but development may occasionally move between stages when findings from later stages require changes to earlier components.

---

## 1. Foundation Stage

### Objective

Establish the fundamental technical infrastructure of PrithviX before implementing the complete land-record processing workflow.

The focus of this stage is on building the project's core components, interfaces, architecture, and development infrastructure from the ground up.

### Scope

The Foundation Stage includes:

- Establishing the overall project structure.
- Implementing the initial architecture defined in `architecture.md`.
- Establishing core data models.
- Defining interfaces between major pipeline components.
- Establishing the initial database schema.
- Creating the basic API structure.
- Setting up the development environment.
- Establishing configuration and environment management.
- Implementing basic logging and error-handling conventions.
- Creating initial skeletons for the processing pipeline.
- Establishing conventions for testing and development.

At this stage, individual components do not need to perform the complete intended processing. The priority is to establish a stable foundation on which the remaining system can be built.

### Expected Outcome

At the end of the Foundation Stage, PrithviX should have a coherent technical structure with clearly defined responsibilities and interfaces between its major components.

The system may not yet provide a useful end-to-end result.

### Exit Criteria

The Foundation Stage is considered complete when:

- The project structure is established.
- Core architectural decisions are implemented.
- Major components have defined interfaces.
- Initial data models and database structures exist.
- API contracts are established.
- Pipeline components have initial implementations or well-defined skeletons.
- The project can be developed and tested consistently in the intended environment.

---

## 2. MVP Stage

### Objective

Build the core land-record processing workflow and demonstrate that a real document can pass through the primary PrithviX pipeline and produce a meaningful structured and validated result.

The MVP is primarily concerned with **technical feasibility and end-to-end functionality**, rather than a polished user experience.

### Core Workflow

The primary workflow should progress approximately as follows:

```text
Land Record Document
        ↓
Image Preprocessing
        ↓
OCR
        ↓
Text / Layout Processing
        ↓
NLP / Information Extraction
        ↓
Structured Land Record
        ↓
Validation Engine
        ↓
Validated Record
        ↓
Database
```

### Scope

The MVP Stage includes:

- Implementing the actual document-processing pipeline.
- Implementing image preprocessing.
- Integrating the selected OCR solution.
- Processing OCR output.
- Implementing relevant NLP and information-extraction functionality.
- Converting extracted information into the project's structured data model.
- Implementing the initial validation engine.
- Implementing cross-field and cross-record validation where applicable.
- Implementing initial GIS-related validation where applicable.
- Persisting processed records in PostgreSQL/PostGIS.
- Establishing confidence and validation results.
- Testing the pipeline using representative land-record documents.
- Establishing an end-to-end processing workflow.

The MVP should prioritize a reliable and understandable workflow over implementing every planned feature.

### Expected Outcome

A real land-record document should be capable of being processed through the system and transformed into a structured record with meaningful validation results.

For example:

```text
Input
  ↓
Scanned Land Record
  ↓
OCR + Processing
  ↓
Extracted Information
  ↓
Structured Record
  ↓
Validation
  ↓
Validation Results + Confidence
  ↓
Stored Land Record
```

The MVP does not need to provide the final polished user interface.

### Exit Criteria

The MVP Stage is considered complete when:

- A real document can pass through the complete core pipeline.
- OCR produces usable information.
- Relevant information can be extracted and structured.
- Validation rules can identify relevant inconsistencies or errors.
- Validation results and confidence information are generated.
- Processed records can be persisted in the database.
- The complete workflow can be reproduced reliably.
- Major pipeline components communicate correctly.

The key question for this stage is:

> **Can PrithviX take a real land record and produce a meaningful structured and validated digital record?**

---

## 3. Application Stage

### Objective

Transform the functional MVP into a complete application that can be used by its intended users.

The focus shifts from proving that the processing pipeline works to making the entire system accessible, understandable, and practical to use.

### Scope

The Application Stage includes:

- Building the web application.
- Implementing the frontend.
- Integrating the frontend with the backend APIs.
- Implementing document upload and processing workflows.
- Displaying extracted land-record information.
- Displaying validation results and confidence scores.
- Implementing human verification workflows.
- Providing interfaces for reviewing and correcting extracted information.
- Integrating GIS/map-based functionality where required.
- Implementing relevant user and system management functionality.
- Improving error handling and user feedback.
- Connecting all major components into a unified application.
- Testing complete user workflows.

The application should expose the capabilities developed during the MVP stage without unnecessarily duplicating processing logic in the application layer.

### Expected Outcome

A user should be able to interact with PrithviX through the application and perform the intended land-record digitization and validation workflow from beginning to end.

A typical workflow should resemble:

```text
User
 ↓
Upload Document
 ↓
Processing
 ↓
OCR / NLP / Validation
 ↓
Review Results
 ↓
Human Verification
 ↓
Correction / Approval
 ↓
Final Digital Record
```

### Exit Criteria

The Application Stage is considered complete when:

- The primary user workflow is fully functional.
- The frontend and backend communicate correctly.
- Documents can be uploaded and processed.
- Processing results can be viewed.
- Validation results are clearly presented.
- Human verification can be performed.
- Corrected/verified information can be persisted.
- Major user-facing errors are handled appropriately.
- The complete application can be demonstrated from start to finish.

The key question for this stage is:

> **Can a user actually use PrithviX to perform the intended workflow?**

---

## 4. Submission Stage

### Objective

Stabilize, refine, and optimize PrithviX for demonstration, evaluation, and final submission.

At this stage, development shifts away from introducing major architectural changes and toward improving the reliability, accuracy, usability, and presentation of the existing system.

### Scope

The Submission Stage includes:

- Fixing remaining bugs.
- Improving pipeline accuracy.
- Improving validation rules based on testing.
- Improving OCR/NLP performance where practical.
- Optimizing performance where necessary.
- Improving error handling.
- Improving the user experience.
- Incorporating relevant feedback from mentors.
- Incorporating relevant feedback from judges or evaluators.
- Refining the application's visual presentation.
- Improving demo reliability.
- Preparing the deployment/demo environment.
- Finalizing technical documentation.
- Preparing sample datasets and demonstration documents.
- Preparing the final demonstration workflow.
- Finalizing presentation and supporting material.

### Change Policy

Major architectural changes should generally be avoided during this stage unless they are necessary to resolve a significant technical problem.

Changes should primarily be:

- Corrective
- Incremental
- Performance-oriented
- Accuracy-oriented
- Usability-oriented
- Feedback-driven

The purpose of this stage is **not to continuously expand the scope of PrithviX**, but to make the existing system stronger and more reliable.

### Expected Outcome

PrithviX should be stable enough to demonstrate its complete workflow consistently and should present the project's technical capabilities clearly to mentors, judges, and other evaluators.

### Exit Criteria

The Submission Stage is considered complete when:

- Critical bugs are resolved.
- The primary demonstration workflow is stable.
- Major accuracy issues have been addressed.
- Relevant mentor and judge feedback has been incorporated.
- The application can be reliably demonstrated.
- The deployment/demo environment is prepared.
- Documentation is sufficiently complete.
- The final presentation and demonstration materials are ready.

The key question for this stage is:

> **Can PrithviX reliably demonstrate its intended capabilities in a final evaluation?**

---

# Stage Progression

The four stages represent increasing levels of system maturity:

```text
Foundation
    ↓
Technical Structure
    ↓
MVP
    ↓
Working Processing Pipeline
    ↓
Application
    ↓
Complete User-Facing System
    ↓
Submission
    ↓
Stable, Refined, Competition-Ready System
```

Each stage should build on the previous one rather than attempting to develop the entire system simultaneously.

The progression can therefore be summarized as:

| Stage | Primary Question |
|---|---|
| **Foundation** | Is the technical foundation correctly established? |
| **MVP** | Can the core processing workflow actually work? |
| **Application** | Can a user use the complete system? |
| **Submission** | Is the system stable and refined enough for final evaluation? |

This staged approach allows PrithviX to establish technical feasibility before investing heavily in application development, while leaving a dedicated phase for refinement and feedback-driven improvements before final submission.

---




# Architecture Evolution Across Stages

PrithviX follows a **progressive architectural approach**. The system is initially developed as a monolith to reduce development complexity and allow the core functionality to be established quickly. The architecture is designed from the beginning with the eventual separation into independent services in mind.

This avoids prematurely introducing the operational complexity of a microservice architecture before the boundaries between components have been validated through actual implementation.

## Foundation Stage — Modular Monolith

During the Foundation Stage, PrithviX is implemented as a **modular monolith**.

All major components exist within a single deployable application, but their responsibilities and interfaces are kept clearly separated according to the planned architecture.

The implementation should maintain logical boundaries between components such as:

```text
API
 ├── Image Preprocessing
 ├── OCR
 ├── NLP
 ├── Validation Engine
 ├── Human Verification
 └── Data Management
```

These components should communicate through well-defined internal interfaces rather than being tightly coupled together.

The purpose of this approach is to:

- Keep initial development simple.
- Avoid premature infrastructure and deployment complexity.
- Allow rapid iteration while the core workflow is still being established.
- Validate component responsibilities and interfaces through implementation.
- Make the eventual transition to microservices straightforward.

The Foundation Stage therefore establishes the **logical microservice boundaries without introducing the operational overhead of actual microservices**.

---

## MVP Stage — Microservice Separation

Once the core processing workflow has been established and the component boundaries have been validated, the system transitions from the modular monolith into its intended **microservice architecture**.

The major components are separated into independently deployable services.

The resulting structure should follow the architecture defined in `architecture.md`, for example:

```text
                    API Gateway
                         │
        ┌────────────────┼────────────────┐
        │                │                │
 Image Processing       OCR              NLP
        │                │                │
        └────────────────┼────────────────┘
                         │
                 Validation Engine
                         │
              ┌──────────┴──────────┐
              │                     │
       Human Verification      Data Management
                                    │
                              PostgreSQL/PostGIS
```

Communication between services should use the interfaces and data contracts defined during the Foundation Stage.

The separation should be performed during MVP development rather than immediately at project initialization, allowing the actual implementation experience to inform the final service boundaries.

### Transition Principles

The transition from monolith to microservices should:

- Preserve the established functionality of the pipeline.
- Maintain clear API and data contracts.
- Avoid changing business logic unnecessarily during the migration.
- Separate services according to established responsibilities.
- Allow individual services to be developed, tested, and deployed independently.
- Establish the communication mechanisms required by the final architecture.
- Ensure that the complete end-to-end pipeline continues to function after separation.

The objective is not simply to divide the code into multiple services, but to establish a **functional distributed architecture based on boundaries that have already been validated during development**.

---

## Architectural Progression

The architectural evolution can therefore be summarized as:

```text
Foundation Stage
        │
        ▼
Modular Monolith
        │
        │  Validate boundaries
        │  Validate interfaces
        │  Build core functionality
        ▼
MVP Stage
        │
        ▼
Microservice Architecture
        │
        │  Build complete pipeline
        ▼
Application Stage
        │
        ▼
Complete User-Facing System
        │
        ▼
Submission Stage
        │
        ▼
Stable & Refined System
```

This approach intentionally separates **architectural design** from **architectural deployment**. The microservice architecture is planned from the beginning, while its physical separation is deferred until the MVP stage when the boundaries and requirements are sufficiently understood.