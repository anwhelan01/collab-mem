# chadai — UK consumer-rights fact-finder (sealed, hold-nothing Hermes product)

STATUS: active        UPDATED: 2026-07-17 by ChadAI Codex

## What

Chad is the sealed, hold-nothing UK consumer-rights product. Its orchestrator and Retriever/Auditor
workers are operated by the separate ChadAI Codex lane. It is a distinct tenant from Ask Ebbi; the
shared coordination layer is this Git ledger, not shared runtime memory or data.

## Where

- **Runtime host:** `ssh alwyz-01-dock`
- **Compose project:** `/home/anwhelan/apps/chadai-prod`
- **Current stack:** healthy ChadAI web, Hermes, admin, Caddy, internal Open Notebook and Factbox/SurrealDB services.
- **Private networks:** `chadai_core`, `chadai_edge`, `chadai_admin_egress`
- **Persistent resources:** ChadAI-owned `chadai-*` volumes and secrets; do not attach Ask Ebbi.
- **Deep cursor:** `/Volumes/deep-1t/Users/k3ss/k3ss-official/ChadAI/HANDOFF.md`

## Done

- ChadAI has a dedicated Docker tenant on the new two-tenant VPS boundary.
- Its internal Open Notebook remains private to ChadAI and is pinned/configured as part of that stack.
- Existing hold-nothing and isolation requirements remain the ChadAI Codex's responsibility.

## Outstanding

- ChadAI Codex maintains its own build roadmap and verifies its own health, source and GDPR controls.
- Any shared-source or cross-product integration requires an explicit Tony-approved boundary; no implicit
  sharing with Ask Ebbi is permitted.

## Next

1. ChadAI Codex continues its current roadmap from `ChadAI/HANDOFF.md`.
2. Keep the ChadAI Compose project, Hermes state, databases, Open Notebook and secrets isolated from Ask Ebbi.
