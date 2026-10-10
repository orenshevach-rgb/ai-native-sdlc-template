# Expense tracker self-service — Product Requirements and Design (PRD)

- Derived from: intent/intent.md (commit a1b2c3d)
- Author: A. Chen + agent
- Status: Approved
- Date: 2026-08-21

## Overview

Employees submit expense reports by emailing PDFs, approvers manually match receipts to policy, and employees cannot see report status. This spec covers self-service submission and review for employees and approvers.

## Outcomes

### Outcome 1 — Employee submission and visibility

- User outcome: As an employee, I can submit an expense and see its status, next step, and expected date, so I know what to do and when to expect a decision.
- Acceptance criteria:
  1. An authenticated employee can submit an expense with amount, date, category, receipt image, and optional note.
  2. The employee can see draft, submitted, in review, approved, rejected, or paid status, plus the next step and expected date.

### Outcome 2 — Approver review

- User outcome: As an approver, I can see a prioritized queue with policy violations flagged, so I can review reports quickly and consistently.
- Acceptance criteria:
  1. The queue sorts reports by age and visibly flags policy violations.
  2. Each flag distinguishes non-negotiable from advisory policy rules.

## Non functional requirements

- Existing SSO only; no new PII in session data.
- ERP export format unchanged.
- p95 latency under 500 ms for submit and status endpoints.

## Out of scope

Corporate card reconciliation, per-diem automation, native mobile app, and bulk receipt upload in v1.

## Designs

- Web app: `portal/src/expenses/` (React); API: `claims-api/routes/expenses.py` (FastAPI); policy engine as a library.
- Data flow: submit → validate → policy rules → status store → approver queue; nightly export job to ERP.
- Interfaces: `POST /expenses`, `GET /expenses`, `GET /expenses/{id}/status`; export job reads the status store.

## Open issues

Bulk upload is deferred to v2; A. Chen owns the follow-up decision.

## Verification plan

- Unit tests for the policy engine covering all six statuses and their transitions.
- Integration test for submit → status.
- Screenshot of the status panel matches the approved mock.

## Other

- Security skill: SSO enforced; receipt images stripped of EXIF metadata before storage.
- UX skill: status and next step visible without opening the report.
- Policy conflict: "meals over $75 require an itemized receipt" versus "scanned receipts only" — escalated to the policy owner; decision: itemized photos accepted as PDF attachment in v1.
- ERP export is weekly and scheduled; paid status may lag by up to a day.
