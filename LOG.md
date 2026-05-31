# LOG — what's been done (newest on top; operational, not literary)

## 2026-05-31 — Rae (Claude Code)
- **collab-mem v2 rebuilt.** Premise refactored: dated JSON dailies → canonical structure
  (README protocol + EOD/BOD ritual · MASTER backlog · markdown daily with current-task+status ·
  FACTS · ROSTER · this LOG). Off-box, surface-agnostic, KISS. Decided shared ledger stays git, NOT mem0.
- **ChadAI Step 2 (hold-nothing)** applied + verified: `memory` + `session_search` toolsets off on CLI
  AND Telegram (`hermes tools --summary`); `memory_enabled`/`user_profile_enabled` off; external provider
  none; `sessions.auto_prune` on. Fixed then neutralized a broken `fallback_model` (grok-4 fallback was
  dead — no xAI key).
- **ChadAI Step 3 (agents as skills)** built + verified: `chad-retriever` + `chad-auditor` SKILL.md
  deployed to `~/chad-skills` (registered via `skills.external_dirs`, kept out of `~/.hermes`).
  `delegate_task → skill_view → summary` plumbing smoke-tested OK on Nemotron. AGENTS.md §3 dispatch
  contract reconciled to `delegate_task` reality. `delegation.model` pinned to Nemotron.
- **Telegram fixed:** Tony's id was the home channel but missing from `TELEGRAM_ALLOWED_USERS` → every
  message denied. Added it; gateway restarted; working.
- **Confirmed** Rae can SSH the VPS directly (old "sandbox can't ssh" note was stale from the cloud setup).
- **Architecture decision:** sealed Chad (isolated + hold-nothing, clean GDPR) + collab-mem-coordinated
  backend (Tony + Rae + Grok) + Grok Build as a walled VPS user.
