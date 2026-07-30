# LOG — what's been done (newest on top; operational, not literary)

## 2026-07-30 — Codex
- **Hermes-native surface exercised live:** opened the authenticated loopback
  board through SSH in Chrome, verified full canonical collab-mem source and
  receipts, and ran one selected card Intake → Build. The run exposed and
  rejected two real integration faults: archived history counted as a
  duplicate and prose-only station handoffs. `kanban-surface` PRs #4–#5 fixed
  both plus the profile installer; all local, CI and on-box suites passed.
  PRs #6–#7 then made project `next-N` identities permanent and ledger sync
  projection-only. Hermes is capped at one global worker, auto-decompose is
  disabled, and manual admission is required; all five native services/timers
  are active with no ready/running cards.
- **Hermes-native Kanban Surface deployed:** restored Tony's original
  architecture after the KRP detour; merged `kanban-surface` PRs #1–#3,
  installed the dedicated KSM identity and five resident profiles, projected
  28 source-linked collab-mem cards, and started the loopback UI/watcher.
  Import ran with the dispatcher gated; zero model runs occurred, the repeat
  sync created zero duplicates, and automatic WIP was capped at one.
- **Strict vanilla-host acceptance:** purged Docker/containerd, Snap/LXD, cron,
  cloud/provisioning agents, PackageKit/Polkit, irrelevant storage/device
  services, VMware tooling on KVM, stale identities, caches and completed pilot
  material. Two cold boots returned SSH/42 and exact KSM health; no failed units
  or orphan packages remained. Real Hermes and Codex ACP smoke turns both ended
  normally without workspace mutation.
- **KSM host established:** verified and archived the old `alwyzon-1` tenant
  state, removed approved Ask Ebbi/ChadAI/Cloudflare/Docker residue, and retained
  the hardened SSH/UFW/Fail2ban/audit baseline.
- **KISS protocol proved:** built KRP/1 with one canonical writer, run-scoped
  capabilities, optimistic revisions, idempotency and local Unix-socket IPC.
  Deterministic coordination stays outside ACP/MCP/model context.
- **Production-shaped daemon accepted:** installed separate `ksm`, `hermes` and
  `ksm-worker` identities; deployed `ksm.service`; caught and fixed
  worker-readable state; repeated lifecycle ended in `REVIEW` with direct
  database access denied.
- **Real agents accepted:** authenticated Hermes and Codex separately and ran
  real ACP turns with unchanged workspaces. The measured 11.6k/16.5k bootstrap
  inputs locked the rule that one ACP session covers one coherent plan/run.
- **GitHub boundary established:** private
  `k3ss-official/kanban-surface-manager`, repo-scoped deploy key, foundation PR
  #1 merged.
- **First complete relay accepted:** Hermes planned `KSM-PILOT-1`; Codex
  proposed the patch; KSM/human gates rejected malformed metadata and a missing
  test import before mutation; 15 tests passed; PR #2 merged; release `9b98356`
  deployed; live health left the database unchanged; KSM closed revision 15 as
  `DONE`.

## 2026-07-17 — Ask Ebbi Codex
- **Two-tenant boundary recorded:** `alwyz-01-dock` is the shared host for two isolated Docker projects. ChadAI remains owned by the separate ChadAI Codex lane; Ask Ebbi owns its own future Compose project. No networks, volumes, Hermes homes or secrets are shared.
- **collab-mem v2 aligned:** added the current Ask Ebbi 3 card, marked Ask Ebbi 2 historical, refreshed the ChadAI card, updated current facts/ownership, and converted today's baton to the documented Markdown daily format.
- **Open Notebook boundary recorded:** the existing internal ChadAI Open Notebook is not automatically an Ask Ebbi citation vault or shared service.

