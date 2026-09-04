# CARD-CONTRACT.md

Desk OS pilot contract for card work across Hermes, collab-mem, and outside orchestration.

Status: draft v2 (Scribe improvement pass; Brett verified)
Card: `t_9876a69b` / job `desk-os-drill`
Owner: Brett (orchestrator); Rae = KSM on M4 Hermes
Scope: Tony desk OS pilot only

---

## 1. Purpose

One card is the unit of work. Every actor that picks up, moves, blocks, or finishes work cites that card. Hermes shows the board. collab-mem is the durable ledger. Brett (and other desk agents outside Hermes) orchestrate hires and handoffs without living inside Hermes.

This contract is AI-agnostic: it does not assume one vendor, one bot runtime, or one model pool.

---

## 2. Surfaces

### 2.1 Hermes desktop (interactive board)

- Hermes desktop is the interactive Kanban board for the desk.
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

### 3.1 Parent depth limit

- Max parent chain depth: 2 (parent -> child). No grandchildren.
- If work needs a further split, open a sibling card under the same parent, or promote a new parent with Brett's hire.
- Receipts on a child must cite both `card: <child-id>` and `parent: <parent-id>`.
- Hermes parent/child example for this drill: parent `t_9876a69b` / child `t_daab07b9`.

---

## 4. Actors and roles

| Actor class | Role |
| --- | --- |
| Human (Tony) | Approves gates, publishes, owns outcomes |
| Brett | Outside-Hermes orchestrator; hires; routes |
| Hermes / Rae (KSM) | Interactive board and desk control on M4 |
| Specialist agents | Do one job per card (research, draft, edit, package, etc.) |
| Model workers | API or subscription models used as compute; not Hermes bots by default |

Actors may be humans, local agents, API models, or subscription models. The contract does not privilege one pool.

### 4.1 Owner vs verifier

Every live card names both fields. They are not the same role.

| Field | Meaning |
| --- | --- |
| `owner` | Accountable for moving the card to done (or to a clear blocked state). Usually the hired specialist while active; Brett while unassigned. |
| `verifier` | Checks the artifact against the hire brief. May reject back to owner. Does not do the primary work. |

Rules:

- Owner writes progress receipts; verifier writes `submitted` review outcomes (`approved` / `rejected` at the verify layer).
- Human gates (`APPROVE:<id>`) are above verifier: Tony still gates merge / publish / spend.
- One owner at a time. Verifier may be Brett or another named agent.
- If owner and verifier would be the same actor, Brett must rename one of them before `submitted`.

---

## 5. Receipt grammar

Every meaningful step emits a receipt. Keep it short and parseable.

```
receipt:
  card: <card-id or job-slug>
  actor: <who>
  verb: <what-happened>
  evidence: <path, hash, quote, or pointer>
  next: <next-actor-or-gate-or-done>
```

Optional lines when useful:

```
  parent: <parent-card-id>
  owner: <current-owner>
  verifier: <current-verifier>
  at: <UTC ISO timestamp>
```

Rules:

- `card` is required.
- `actor` is the real doer (agent name, human, or model pool label).
- `verb` is past-tense and specific (`drafted`, `blocked`, `approved`, `stale-flagged`).
- `evidence` points at something checkable (file path in collab-mem, commit, artifact id). No vibes.
- `next` names who or what happens after this receipt. Use `done` only when the card is complete and gated if required.

### 5.1 Receipt conflict rule

When two receipts disagree on state for the same card:

1. collab-mem (committed ledger) beats chat-only claims.
2. Later `at:` timestamp wins among ledger receipts on the same verb class.
3. Human gate receipts (`approved` / `rejected` with `APPROVE:<id>`) beat agent receipts.
4. `blocked` / `stuck-escalated` from Brett or Rae beat concurrent `progressed` from an owner until `unblocked`.
5. On conflict, append a `progressed` receipt that cites both evidence pointers and states the chosen truth; do not rewrite history.

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
- `submitted` - ready for verifier or human
- `approved` - verify or human gate passed
- `rejected` - verify or human gate failed; include reason in evidence
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
- Stuck cards stay visible on the board; do not delete history.

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

---

## 10. Hire rules (Brett / orchestrators)

1. Cite the card id in every hire message.
2. One primary job per hire.
3. Name `owner` and `verifier` when known (they must differ before `submitted`).
4. Point at evidence location (collab-mem path or expected artifact name).
5. Do not hire for publish or X actions in this pilot.
6. Name the model pool when cost or capability matters (`pool: api|subscription|local|hermes`).

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
2. Brett verifies (including owner/verifier, receipt conflict, parent-depth).
3. A sample receipt for this draft is attached.
4. File lands in collab-mem via PR (human merge gated by `APPROVE:desk-os-drill`).

---

## Sample receipt (this draft)

```
receipt:
  card: t_9876a69b
  parent: (none - this is parent)
  owner: Scribe
  verifier: Brett
  actor: Scribe
  verb: drafted
  evidence: CARD-CONTRACT.md v2 in PR #1
  next: Brett (verify)
  at: 2026-09-04T15:12:00Z
```

---

## Verify notes (Brett 2026-09-04)

- v2 accepted: closes parent-depth (max 2), owner vs verifier, receipt conflict order.
- Delivery truncated mid section 10 in agent channel; sections 10-12 reconstructed from v1 + new hire rule 3. Scribe may confirm tail if anything material was lost.
- Status: ready for Tony gate `APPROVE:desk-os-drill` before merge.
