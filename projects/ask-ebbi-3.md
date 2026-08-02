# ask-ebbi-3 — Ask Ebbi planning intelligence product (the canonical repo)

STATUS: paused        UPDATED: 2026-08-01 by overnight documentation sweep

## What

Ask Ebbi is a Hermes-native planning research and drafting assistant for UK planning consultants:
cited Q&A over UK planning law, 91,121 PINS appeal precedents, NotebookLM-style casefiles, and
style-learned drafting, orchestrated by **Ebbi**, a dedicated Hermes agent, behind one dashboard.
The dashboard is thin glass over the Hermes orchestrator + specialist roles; the Ask Ebbi Library
owns deterministic retrieval and citations. **This is one product** — no separate LandPro product
or roadmap. Consolidated 2026-07-12 into this repo from the ask-ebbi-2 lineage plus assets
recovered from production; historical predecessor retained at [ask-ebbi-2](ask-ebbi-2.md) for
provenance only.

## Where

- **Canonical local repo:** `/Volumes/deep-1t/Users/k3ss/k3ss-official/ask-ebbi-3/`
- **Canonical GitHub repo:** `k3ss-official/ask-ebbi-3` (private) — branch **`ui-console-alt`**,
  latest `323db65` (recover Hermes runtime on shadowed volumes), on top of the previously recorded
  `2573361` (gitignore: skill package) and `4c35c3f` (overnight UI rebuild: landing page, brand
  assets, ops docs, RAE review stack). The branch has diverged from `main`; verify before any
  reopening or deployment.
- **M4 staging:** container **`ebbi`** on `127.0.0.1:8051`; UI demo `ebbi-rae` on `127.0.0.1:8052`
  (further alt demos on 8053–8099, `docs/operations/UI_DEMO_ENVIRONMENTS.md`).
- **Public beta:** `https://askebbi.com` via a controlled **Cloudflare Tunnel** (health endpoint
  verified HTTP 200). Telegram gateway active.
- **VPS placement:** `alwyzon-1` is no longer an Ask Ebbi target. Tony shelved
  Ask Ebbi on 2026-07-30 and dedicated that host to KSM.
- **Stack:** FastAPI/Uvicorn + Docker; ChromaDB corpus (46,054 chunks) + 91,121 PINS appeals + 3
  style documents; MCP server (8 Library tools); xterm.js CLI; Hermes gateway; inference on the
  Nous `tencent/hy3:free` lane (OpenRouter/Ollama not part of the active architecture).

## Done (newest first)

- **2026-07-30 — shelved by Tony.** The approved `alwyzon-1` cleanup removed
  Ask Ebbi/ChadAI/Cloudflare/Docker residue from that host after a verified
  archive was copied to the local 2 TB backup volume. No Ask Ebbi VPS deployment
  is active.
- **2026-07-20 — overnight UI rebuild pushed.** `4c35c3f` (landing page rebuild + brand assets +
  ops docs + RAE review stack) + `2573361` (gitignore cleanup). Both on `ui-console-alt`, pushed.
- **2026-07-17 (Ask Ebbi Codex)** — canonical repo confirmed as future source of truth; primary
  (`8051`) and demo (`8052`) UI containers isolated by port; casefile event-loop defect fixed and
  casefile/list/appeals health checks verified; planning-intelligence feed + delegation-visibility
  stub added; local test suite 14 passed; public admin/docs/OpenAPI/CLI surfaces blocked from
  public ingress; `collab-mem` confirmed as the off-box coordination ledger (not app memory).
- **2026-07-12 — consolidated as the canonical repo** from the ask-ebbi-2 lineage plus
  production-recovered assets; migration/provenance docs added (`docs/migration/`).
- Inherited from ask-ebbi-2: 91,121-case appeals DB, casefiles, 8-tool Library MCP, Ebbi-as-
  backend architecture — see [ask-ebbi-2](ask-ebbi-2.md) → Done for the full pre-consolidation
  history.

## Outstanding

- **Paused:** do not resume build, deployment, Cloudflare or runtime work until
  Tony explicitly reopens the project.
- **Responsiveness status is unverified** — horizontal overflow + iPad portrait layout was reported
  broken during the earlier UI work, but the project is paused and the fix has not been re-verified.
  Do not assume it landed until the project is reopened and the current branch is checked.
- Reconcile the canonical Git branch(es) and any uncommitted UI/research work before VPS
  deployment (`ui-console-alt` vs. whatever Ask Ebbi Codex is tracking — check for drift).
- Defer the dedicated Ask Ebbi Compose build until Tony reopens the project and selects a new
  runtime target; `alwyzon-1` is dedicated to KSM and must not be targeted.
- Replace the beta CLI subprocess transport with an authenticated Hermes gateway boundary once
  the local beta is stable; finish app auth + operational hardening before wider public access.
- Verify Telegram round-trip end-to-end; resolve the separate `rae.askebbi.com` DNS/demo record
  if that demo is still needed.
- Decide the legacy-citation source-of-truth/vault boundary (append-only provenance) — ChadAI's
  Open Notebook is not automatically the Ask Ebbi SOT.
- Register free acquisition channels (APIs, email signups, downloads, RSS/Atom, crawler adapters)
  with licensing/provenance checks.
- Carried from ask-ebbi-2, not independently re-verified: SOUL citation-leak risk on uncited
  comparators.

## Next (ordered)

1. **Tony:** explicitly reopen Ask Ebbi and select its new runtime target.
2. On reopening, verify local/GitHub/container/Cloudflare state before trusting
   the 2026-07-20 status above; then resume from the responsiveness and branch
   reconciliation work without targeting `alwyzon-1`.
