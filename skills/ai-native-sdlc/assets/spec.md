# <Title> — Product Requirements and Design (PRD)

- Derived from: intent.md <commit or path>
- Author: <agent + human reviewer>
- Status: Draft | Approved
- Date: <YYYY-MM-DD>

## Overview

<Summarize the problem, target users, accepted intent, and scope of this spec.>

## Outcomes

### Outcome 1 — <short name>

- User outcome: As a <user>, I can <capability>, so I can <benefit>.
- Acceptance criteria:
  1. <Observable behavior or result that proves this outcome.>
  2. <Another criterion, if needed.>

<Repeat this outcome block for every user outcome in intent.md. Give each outcome its own acceptance criteria.>

## Non functional requirements

<Performance, security, privacy, accessibility, reliability, compliance, and operability constraints that apply.>

## Out of scope

<What this version will not do; keep consistent with the accepted intent.>

## Designs

<Proposed experience, architecture, data flow, interfaces, and rationale. Link mocks or diagrams when available.>

## Open issues

<Unresolved decisions and concerns, with an owner and resolution path. Say "None" when clear.>

## Verification plan

<How the acceptance criteria and non functional requirements will be checked during Build and review.>

## Other

<Relevant dependencies, trade-offs, risks, standards applied, rollout notes, or decisions that do not fit above.>

<!-- Include the next three sections only when the product produces AI outputs. Remove them for non-AI products. Product AI evaluations are distinct from Build verification and agent-workflow evals. -->

## AI Failure Modes

<For each AI output, list credible failure modes and rate likelihood and impact using a defined scale.>

| Output | Failure mode | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| <AI output> | <How it could be wrong or unsafe> | <Low/Medium/High> | <Low/Medium/High> | <Prevention, detection, or recovery> |

## Evaluation Plan

<Enumerate every AI output, define quality for each, and say how it will be measured.>

| AI output | What quality means | Evaluation method |
|---|---|---|
| <AI output> | <Observable quality criteria> | <Dataset, human review, automated check, or metric> |

## Evaluation Rubric

<Define these three levels separately for every AI output so plausible-but-wrong results are recognizable.>

### <AI output>

- Good: <Correct and useful result.>
- Subtly wrong / plausible-but-wrong: <Looks credible but misses a material fact, constraint, or user need.>
- Clearly broken: <Unusable, invalid, or unsafe result.>
