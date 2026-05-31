# MASTER — the backlog (durable; daily lists hang off this)

> Each item has an id (M#), a one-line description, an owner, and a status.
> Statuses: `backlog` · `active` · `blocked` · `done` · `later`. Done items drop to LOG.md.

| id | Item | Owner | Status | Notes |
|----|------|-------|--------|-------|
| M1 | **Grok Build onboarding** — Tony trials on M4, then install on the Chad VPS as its OWN non-privileged user (no `~/.hermes` access); Tony runs `grok-build login` | Tony / Rae | active | SuperGrok confirmed. Trialing on M4 now. |
| M2 | **Seat Grok in collab-mem** — add to ROSTER, agree the Rae/Grok ownership split + read/write protocol | Rae | active | This v2 rebuild is step one. |
| M3 | **ChadAI Step 4 — data sources + channel** — legislation.gov.uk API (law), Scrapling MCP (T&C scrape), NotebookLM MCP (grounding notebook), FB pop-out web frame. Enforce API-for-law / scrape-for-T&Cs / answer-only-from-notebook | Rae | backlog | The real test of Nemotron multi-tool reliability. |
| M4 | **ChadAI Step 5 — full hold-nothing purge** — wipe `state.db` rows + `sessions/` + `pairing/` at conversation end (disabling recall ≠ deleting transcript) | Rae | backlog | Depends on how the FB frame signals session-end. |
| M5 | **Wire collab-mem ↔ ChadAI/HANDOFF.md** — collab-mem points at HANDOFF.md as the deep build cursor; one canonical operational entry point, no drift | Rae | backlog | HANDOFF.md on M4 at `/Volumes/deep-1t/Users/k3ss/k3ss-official/ChadAI/`. |
| M6 | **Top-tier model for Chad** — once a working inference key (xAI/Anthropic/OpenAI) is on the box, move Chad off Nemotron | Tony | later | Blocked on a key; see FACTS → Models. |
| M7 | **ChadAI Stage-2 — self-hosted grounded notebook** (InsightsLM/AnythingLLM) to close the NotebookLM third-party GDPR gap | Rae | later | Post-MVP hardening. |

## ChadAI build roadmap (the M3/M4 epic, for context)
1. Hermes install — ✅ done
2. Hold-nothing config — ✅ done + verified
3. Agents as skills (Chad / Retriever / Auditor) — ✅ done + smoke-tested
4. Data sources + channel — ◐ M3
5. Full hold-nothing purge + end-to-end test — ◐ M4
