# ask-ebbi-3 — the Planning Oracle (canonical repo, live in production)

STATUS: active        UPDATED: 2026-07-20 by Rae (Claude Code)

## What

Cited planning Q&A over UK planning law for consultants — 91,121 PINS appeal precedents,
NotebookLM-style casefiles, style-learned drafting, live planning intelligence, all orchestrated
by **Ebbi**, a dedicated Hermes agent, behind one dashboard. This is the **canonical Ask Ebbi
repo**, consolidated 2026-07-12 from the ask-ebbi-2 lineage plus assets recovered from
production — supersedes [ask-ebbi-2](ask-ebbi-2.md) (kept for history, do not build there).

## Where

- **Repo:** `k3ss-official/ask-ebbi-3` (`git@github.com:k3ss-official/ask-ebbi-3.git`),
  branch **`ui-console-alt`** — latest `2573361` (gitignore: skill package) on top of `4c35c3f`
  (overnight UI rebuild: landing page, brand assets, ops docs, RAE review stack). Both pushed.
- **Live:** `askebbi.com` via a controlled **Cloudflare Tunnel** → canonical M4 Docker instance,
  container **`ebbi`** on port **8051**. Alt UI demos run on separate Docker instances, ports
  8052–8099 (`docs/operations/UI_DEMO_ENVIRONMENTS.md`). VPS remains the intended production
  target, not the current source of truth (`docs/migration/`).
- **Stack:** FastAPI/Uvicorn + Docker; ChromaDB corpus (46,054 chunks) + 91,121 PINS appeals;
  MCP server (8 Library tools); xterm.js CLI; Hermes gateway; Telegram gateway (active).

## Done (newest first)

- **2026-07-20 — overnight UI rebuild pushed.** `4c35c3f` (landing page rebuild + brand assets +
  ops docs + RAE review stack) + `2573361` (gitignore cleanup). Both on `ui-console-alt`, pushed.
- **2026-07-12 — consolidated as the canonical repo** from the ask-ebbi-2 lineage plus
  production-recovered assets; migration/provenance docs added (`docs/migration/`).
- Inherited from ask-ebbi-2: 91,121-case appeals DB, casefiles, 8-tool Library MCP, Ebbi-as-
  backend architecture — see [ask-ebbi-2](ask-ebbi-2.md) → Done for the full pre-consolidation
  history.

## Outstanding

- **Responsiveness fix in progress** — horizontal overflow + iPad portrait layout broken; fix is
  being committed now. Working tree was clean/pushed as of this update — confirm it landed before
  assuming fixed.
- Carried from ask-ebbi-2, not re-verified against this build: no auth in front of the app, no
  streaming bridge (`hermes -z` per message), SOUL citation-leak risk on uncited comparators.

## Next (ordered)

1. **Land + verify the responsiveness fix** (horizontal overflow + iPad portrait) on
   `ui-console-alt`, push.
2. Confirm live `askebbi.com` renders correctly post-fix on mobile + iPad portrait.
3. Re-check auth / streaming / citation items carried from ask-ebbi-2 against the current
   canonical build; update this card once confirmed either way.
