# projects/ — one card per project (the unified layer)

Each active project gets ONE file here: `projects/<name>.md`. It is the canonical
answer to: **what is this · where does it live · what's done · what's outstanding ·
what's next**. Any AI told *"go look in collab-mem for `<name>` and tell me what's
next"* reads that one file and can act.

## The card schema (keep every card in this shape)

```
# <name> — one-line what
STATUS: active | paused | done        UPDATED: YYYY-MM-DD by <who>
## What        — 3–6 lines: the product/goal + the architecture in one breath
## Where       — repos, hosts, paths, ports, services, access (or a FACTS pointer)
## Done        — compressed history, newest on top, dated
## Outstanding — the honest gap list (bugs, debt, decisions awaiting Tony)
## Next        — ORDERED. Item 1 is THE next action. Say who it needs.
```

## Rules
- **Update the card when state changes** — a stale card is worse than none (it lies).
  EOD ritual now includes: touch the card of any project you worked on today.
- Cards summarise and **point** to deep docs (repo READMEs, HANDOFFs, runbooks) —
  don't duplicate their content here.
- MASTER.md keeps one backlog line per project (`M#`) pointing at its card;
  cross-project priorities live in MASTER, execution detail lives in the card.
- Durable access facts (ssh, keys, walls) stay in FACTS.md — cards link, not repeat.
