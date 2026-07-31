# chadai — UK consumer-rights fact-finder (sealed, hold-nothing Hermes product)

STATUS: paused        UPDATED: 2026-07-31 by overnight documentation sweep

## What

Chad is the sealed, hold-nothing UK consumer-rights product. Its orchestrator and Retriever/Auditor
workers are operated by the separate ChadAI Codex lane. It is a distinct tenant from Ask Ebbi; the
shared coordination layer is this Git ledger, not shared runtime memory or data.

## Where

- **Former runtime host:** `ssh alwyz-01-dock`
- **Former Compose project:** `/home/anwhelan/apps/chadai-prod`
- **Current placement:** no ChadAI workload remains on the former KSM host after
  the approved 2026-07-30 shelf/cleanup. The next runtime target is not yet
  recorded; do not treat the former stack, networks or volumes as live.
- **Deep cursor:** `/Volumes/deep-1t/Users/k3ss/k3ss-official/ChadAI/HANDOFF.md`

## Done

- ChadAI had a dedicated Docker tenant on the former two-tenant VPS boundary.
- Its internal Open Notebook and persistent resources were private to ChadAI;
  their former host placement is now historical.
- Existing hold-nothing and isolation requirements remain the ChadAI Codex's responsibility.

## Outstanding

- **Paused:** ChadAI Codex must select and verify a new runtime target before
  treating the former deployment notes as current.
- ChadAI Codex maintains its own build roadmap and verifies its own health, source and GDPR controls.
- Any shared-source or cross-product integration requires an explicit Tony-approved boundary; no implicit
  sharing with Ask Ebbi is permitted.

## Next

1. **Tony:** explicitly reopen ChadAI and select its new runtime target.
2. On reopening, ChadAI Codex continues from `ChadAI/HANDOFF.md` after a fresh
   local/GitHub/runtime verification.
3. Keep ChadAI state, databases, Open Notebook and secrets isolated from Ask Ebbi.
