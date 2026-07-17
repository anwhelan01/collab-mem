# ask-ebbi-3 — Ask Ebbi planning intelligence product

STATUS: active        UPDATED: 2026-07-17 by Ask Ebbi Codex

## What

Ask Ebbi is a Hermes-native planning research and drafting assistant for UK planning consultants.
The dashboard is thin glass over a dedicated Hermes orchestrator and specialist roles; the Ask Ebbi
Library owns deterministic retrieval, legal/planning evidence and citations. This is one product:
there is no separate LandPro product or LandPro roadmap.

## Where

- **Canonical local repo:** `/Volumes/deep-1t/Users/k3ss/k3ss-official/ask-ebbi-3/`
- **Canonical GitHub repo:** `k3ss-official/ask-ebbi-3` (private)
- **M4 staging:** `ebbi` on `127.0.0.1:8051`; UI demo `ebbi-rae` on `127.0.0.1:8052`
- **Public beta:** `https://askebbi.com` (health endpoint verified HTTP 200)
- **Future VPS:** `ssh alwyz-01-dock`; Ask Ebbi is not deployed there yet. It will be a separate Compose project from ChadAI.
- **Current evidence:** 46,054 Chroma chunks, 91,121 appeals, 3 style documents; local health and casefile APIs are working.

## Done

- Canonical Ask Ebbi 3 repository created as the future source of truth; historical Ask Ebbi 2 is retained for provenance.
- Hermes-only inference path standardized on the Nous `tencent/hy3:free` lane; OpenRouter and Ollama are not part of the active architecture.
- Primary and alternate UI containers are isolated by port (`8051` main, `8052` demo); public dashboard is behind the Cloudflare tunnel.
- Casefile event-loop defect fixed and casefile/list/appeals health checks verified.
- Current planning-intelligence feed and delegation-visibility stub added for the beta UI.
- Local test suite: 14 passed. Public admin/docs/OpenAPI/CLI surfaces remain blocked from public ingress.
- `collab-mem` adopted as the off-box coordination ledger; it is not mounted as application memory.

## Outstanding

- Reconcile the canonical Git branches and uncommitted UI/research work before VPS deployment.
- Build and verify the dedicated Ask Ebbi Compose project on `alwyz-01-dock` without touching ChadAI's networks, volumes or internal Open Notebook.
- Replace the beta CLI subprocess transport with an authenticated Hermes gateway boundary when the local beta is stable.
- Finish app authentication and operational hardening before expanding public access.
- Verify Telegram round-trip and resolve the separate `rae.askebbi.com` DNS/demo record if that demo remains needed.
- Decide the legacy-citation SOT/vault boundary and implement append-only provenance; ChadAI's Open Notebook is not automatically the Ask Ebbi SOT.
- Register free acquisition channels (APIs, email signups, downloads, RSS/Atom and crawler adapters) with licensing and provenance checks.

## Next

1. Claim and reconcile the Ask Ebbi 3 branch/worktree state; preserve all UI/research provenance. **Owner: Ask Ebbi Codex; Tony approves destructive archival.**
2. Draft the isolated `ask-ebbi-prod` Compose project for `alwyz-01-dock`, including private networks, volumes, secrets, health checks and resource limits. **Owner: Ask Ebbi Codex.**
3. Run a local-to-container Hermes gateway proof before adding further agents/tools. **Owner: Ask Ebbi Codex.**
4. Agree the citation-vault contract and source acquisition registry. **Owner: Tony + Ask Ebbi Codex.**
