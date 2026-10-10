# Gate ledger

Every gate decision — intent acceptance, spec approval, plan approval, PR
merge, **release authorization**, on-call triage — is a record in
`ledger.jsonl` (one JSON object per line). Records are hash-chained
(`prev_hash`/`hash`), so the ledger is append-only by construction: any edit
to a past record breaks the chain and fails `verify`.

The release gate hook accepts `RELEASE_APPROVAL=ledger:<record-id>` and
verifies that the record is an approved `release_authorization` decision before
allowing a production deploy. An approval for another gate cannot authorize a
release.

Copying the hook does not activate it. Wire it into the agent's tool-use policy
or the deployment pipeline, and protect who may commit release approvals. The
free-form `RELEASE_APPROVAL=<ticket-or-signer>` path is an example stand-in for
a trusted approval service; use ledger-backed or service-verified approvals for
an enforced release gate.

## Record an approval

```bash
python3 scripts/gate_ledger.py record \
  --gate release_authorization \
  --artifact "deploy app v1.2.3 to production" \
  --commit cf13ec7 \
  --approver "Ada (release manager)" \
  --evidence "INC-42 / REL-2026-08-26" \
  --expires-at 2026-08-27T12:00:00Z
git add gates/ledger.jsonl && git commit -m "gates: record release authorization REL-…"
```

- `--gate` uses the gate names from `workflow-graph.yaml` (e.g.
  `product_owner_accept`, `release_authorization`, `on_call_triage`).
- `--expires-at` is optional (ISO-8601 UTC or epoch seconds); a release
  authorization that should not outlive the deploy window should set it.
- The record is only *authoritative* once **committed**: `verify
  --require-committed` (what the gate hook uses) rejects uncommitted ledgers.

## Verify

```bash
python3 scripts/gate_ledger.py verify --record release_authorization-001 \
  --gate release_authorization \
  --require-committed --graph workflow-graph.yaml --require-gates
```

- `--require-committed` — fail unless the ledger is committed and clean.
- `--gate` — fail if the record belongs to another gate.
- `--graph … --require-gates` — fail unless every gate in the workflow graph
  has at least one recorded decision (governance completeness check).

## A project with no deployment

If the project owner explicitly ends a demo after merge, record the scope
decision as `--gate release_authorization --decision rejected` with evidence.
This records that no release was authorized; the production hook rejects the
record. Remove the release and downstream production nodes from that project's
workflow graph. A later deployment needs a fresh, approved release record.

## Tamper detection

`verify` replays the chain: every record's `hash` must match its content and
each `prev_hash` must point at the previous record. Rewriting history (e.g.
changing an approver after the fact) is detected immediately. This is
evidence-grade provenance for compliance: *who approved what, when, with what
evidence* — queryable instead of implicit.
