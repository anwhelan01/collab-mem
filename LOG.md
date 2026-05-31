# LOG — what's been done (newest on top; operational, not literary)

## 2026-05-31 (cont.) — Rae (Claude Code) — Chad VALIDATED on a real case
- Tested Chad (first-out, ungrounded, persona-untuned) cold on a real case: X permanently suspended
  a 17-year real-name account (`@tonywhelan`) as an automated false-positive after API testing.
- **Chad independently prescribed the winning strategy:** SAR under UK GDPR → compel disclosure of
  the suspension logic → ICO backstop → correct Dublin controller → bypass the broken appeal form.
- Real outcome (Tony, 11–28 May 2026): SAR (Art 15 + **22** + DPA 2018) → rebutted X's standard-export
  fob-off citing 15(1)(h)+22 + "an export doesn't reset the clock" + s.165 DPA threat → **account
  restored in 17 days** after ~13 months of failed appeals. X admitted an automated mistake in writing.
- Refinements identified (→ MASTER M8, paid-tier lines): Art 22 spearhead, export-rebuttal line,
  escalation ladder, dispute-letter drafting. Fixture stashed at `ChadAI/docs/cases/x-suspension-sar.md`.
- Verdict: the MVP premise is validated. The one weakness (ungrounded citations) is exactly what Step 4 fixes.

## 2026-05-31 — Rae (Claude Code)
- **collab-mem v2 rebuilt.** Premise refactored: dated JSON dailies → canonical structure
  (README protocol + EOD/BOD ritual · MASTER backlog · markdown daily with current-task+status ·
  FACTS · ROSTER · this LOG). Off-box, surface-agnostic, KISS. Decided shared ledger stays git, NOT mem0.
- **ChadAI Step 2 (hold-nothing)** applied + verified: `memory` + `session_search` toolsets off on CLI
  AND Telegram; `memory_enabled`/`user_profile_enabled` off; external provider none; `sessions.auto_prune`
  on. Fixed then neutralized a broken `fallback_model` (grok-4 fallback was dead — no xAI key).
- **ChadAI Step 3 (agents as skills)** built + verified: `chad-retriever` + `chad-auditor` SKILL.md
  deployed to `~/chad-skills` (registered via `skills.external_dirs`, kept out of `~/.hermes`).
  `delegate_task → skill_view → summary` plumbing smoke-tested OK on Nemotron. AGENTS.md §3 dispatch
  contract reconciled to `delegate_task` reality. `delegation.model` pinned to Nemotron.
- **Telegram fixed:** Tony's id was the home channel but missing from `TELEGRAM_ALLOWED_USERS` → every
  message denied. Added it; gateway restarted; working.
- **Confirmed** Rae can SSH the VPS directly (old "sandbox can't ssh" note was stale from the cloud setup).
- **Architecture decision:** sealed Chad (isolated + hold-nothing, clean GDPR) + collab-mem-coordinated
  backend (Tony + Rae + Grok) + Grok Build as a walled VPS user.
