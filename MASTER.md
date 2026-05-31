# MASTER — the backlog (durable; daily lists hang off this)

> Each item has an id (M#), a one-line description, an owner, and a status.
> Statuses: `backlog` · `active` · `blocked` · `done` · `later`. Done items drop to LOG.md.

| id | Item | Owner | Status | Notes |
|----|------|-------|--------|-------|
| M1 | **Grok Build onboarding** — Tony trials on M4, then install on the Chad VPS as its OWN non-privileged user (no `~/.hermes` access); Tony runs `grok-build login` | Tony / Rae | active | SuperGrok confirmed. Trialing on M4 now. |
| M2 | **Seat Grok in collab-mem** — add to ROSTER, agree the Rae/Grok ownership split + read/write protocol | Rae | active | v2 rebuild done; Grok seat + protocol still to finalise. |
| M3 | **ChadAI Step 4 — data sources + channel** — legislation.gov.uk API + Scrapling MCP + NotebookLM MCP + FB frame; answer-only-from-notebook | Rae | **blocked on Tony** | Plan ready → `docs/step4-plan.md`. Flow locked → `docs/session-flow.md`. **5 decisions await Tony** (see today's daily). Also fixes the X-SAR ungrounded-citation gap. |
| M4 | **ChadAI Step 5 — full hold-nothing purge** — wipe `state.db` rows + `sessions/` + `pairing/` + uploads + email address at session end; server-side, guaranteed | Rae | backlog | Spec'd in session-flow.md (the wipe never depends on the client). |
| M5 | **Wire collab-mem ↔ ChadAI/HANDOFF.md** — one canonical entry point, no drift | Rae | backlog | HANDOFF.md on M4 at `/Volumes/deep-1t/Users/k3ss/k3ss-official/ChadAI/`. |
| M6 | **Top-tier model for Chad** — once a working inference key is on the box, move Chad off Nemotron | Tony | later | Blocked on a key; see FACTS → Models. |
| M7 | **ChadAI Stage-2 — self-hosted grounded notebook** to close the NotebookLM third-party GDPR gap | Rae | later | Post-MVP hardening. |
| M8 | **Chad instruction polish + paid-tier lines (from the X-SAR case)** — Art 22/15(1)(h) spearhead, export-rebuttal line, escalation ladder (s.165 DPA), dispute-letter drafting (paid). Pin the answer-walkthrough | Tony / Rae | backlog | Source: `ChadAI/docs/cases/x-suspension-sar.md`. Free = strategy; Paid = the counter-play. |

## ChadAI build roadmap
1. Hermes install — ✅ done
2. Hold-nothing config — ✅ done + verified
3. Agents as skills (Chad / Retriever / Auditor) — ✅ done + smoke-tested + validated on a real case
4. Data sources + channel — ◐ M3 (plan + flow ready; awaiting Tony's 5 decisions)
5. Full hold-nothing purge + end-to-end test — ◐ M4
```

Canonical specs now in `ChadAI/docs/`: `session-flow.md` (the session), `step4-plan.md` (the wiring),
`cases/x-suspension-sar.md` (the validation fixture).
