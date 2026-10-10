# Expense tracker self-service

- Author: A. Chen (finance ops)
- Source: finance-ops Slack #tooling-ideas
- Status: Accepted
- Date: 2026-08-20

## Problem

Employees submit expense reports by emailing PDFs to finance. Approvers spend roughly a third of their time matching receipts to policy rules by hand, and employees can't see where a report stands.

## User Outcomes

- As an employee, I can submit an expense and see its status, next step, and expected date, so I know what to do and when to expect a decision.
- As an approver, I can see a prioritized queue with policy violations flagged, so I can review reports quickly and consistently.

## Affected users and systems

Employees, approvers, finance ops; expense policy documents; existing payroll/ERP export integration.

## Constraints

No new PII beyond existing payroll data. Existing SSO only. Must export in the current ERP format. Mobile-first is out of scope.

## Out of scope

Corporate card reconciliation, per-diem automation, native mobile app.

## Open questions

Should bulk receipt upload be supported in v1? Which policy rules are non-negotiable versus advisory?
