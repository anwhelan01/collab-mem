# Card Contract (draft — desk-os-drill)

## Boards
- Interactive UI: Hermes desktop Kanban (Rae = KSM on M4).
- Ledger: anwhelan01/collab-mem (markdown in git).
- If it is not on a card + receipt, it did not happen.

## Card fields
id, title, owner, verifier, status (todo|ready|running|blocked|review|done), parent, pool (hermes|cursor|grok-bot|api|subscription|cli|human)

## Receipt (required on finish)
card / actor / verb(claimed|blocked|unblocked|done|failed|stale) / evidence / next

## Stale / blocked
- No receipt in 2h while running => KSM flags stale.
- Blocked = waiting on human or dependency; must name the waiter.
- Human merge/money/secrets: APPROVE:<card-id> in Brett chat.

## Hiring outside Hermes
Brett briefs cite card id. Worker returns receipt to Brett; Brett/Rae update board + collab-mem daily.
