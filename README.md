# Metalpol Complaint AI Automation

## Overview

This repository contains a proposed AI-assisted automation solution for the complaint handling process at Metalpol, a fictional automotive manufacturing company.

The project focuses on process analysis, AI integration, and system automation design.

The main goal is to reduce manual work, improve consistency, accelerate customer response time, and provide better operational visibility.

---

## Business Context

Metalpol currently handles customer complaints manually using:
- email inboxes,
- Excel spreadsheets,
- SAP ERP,
- JIRA tickets.

The current process suffers from:
- delayed or missed complaint emails,
- inconsistent defect categorization,
- manual data entry,
- lack of integration between systems,
- limited analytics and reporting.

---

## Proposed Solution

The proposed solution introduces:
- automated email ingestion,
- AI-assisted complaint classification,
- SAP integration,
- automatic JIRA ticket creation,
- centralized complaint storage,
- analytics-ready structured data.

A human-in-the-loop approach is used for low-confidence or complex cases.

---

## Repository Structure

```text
docs/
├── 01_as_is_event_storming.md
├── 02_to_be_event_storming.md
└── 03_solution_specification.md

diagrams/
├── as_is_event_storming.mmd
└── to_be_event_storming.mmd
```

---

## Documents

### 01_as_is_event_storming.md
Analysis of the current complaint handling process, including:
- business flow,
- bottlenecks,
- manual operations,
- process issues.

### 02_to_be_event_storming.md
Proposed future-state process with AI automation and system integrations.

### 03_solution_specification.md
Technical solution proposal describing:
- architecture,
- integrations,
- AI usage,
- decision logic,
- trade-offs,
- future improvements.

---

## Main Technologies

| Area | Technology |
|---|---|
| Backend | Python / FastAPI |
| AI | LLM (OpenAI / Claude) |
| Email Integration | Microsoft Graph API |
| ERP Integration | SAP REST API |
| Ticketing | JIRA REST API |
| Database | PostgreSQL |
| File Storage | Azure Blob Storage |

---

## AI Usage

LLM is used for:
- extracting structured complaint data from emails,
- defect categorization,
- generating response drafts.

Rule-based logic is used for:
- business-critical decisions,
- validation,
- workflow orchestration.

---

## Design Principles

- Automation should reduce repetitive manual work.
- Human oversight remains necessary for uncertain or complex cases.
- AI should support decision-making, not fully replace it.
- The system should be modular and scalable.

---

## Trade-offs

| Decision | Trade-off |
|---|---|
| LLM classification | Flexible but less predictable |
| Human fallback | Higher reliability but lower automation |
| MVP-first approach | Faster delivery but limited functionality |

---

## Future Improvements

Potential future extensions:
- ML-based classification trained on historical complaint data,
- computer vision defect analysis,
- advanced analytics dashboards,
- integration with production monitoring systems.

---

## Project Goal

This repository is not a production-ready implementation.

It is a conceptual and architectural proposal demonstrating:
- system thinking,
- AI automation strategy,
- process optimization,
- integration planning.

---

## Author

Paulina
