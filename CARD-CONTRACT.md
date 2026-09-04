# CARD-CONTRACT.md

Desk OS pilot contract for card work across Hermes, collab-mem, and outside orchestration.

Status: draft (Scribe T4; Brett verified with notes)
Card: `t_9876a69b` / job `desk-os-drill`
Owner: Brett (orchestrator); Rae = KSM on M4 Hermes
Scope: Tony desk OS pilot only

---

## 1. Purpose

One card is the unit of work. Every actor that picks up, moves, blocks, or finishes work cites that card. Hermes shows the board. collab-mem is the durable ledger. Brett (and other desk agents outside Hermes) orchestrate hires and handoffs without living inside Hermes.

This contract is AI-agnostic: it does not assume one vendor, one bot runtime, or one model pool.

---

## 2. Surfaces

### 2.1 Hermes desktop (interactive Kanban)

- Hermes desktop is the interactive Kanban UI for the desk.
- Rae runs as KSM (Kanban / desk control) on the M4.
- Columns and card moves in Hermes are the live operator view.
- Hermes is not the source of truth for history; it is the working surface.

### 2.2 collab-mem (git markdown ledger)

- collab-mem is a git-backed markdown ledger.
- Card state, receipts, approvals, and decisions that must survive a restart live here as files.
- Prefer small, append-friendly markdown over opaque blobs.
- Commit messages and file paths should cite the card id when the change is card-scoped.

### 2.3 Outside Hermes (orchestration)

- Brett orchestrates outside Hermes.
- Desk agents, API workers, and subscription-model workers may run off-Hermes.
- Every hire, dispatch, or handoff cites a card id. No orphan workstreams.

---

## 3. Card identity

- Format: short opaque id (example: `t_9876a69b`).
- Human-facing job slug may also be cited (example: `desk-os-drill`).
- One job per card unless Brett explicitly splits.
- Child work cites the parent card id in receipts and hire notes.
- Card titles are human labels; the id is the join key across Hermes, collab-mem, and chat.

---

## 4. Actors

| Actor class | Role |
| --- | --- |
| Human (Tony) | Approves gates, publishes, owns outcomes |
| Brett | Outside-Hermes orchestrator; hires; routes |
| Hermes / Rae (KSM) | Kanban UI and desk control on M4 |
| Specialist agents | Do one job per card (research, draft, edit, package, etc.) |
| Model workers | API or subscription models used as compute; not Hermes bots by default |

Actors may be humans, local agents, API models, or subscription models. The contract does not privilege one pool.

---

## 5. Receipt grammar

Every meaningful step emits a receipt. Keep it short and parseable.

```
receipt:
  card: <card-id>
  actor: <who>
  verb: <what-happened>
  evidence: <path, hash, quote, or pointer>
  next: <next-actor-or-gate-or-done>
```

Rules:

- `card` is required.
- `actor` is the real doer (agent name, human, or model pool label).
- `verb` is past-tense and specific (`drafted`, `blocked`, `approved`, `stale-flagged`).
- `evidence` points at something checkable (file path in collab-mem, commit, artifact id). No vibes.
- `next` names who or what happens after this receipt. Use `done` only when the card is complete and gated if required.
- Prefer a UTC ISO timestamp line when the writer can add one (`at: 2026-09-04T15:09:00Z`).

---

## 6. Lifecycle verbs (minimum set)

- `opened` - card created
- `hired` - actor assigned; must cite card id
- `started` - work begun
- `progressed` - meaningful update with evidence
- `blocked` - cannot proceed; name the blocker
- `unblocked` - blocker cleared
- `stale-flagged` - no progress within SLA
- `stuck-escalated` - blocked or stale past escalate window
- `submitted` - ready for human or verifier
- `approved` - human gate passed
- `rejected` - human gate failed; include reason in evidence
- `done` - card complete
- `cancelled` - abandoned with reason

---

## 7. Stale / blocked / stuck

### 7.1 Blocked

- Card cannot move without an external dependency (approval, missing input, access, decision).
- Receipt verb: `blocked`.
- Evidence must name the blocker.
- `next` must name who can clear it (usually human or Brett).

### 7.2 Stale

- No receipt / board progress within the pilot SLA (default: 24h wall clock unless the card sets a tighter window).
- Any orchestrator (Brett) or Hermes KSM may `stale-flagged`.
- Stale is a signal, not a punishment. Ping the actor once; if silent, reassign or escalate.

### 7.3 Stuck

- Blocked or stale past the escalate window (default: 48h, or 2x SLA if the card sets one).
- Receipt verb: `stuck-escalated`.
- `next`: Brett + human if a gate is involved.
- Stuck cards stay visible on the Kanban; do not delete history.

### 7.4 Recovery

- Clear with `unblocked` or a fresh `hired` / `started` after reassignment.
- Always leave a receipt so collab-mem shows the gap and the fix.

---

## 8. Human gates

- Publish, spend, external send, merge to main on pilot ledgers, and other irreversible desk actions stay human-gated.
- Gate token format for this pilot job: `APPROVE:desk-os-drill` (job slug). Card-id form `APPROVE:t_9876a69b` is also valid when the card is the only handle.
- Only Tony can satisfy the gate unless the card names a delegate.
- Agents may prepare packages and ask for the gate; they do not self-approve.
- Rejection uses `rejected` with reason in evidence; card returns to the owning actor or Brett.
- No agent publishes. No agent posts to X. This pilot is markdown / desk-internal until a human says otherwise.

---

## 9. Model pools (varied compute)

- Work may use API models, subscription models, local models, or Hermes-hosted bots.
- Do not assume a Hermes bot is the only worker type.
- Hire notes should name the pool when it matters for cost, latency, or capability:
  - `pool: api`
  - `pool: subscription`
  - `pool: local`
  - `pool: hermes`
- Receipts stay the same across pools. The card id is the join key, not the runtime.

---

## 10. Hire rules (Brett / orchestrators)

1. Cite the card id in every hire message.
2. One primary job per hire.
3. Name verifier when known (example: Verifier: Brett).
4. Point at evidence location (collab-mem path or expected artifact name).
5. Do not hire for publish or X actions in this pilot.

---

## 11. Constraints (pilot)

- Markdown only for contract and ledger artifacts.
- No X.
- No publish by agents.
- AI-agnostic wording in docs and receipts.
- ASCII hyphens only in contract text (no en/em dashes).

---

## 12. Minimal acceptance for this card

This `CARD-CONTRACT.md` is accepted when:

1. Sections 1-11 are present and unambiguous enough to run a drill.
2. Brett verifies.
3. A sample receipt for this draft is attached.
4. File lands in collab-mem via PR (human merge gated).

---

## Verify notes (Brett 2026-09-04)

- Accepted: surfaces split, receipt grammar, pools, no-publish, 24h stale (beats the 2h draft).
- Fixed in this fold: Owner DT renamed to Brett/Tony; gate token documents job-slug `APPROVE:desk-os-drill`.
- Still open for next pass: owner vs verifier fields; receipt conflict rule; parent-depth limit (Mercury/NVIDIA critiques).

