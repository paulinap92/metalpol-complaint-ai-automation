# Solution Specification — AI Complaint Automation

## Overview

This solution introduces an AI-assisted automation system for handling customer complaints at Metalpol.

The proposed architecture combines email automation, AI-assisted processing, SAP-based validation, and automatic JIRA ticket creation.

The primary goal is to reduce repetitive manual work, improve complaint categorization consistency, and accelerate customer response time while preserving human oversight for uncertain cases.

---

## Business Context

Metalpol currently processes complaints manually using email, Excel spreadsheets, SAP ERP, and JIRA.

The current process suffers from:
- delayed or missed complaint emails,
- inconsistent defect categorization,
- repetitive manual work,
- lack of integration between systems,
- limited reporting and operational visibility.

The case description also states that approximately 85% of complaints can be resolved using SAP production and batch data. Therefore, SAP validation becomes the central decision point of the automation flow.

---

## Architecture

| Component | Purpose |
|---|---|
| Python / FastAPI | Main orchestration and integration layer |
| Microsoft Graph API | Email ingestion via webhook |
| LLM (OpenAI / Claude) | Processing unstructured complaint content |
| SAP REST API | Order and batch verification |
| JIRA REST API | Complaint and correction ticket management |
| PostgreSQL (read-only) | Customer information lookup |
| Azure Blob Storage | Complaint image archive using SAS-token access |

---

## System Flow

1. A complaint email is received through Microsoft Graph API webhook integration.
2. Email content and attachments are extracted automatically.
3. Complaint images are archived in Azure Blob Storage.
4. LLM processes multilingual email content and attached images:
   - extracts structured complaint information,
   - identifies probable defect category,
   - generates response draft suggestions.
5. SAP API is queried:
   - order information,
   - batch information,
   - production-related data.
6. Customer information is retrieved from PostgreSQL.
7. Validation and routing logic evaluates:
   - completeness of complaint data,
   - SAP verification results,
   - AI confidence level.
8. If validation succeeds:
   - JIRA Complaint ticket is created automatically,
   - customer receives automatic response.
9. If validation fails or confidence is low:
   - complaint is assigned to a service specialist.
10. If defect is confirmed:
   - JIRA Correction ticket is created for the quality department.

---

## AI Components

LLM is used for:
- extracting structured information from multilingual emails,
- identifying probable defect categories,
- processing attached complaint images,
- generating customer response drafts.

AI is NOT responsible for:
- final business-critical decisions,
- workflow orchestration,
- ERP validation logic.

Business-critical validation relies primarily on SAP production and batch data.

---

## Validation and Routing Logic

The system uses deterministic rule-based validation logic to decide whether the complaint can be processed automatically.

Example conditions:
- missing order ID → manual review,
- low AI confidence → manual review,
- incomplete SAP data → manual review,
- successful SAP verification → automatic processing.

This approach reduces the risk of incorrect automated decisions.

---

## Example Validation Logic

Simplified example:

```python
def evaluate_complaint(
    order_exists: bool,
    sap_match: bool,
    ai_confidence: float,
    has_images: bool,
    has_description: bool
) -> str:

    if not order_exists:
        return "manual_review"

    if not has_description:
        return "manual_review"

    if ai_confidence < 0.75:
        return "manual_review"

    if not sap_match:
        return "manual_review"

    return "automatic_processing"
```

---

### Operational Considerations

The solution should support seasonal complaint peaks reaching up to ~2000 complaints per month.

Since most complaints contain image attachments (typically 2–5 MB each), complaint images should be archived asynchronously in Azure Blob Storage to avoid blocking the main processing flow.

SAP API requests should be controlled carefully because the ERP system exposes a rate limit of 100 requests per minute.

To avoid SAP overload during seasonal peaks, complaint processing should be asynchronous and requests should be throttled or queued before accessing SAP services.

The system uses SAP order, batch, and production information to verify whether the complaint is likely valid before deciding whether the case can be processed automatically or should be assigned to a service specialist.

Low-confidence or incomplete complaints should always be routed to manual review to reduce the risk of incorrect automated decisions.
---

## Trade-offs

| Decision | Trade-off |
|---|---|
| LLM-based classification | Flexible for multilingual and unstructured data, but less predictable |
| Human fallback | Higher reliability, but lower automation level |
| SAP-first validation | More reliable decisions, but dependent on ERP availability |
| MVP-first approach | Faster implementation, but fewer advanced features initially |

---

## Future Improvements

Potential future extensions:
- supervised ML classification trained on historical complaint data,
- image similarity search for recurring visual defects,
- advanced operational dashboards,
- integration with production monitoring systems,
- automated trend analysis for defect recurrence.

---

## Summary

The proposed solution focuses on practical enterprise automation rather than full AI autonomy.

AI is used where it provides the most value — processing unstructured complaint content — while core business validation remains based on ERP production data and deterministic workflow logic.

The architecture balances automation efficiency, operational reliability, and realistic implementation complexity.