# Org protocol — how the agents run

The org is a thin operating layer on top of the AI-native SDLC loop. Roles
execute the same phases and gates; this protocol adds reporting, peer review,
and escalation so the loop runs with less human steering.

## Reporting

1. Every agent commits its artifact (intent.md, spec.md, plan.md, code+tests,
   review findings) with evidence.
2. When an agent accepts or finishes work, it updates `org/status.yaml`
   (busy/idle, assignment, last_report) in the same commit, using
   `scripts/org_status.py` so the org-chart rules are enforced.
3. Reports flow up one level: engineers → CTO; PM and product → CTO; CTO →
   CEO. Routine reporting is the status file plus artifact commits; the CEO
   reads them at the gates and on escalation.

## Peer review

1. A writer that finishes an artifact requests a review from the CTO.
2. The CTO assigns an idle reviewer from `org/status.yaml` and marks it busy.
   If no reviewer is idle, the request waits in the review queue.
3. The reviewer writes `org/reviews/<artifact>-<id>.md` with severity-ranked,
   evidence-backed findings and a verdict (`approved` | `changes`).
4. The writer addresses findings; a second review round happens if needed.
5. Two unresolved rounds → CTO decides; if the artifact is a spec or plan and
   the conflict persists → CEO.

`org_status.py review assign|submit|escalate` records every queue transition;
hand-editing the queue is not supported.

## Escalation

- Intent ambiguity → PM → CEO.
- Spec/plan disagreement → writer ↔ reviewer (max 2 rounds) → CTO → CEO.
- Anything that would cross a gate without human approval → stop and report.

## Gates

| Gate | Who decides |
|---|---|
| intent | PM first review; CEO only on escalation |
| spec | peer review first; human product owner (CEO) approves the PRD before Build |
| plan | peer review; CEO on unresolved disagreement |
| pr_merge | human (CEO) |
| release | human (CEO) — production-gate.sh + gate ledger record |

If the CEO ends a project after merge, record a rejected release decision and
remove release and production monitoring nodes from that project's graph. The
production hook must still block deployment; a future release needs a new
authorization.

## PRD handoff

The product manager writes `spec.md` from the accepted intent using the PRD
template. Each outcome gets its own acceptance criteria. The reviewer checks
outcome coverage, scope, open issues, and verification; for AI products, the
review also checks that each AI output appears in AI Failure Modes, Evaluation
Plan, and Evaluation Rubric. The CEO approves or rejects the PRD after peer
review. The engineer then maps the approved criteria and any product AI
evaluations to plan proof and tests. Product AI evaluations do not replace the
agent-configuration eval suite.

## Intake

- GitHub issues: `scripts/sync_issues.py pull` → `org/intake/github/<repo>-<n>.md`
- Forms/email: `scripts/intake.py add --source form|email …` writes
  `org/intake/forms/<record>.md` / `org/intake/email/<record>.md`.

The product engineering agent consolidates intake records, files tickets
(`scripts/sync_issues.py push`), and drafts intents for PM review.