## 2026-07-06 — Fable (Claude Code — Ask Ebbi build)
- **Ask Ebbi 2 live end-to-end on alwyzon**: dashboard → Ebbi (Hermes, Nous glm-5.2) → 4 sub-agent
  profiles → Library MCP (8 tools; 41,661 corpus chunks + 91,121 PINS appeals). E2E answer verified
  (Lancaster householder appeals 72/19.4% + NPPF GB para, cited). App = systemd `ask-ebbi.service`,
  loopback :8001. Card: `projects/ask-ebbi-2.md`.
- **alwyzon swept for staging** (Tony's order): Chad's Hermes → `~/quarantine/chad-hermes-20260706`
  (9.3GB, restorable); VPS rebooted; fresh Hermes installed as **Ebbi**. Chad re-placement = M19.
- **Isolation doctrine locked** after M4 near-miss (Ebbi had been created as a profile in Rae's
  ~/.hermes — purged same day, zero residue verified): one agent = one Hermes install = one home.
- **collab-mem updated**: added `projects/` card layer (what/where/done/outstanding/next per
  project), fixed FACTS drift (alwyzon ≠ Chad anymore), M18/M19 added, Chad items marked on-hold.
- Hermes runtime incident (M4, shared Rae/ebbi runtime): installer update collided with Tony's
  `[hermes-restriction-clean]` patch → merge conflicts broke startup; restored `tools/approval.py`
  to stock upstream; Tony's patch preserved in `git stash` for his own decision.

## 2026-05-31 (cont. 2) — Rae (Claude Code) — session flow LOCKED + Step 4 plan in
- **Canonical session flow agreed** (Tony + Rae) and committed to `ChadAI/docs/session-flow.md`.
  Key calls: **go/no-go before the clock**; answer-only-from-notebook (Retriever fetches into the nb
  before Chad cites); verification gate before chat or report; **mandatory report** emailed at close
  (only surviving copy + paid CTA + branding); **email address is session-ephemeral** (used once,
  wiped, never retained); close = user-X OR 1-min idle→warn→close, with a **guaranteed server-side
  wipe** however they leave; 10-min cap is [MVP].
- **Dispatch test (Claude Code Agent tool) succeeded** — background subagent produced `ChadAI/docs/
  step4-plan.md` (all four wires researched, zero mutations). 5 open decisions await Tony (below).
- Confirmed `/goal` IS a real Claude Code built-in (registers a completion condition) — Rae had
  wrongly dismissed it from memory; verify caught Rae, not Chad. Lesson logged: "badly cited" ≠ "false".

## 2026-05-31 (cont.) — Rae (Claude Code) — Chad VALIDATED on a real case
- Tested Chad cold on a real case: X permanently suspended a 17-year real-name account as an
  automated false-positive after API testing. **Chad independently prescribed the winning strategy**
  (SAR under UK GDPR → compel disclosure → ICO backstop → correct Dublin controller → bypass the
  broken appeal form). Real outcome: account restored in 17 days; X admitted an automated mistake.
- Refinements → MASTER M8 (Art 22 spearhead, export-rebuttal, escalation ladder, dispute-letter =
  paid tier). Fixture: `ChadAI/docs/cases/x-suspension-sar.md`.

## 2026-05-31 — Rae (Claude Code)
- **collab-mem v2 rebuilt** (off-box, KISS, README + MASTER + daily + FACTS + ROSTER + LOG; shared
  ledger stays git, NOT mem0).
- **ChadAI Step 2 (hold-nothing)** verified: memory + session_search toolsets off (CLI + Telegram);
  profiling off; provider none; auto_prune on. Dead grok-4 fallback neutralized.
- **ChadAI Step 3 (agents as skills)**: chad-retriever + chad-auditor deployed to ~/chad-skills
  (external_dirs); delegate_task → skill_view → summary smoke-tested OK on Nemotron. AGENTS.md §3
  reconciled. delegation.model pinned to Nemotron.
- **Telegram fixed** (Tony added to allowlist). **SSH-from-M4 confirmed.**
- **Architecture:** sealed Chad (isolated + hold-nothing, clean GDPR) + collab-mem-coordinated
  backend (Tony + Rae + Grok) + Grok Build as a walled VPS user.
