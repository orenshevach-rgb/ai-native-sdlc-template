# CLAUDE.md — repository memory

Keep this file under one page. Add a rule when the same mistake happens twice.

> In Codex projects, the same content lives in AGENTS.md; the role is identical.

## Commands

- Build: `make build` (must finish with "Build succeeded")
- Test: `make test` (all green; never skip or delete a failing test)
- Lint: `make lint` (zero warnings)
- Itest: `make itest` (integration, needs docker)

## Verifying your work

Run build, test, and lint before reporting any task complete, and paste the output.
If a test fails, fix the code, not the test.

## Artifact handoffs

- Design: draft `spec.md` as the AI-native SDLC PRD; give each accepted user
  outcome its own acceptance criteria. Include the three AI sections only if
  the product produces AI outputs. Resolve or assign open issues before approval.
- Build: read the approved PRD, map plan proof and tests to each criterion, and
  run the product AI Evaluation Plan and Rubric when applicable.

## Conventions

<Language/framework conventions, formatting, naming.>

## Architecture

<One-paragraph mental model: main modules and data flow.>

## Things the agent gets wrong

<Each mistake, its fix, and how to check for it.>

## Hooks

<Deterministic hooks and what they enforce.>
