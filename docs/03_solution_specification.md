# Solution Specification — AI Complaint Automation

## Overview

This solution introduces an AI-assisted automation system for handling customer complaints at Metalpol.

The system integrates email ingestion, AI-based processing, SAP data retrieval, and JIRA ticket creation.

---

## Architecture

- Backend: Python (FastAPI)
- Email ingestion: Microsoft Graph API
- AI module: LLM (e.g. OpenAI / Claude)
- ERP integration: SAP REST API
- Ticketing: JIRA REST API
- Database: PostgreSQL
- Storage: Azure Blob Storage

---

## System Flow

1. Email received via Graph API webhook
2. Email content and attachments extracted
3. LLM processes text:
   - extracts structured data
   - classifies defect type
4. SAP API is queried:
   - order data
   - batch information
5. Decision engine evaluates:
   - confidence level
   - completeness of data
6. JIRA ticket created automatically
7. Response sent to customer
8. Data stored for analytics

---

## AI Components

LLM is used for:
- extracting structured data from emails
- classifying complaint type
- generating response drafts

LLM is NOT used for:
- business-critical decision making
- system integration logic

---

## Decision Logic

- High confidence → automatic processing
- Low confidence → human review
- Missing data → request additional information

---

## Trade-offs

- Automation vs accuracy → human fallback required
- LLM flexibility vs unpredictability → validation layer needed
- Speed vs cost → selective AI usage

---

## Future Improvements

- Train ML model for classification using collected data
- Image-based defect detection (computer vision)
- Advanced analytics dashboard
- Integration with production monitoring systems

---

## Summary

The proposed solution focuses on practical automation using AI where it provides the most value, while maintaining control through rule-based logic and human oversight.