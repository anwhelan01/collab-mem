# ask-ebbi-2 — the Planning Oracle (LexisNexis for UK planning, Hermes-native)

STATUS: active        UPDATED: 2026-07-06 by Fable (Claude Code — Ask Ebbi build)

## What

AI planning assistant for UK planning consultants. User zero: Dave ("Ebbi"), Tony's
uncle, planning consultant in Morecambe — free forever as resident tester/advisor.
Architecture per Tony's diagram: **Ebbi, a Hermes agent, IS the backend** — the web
dashboard is thin glass. Ebbi orchestrates four sub-agent profiles inside his own
install (search / prior-art / writer-dave / intel); all planning-law FACTS come from
"the Library" (FastAPI + ChromaDB corpus + SQLite appeals DB) exposed as MCP tools.
Hard rule: **the agent learns workflows, never facts** — every legal claim must come
through a Library tool, cited. Inference: Tony's Nous subscription (glm-5.2). Not
legal advice — research/drafting assistant for professionals; disclaimers throughout.
Business ladder: Dave free → NW paid pilots (£299/6wk) → £99/user/mo → paid
Appeal Prospects Reports (£49–149) → API. Full vision: `Ask-Ebbi/VISION.md`.

## Where

- **Repos (M4):** `/Volumes/deep-1t/Users/k3ss/k3ss-official/ask-ebbi/`
  - `ask-ebbi-2/` — THE product (git init'd, **no commits yet**). README + `hermes/RUNBOOK.md` inside.
  - `Ask-Ebbi/` — v1, source-of-truth docs (`VISION.md`, `REPO_AUDIT.md`, `PAID_PILOT_PLAN.md`, `DEMO_SCRIPT.md`); v1 code changes uncommitted.
  - GitHub `k3ss-official/askebbi-planner-tool` — scaffold, recommended ARCHIVE (nothing worth porting).
- **VPS (staging):** `ssh alwyzon` (port 42, sudo NOPASSWD; HestiaCP box — also runs nginx/mail/DNS for socialite).
  - App: `~/apps/ask-ebbi-2`, systemd **`ask-ebbi.service`**, bound **127.0.0.1:8001** (MemoryMax=1200M).
    Access: `ssh -L 8001:localhost:8001 alwyzon` → dashboard `http://localhost:8001`, admin `/admin.html`.
  - **Ebbi's Hermes:** `~/.hermes` (v0.18.0, SOUL = Ebbi, Nous glm-5.2 authed). Sub-profiles:
    `search`, `prior-art`, `writer-dave`, `intel` — each with SOUL + Library MCP (`~/.local/bin/<name>`).
  - MCP server: `~/apps/ask-ebbi-2/mcp_server/server.py` (stdio; 8 tools: search_knowledge,
    search_appeals, appeal_stats, get_site_constraints, corpus_status, get_style_profile,
    list_casefiles, casefile_context).
  - **Chad quarantine:** `~/quarantine/chad-hermes-20260706` (9.3GB) + gateway unit + wrapper — restorable by `mv`.
- **Data:** 41,661 corpus chunks (NPPF Dec-24, PPG, GPDO, 8 statutes, Approved Docs, Lancaster
  Local Plan Jan-25, Lancashire M&W, Historic England, planning.data.gov.uk) + **91,121 PINS appeal
  cases** (`data/appeals.db`, quarterly refresh via `scripts/ingest_appeals.py`).

## Done (newest first)

- **2026-07-06 — Ebbi runs the show.** Dashboard `/api/chat` routes ALL chat to Ebbi (hermes -z
  envelope + inline history); sub-agent profiles created/wired; MCP grew to 8 tools; E2E verified:
  one dashboard question returned Lancaster householder appeal stats (72 cases, 19.4%) + NPPF
  Green Belt para with quote+URL. Admin chat = Ebbi. No OpenAI needed — all inference on Nous.
- **2026-07-06 — VPS deploy.** Swept Chad's stale Hermes to quarantine (Tony's order — staging box),
  rebooted, fresh Hermes installed as Ebbi, Nous OAuth connected by Tony, acid test passed
  ("Lancaster: 232 appeals, 17.2%"). App deployed via requirements.lock.txt (pip conflict fixed;
  unused `ollama` pkg dropped).
- **2026-07-06 — M4 isolation purge.** Ebbi-as-profile inside Rae's ~/.hermes was wrong → deleted,
  zero residue verified. Doctrine locked: one agent = one Hermes install = one home.
- **2026-07-06 — ask-ebbi-2 built** on v1's proven Library: PINS appeals module (91k cases, search/
  stats + decision-letter links), NotebookLM-style **casefiles** (auto-gathered bounded grounding:
  policy + comparable appeals + live constraints; chat retrieves only within the casefile), new
  dashboard (Oracle/Casefiles/Appeals/Draft) + admin backend.
- **2026-07-05 — v1 made demo-ready.** Audit chose Ask-Ebbi over planner-tool; fixed 4 dead
  ingestors (URL rot) + purged 5,027 mislabeled national spatial chunks; constraint lookup now
  site-specific (postcodes.io geocode; LA1 1YS → Lancaster Castle Gr-I verified); pilot plan +
  demo script written.

## Outstanding

- **No auth on the app** — loopback-only until fixed. Blocks putting it on askebbi.com and the Dave demo-by-URL.
- **No streaming / slow bridge** — `hermes -z` per message (15–60s); upgrade to `hermes serve` WS backend.
- **SOUL leak observed:** Ebbi added an uncited "national average 30–35%" from model weights — tighten SOULs (uncited comparators must be labelled or omitted) + build a citation eval.
- **Appeal decision LETTERS not ingested** (metadata + ACP links only) — the fetcher is the biggest product-value item open.
- **Intelligence Desk not live:** needs `hermes gateway install` + WhatsApp/Telegram pairing (Tony, interactive) then cron the morning-briefing + corpus-health skills.
- Retrieval ranking: HE point-records drown GPDO for PD questions (doc-type boosting needed).
- Repo hygiene: ask-ebbi-2 has no first commit; v1 changes uncommitted; planner-tool not yet archived on GitHub.
- Markdown renders as plain text in chat bubbles (cosmetic).

## Next (ordered)

1. **Ingest appeal decision letters** (Lancaster + NW first) into the corpus as `appeal_decision`
   chunks — unlocks real precedent reasoning in casefiles and the paid Appeal Prospects Report.
   Owner: any AI, no Tony input needed.
2. **Gateway + WhatsApp pairing** on alwyzon → schedule morning-briefing + corpus-health crons.
   Owner: Tony (interactive pairing), 10 min, then AI finishes.
3. **Tighten sub-agent/Ebbi SOULs** re uncited claims + spot-check 20 answers for citation integrity.
4. **First commits**: ask-ebbi-2 initial commit, v1 fixes commit, archive planner-tool (Tony go-ahead).
5. **Auth + askebbi.com exposure** (Hestia nginx + basic auth minimum) → then book the Dave demo
   (`Ask-Ebbi/DEMO_SCRIPT.md` is written and pre-verified).
6. Stage-2 bridge: `hermes serve` WebSocket for streaming + native sessions.
