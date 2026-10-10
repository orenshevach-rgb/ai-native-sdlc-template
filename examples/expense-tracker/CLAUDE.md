# CLAUDE.md — expense-tracker repository memory

Keep this file under one page. Add a rule when the same mistake happens twice.

## Commands

- Build: `make build` (must finish with "Build succeeded")
- Test: `make test` (all green; never skip or delete a failing test)
- Lint: `make lint` (zero warnings)
- Itest: `make itest` (integration, needs docker)

## Verifying your work

Run build, test, and lint before reporting any task complete, and paste the output.
If a test fails, fix the code, not the test.

## Artifact handoffs

- Read the accepted intent and approved `spec.md` PRD before planning. Map
  each outcome's acceptance criteria to plan proof and tests.
- This expense tracker has no product AI outputs, so its PRD does not need AI
  Failure Modes, Evaluation Plan, or Evaluation Rubric sections.

## Conventions

- TypeScript/React on the portal, FastAPI for claims-api. No new state management library.
- Money is always Decimal, never float.
- Every endpoint needs an integration test in `claims-api/tests/`.

## Architecture

- `portal/` renders and talks only to claims-api.
- `claims-api/routes/` = HTTP, `services/` = domain logic, `adapters/` = ERP and storage.
- Export job runs nightly from the status store.

## Things the agent gets wrong

- Do not bump dependency versions; the platform team owns them.
- Receipt images must be stripped of EXIF before storage — the security skill enforces it.

## Hooks

- `production-gate.sh` blocks prod deploys without release authorization.
- A test-file hook blocks edits to tests during fix tasks.
