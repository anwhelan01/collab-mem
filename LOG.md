# LOG — what's been done (newest on top; operational, not literary)

## 2026-08-05 — Final Ask Ebbi orphan-volume cleanup
- Re-verified `ebbi_ui_home` (1.8 GB) and `ebbi_ui_data` (376 KB) on
  `alwyzon-1`: both were unattached, unlabeled legacy volumes. The former held
  an obsolete Hermes home with seven `.env` files; no secret values were read
  or exposed.
- Permanently removed exactly those two volumes. Preserved the live
  `ask-ebbi_ebbi_data` and `ask-ebbi_ebbi_hermes` volumes.
- Post-removal verification: container `ebbi` remained running/healthy and both
  `/api/health` and `/api/ready` returned successfully.
- Removed seven exact temporary Ask Ebbi audit/build trees, including the 3.6 GB
  extracted `/tmp/imgroot`; `/tmp` fell to 18 MB and root-disk use fell from
  82 GB to 78 GB, leaving 154 GB free.
- Routed the stale readiness corpus-count correction to engineering and gated
  the next controlled rebuild on all reviewed UAT fixes. That rebuild must
  restore bundled `hermes/RUNBOOK.md` parity and stamp the OCI source revision.
- After Codex attributed all reclaimable BuildKit state to Ask Ebbi, removed
  54.121 GB of logical unused cache in two passes, deleted the unused 4.02 GB
  standalone Hermes image, and removed ten exact `/tmp` source/script/log
  residues. Physical root-disk use fell from 78 GB to 26 GB, leaving 207 GB
  free; the remaining 8.762 GB BuildKit state is shared/active with zero
  reclaimable bytes.
- Final verification preserved the one healthy `ebbi` container, loopback-only
  port, both named volumes, and `/var/lib/hermes` untouched. Health remained
  59,902 chunks and readiness the known 59,294 startup snapshot pending the
  reviewed code fix.

## 2026-08-05 — Three genuine Ask Ebbi feedback rows recovered
- Investigated Tony's report that a user's 2026-08-04 feedback was absent from
  the iCloud inbox. Production and iCloud contained only Tony's later test row;
  the five-minute sync job itself was healthy.
- Found three genuine submissions from display name `d` in the preserved
  `pre-feedback-clear-20260804T182846Z.db` snapshot. They had been included in a
  seven-row inbox clear at 18:28 UTC, before the later mirror state captured
  them; this was a data-clear incident, not a sync/provider failure.
- Restored only the three genuine rows, excluding four obvious test rows.
  Verified four live rows total, four generated iCloud item files, successful
  scheduler execution, and File Provider state `isUploaded=1`, not uploading,
  not paused and not excluded. UAT health/readiness remained green.

## 2026-08-05 — Ask Ebbi reduced to GitHub UAT plus live VPS lane
- On Tony's explicit command, permanently deleted every identified local Ask
  Ebbi source checkout and source/data backup, including the local
  `ask-ebbi-uat` checkout, legacy clones, Hotblack archives and VPS backup tree.
- Removed the stopped `ebbi-ui-uncle` container and all ten rollback/demo image
  tags. Preserved the running healthy `ebbi` container, current `latest` image,
  live source `/home/anwhelan/ask-ebbi`, deployment secrets and both named
  volumes.
- Verified the sole source lanes are private GitHub
  `k3ss-official/ask-ebbi-uat/main` and the live VPS tree. Health and readiness
  remained green after cleanup.

## 2026-08-05 — Superseded Ask Ebbi remotes deleted
- On Tony's explicit command, deleted private GitHub repositories
  `k3ss-official/askebbi-planner-tool`, `k3ss-official/Ask-Ebbi`,
  `k3ss-official/ask-ebbi-2` and `k3ss-official/ask-ebbi-3`.
- Verified each exact remote through the GitHub API after deletion: all four
  return HTTP 404 / Not Found. Local checkouts and backups were not deleted.
- Canonical private UAT remains `k3ss-official/ask-ebbi-uat`.

## 2026-08-05 — Ask Ebbi UAT repository established
- Created private `k3ss-official/ask-ebbi-uat`, reconciled from the live
  `alwyzon-1` source plus the known-good build/test baseline, and pushed
  `main` at `1c3d257`.
- Preserved the live UK relay code absent from every prior GitHub branch,
  restored coherent tests/build policy, and synced the reconciled tracked tree
  back to the host without rebuilding or restarting the service.
- Verified 182/182 tracked files match GitHub/local/host, 64/64
  application-bearing files match host/container, 233 tests pass, the private
  repository contains no runtime data or detected secrets, and `ebbi` remains
  healthy/ready.
- Archived the pre-sync source at
  `/home/anwhelan/ask-ebbi-backups/source-before-uat-repo-20260805T125430Z.tar.gz`.
  The unlabelled recovery image remains live; the next deliberate build must
  restore OCI `APP_SOURCE_COMMIT` provenance.

## 2026-08-05 — Ask Ebbi UAT truth reconciliation
- Corrected the canonical Ask Ebbi state after live inspection proved the
  paused/KSM-only record stale. `alwyzon-1` now runs healthy Docker container
  `ebbi` at deployed application revision `bd16dc04` from
  `feat/uncle-demo-feedback`, alongside isolated host-native KSM services.
- Verified the image label, deployment marker, source checksum, loopback health
  and readiness, public Cloudflare Access gate, 59,294 corpus chunks, current
  appeals counts, 29 OpenAPI paths, Hermes v0.19.1 and container resource/log
  controls. The exact deployed application revision passed all 233 project
  tests (18 deprecation warnings, no failures). No runtime, secret, volume,
  gateway or ingress mutation was made.
- Updated M18, host facts, ownership, today's baton and the project card. The
  UAT branch remains intentionally ahead of `main` pending Tony's acceptance;
  no stale branch or PR was merged.
- Re-verified the shared-host identity boundary: the current KSM services all
  run as host `hermes`, the retired KRP socket is absent, and only `anwhelan`
  belongs to the Docker group.
- Reconciled and pushed the Ask Ebbi current deployment docs, then synced those
  three doc files to the bare host working tree after creating reversible backup
  `/home/anwhelan/ask-ebbi-backups/docs-reconcile-20260805T082603Z`. No container
  rebuild or service restart occurred; the running image remains `bd16dc04`.

## 2026-08-05 — Overnight documentation sweep
- **AUTO_SAFE / fixed-and-verified:** audited GitHub `main` at `5ffdfdd`, created
  the current-day handoff, and updated the KSM project card from the prior
  `088c4d4` projection to the live 2026-08-05 02:00 UTC snapshot. The generated
  board contains 34 cards: 29 blocked, 2 triage and 3 done; it was not
  hand-edited.
- **INFORMATIONAL / verified:** authenticated GitHub checks confirmed the
  recorded heads for `kanban-surface` `main` (`346b1e8`), Ask Ebbi 3
  `ui-console-alt` (`323db65`), Stunning `main` (`7d07cbd`), Scam Alert UK
  `feature/sqlite-osint-fixes` (`8ae693b`) and Socialite `main` (`b71646a`)
  still match their project cards.
- **REVIEW_REQUIRED / Tony decision:** old collab-mem pull requests #1, #2 and
  #3 remain open. No merge or close action was taken.
- No runtime, gateway, launchd, main Hermes config, secret or irreversible
  operation was attempted.

## 2026-08-04 — Overnight documentation sweep
- **AUTO_SAFE / fixed-and-verified:** audited the fetched GitHub `main` at
  `088c4d4`, created the missing current-day handoff, and updated the KSM
  project card from the prior `f3e416d` board snapshot to the live
  2026-08-04 02:00 UTC projection. The board contains 28 blocked, 2 triage and
  3 done, with no ready/running cards; the machine-rendered board file was not
  hand-edited.
- **INFORMATIONAL / verified:** the active `kanban-surface` repo `main` remains
  at `346b1e851169` (`fix: make dispatch explicitly manual (#7)`). The linked
  Ask Ebbi card's `ui-console-alt` branch remains at `323db65`; the project is
  paused and no runtime or deployment action was taken.
- **REVIEW_REQUIRED / Tony decision:** old collab-mem pull requests #1, #2 and
  #3 remain open. No merge or close action was taken.
- No runtime, gateway, launchd, main Hermes config, secret or irreversible
  operation was attempted.

## 2026-08-03 — Overnight documentation sweep
- **AUTO_SAFE / fixed-and-verified:** fast-forwarded the clean canonical
  checkout to GitHub `main` at `f3e416d`, verified the machine-rendered board
  projection, and recorded its current 27-blocked / 2-triage / 3-done state in
  the KSM project card without editing `docs/board-state.md`.
- **BLOCKED / investigation:** two read-only SSH attempts to `alwyzon-1`
  exceeded their command timeouts. No host health, service, tenant or runtime
  claim was changed or inferred from the timeout.
- **REVIEW_REQUIRED / Tony decision:** three old collab-mem pull requests
  remain open (#1, #2 and #3). No merge or close action was taken.

## 2026-08-02 — Overnight documentation sweep
- Audited `main` at `184e768`, current project branch heads, internal Markdown
  links and the machine-rendered board projection. Added the current daily
  handoff and corrected the stale Ask Ebbi instruction that still named the
  dedicated KSM host as a future Compose target.
- Left `docs/board-state.md` untouched because it is machine-rendered. No
  runtime, gateway, launchd, main Hermes config, secret or irreversible changes
  were made.


## 2026-08-01 — Overnight documentation sweep
- Audited `main` at `02b4097` and corrected four low-risk documentation defects:
  restored the current daily handoff, removed an unmatched `MASTER.md` code fence,
  and reconciled the Ask Ebbi 3 and Stunning project cards against their current
  GitHub branch heads.
- No runtime, gateway, launchd, main Hermes config, secret or irreversible changes
  were made. Machine-rendered `docs/board-state.md` was left untouched.

## 2026-07-30 — Tony / Rae
- **Grok Build onboarding completed:** Tony trialled Grok Build on the M4, then installed it on the Chad VPS under its own non-privileged user with no access to `~/.hermes`; Tony completed `grok-build login` using the confirmed SuperGrok subscription.

## 2026-07-30 — Codex
- **Hermes-native KSM accepted and KRP retired:** passed two additional cold
  boots with distinct boot IDs; all five services/timers returned active, exact
  health passed, the board stayed 27 blocked + 3 done with zero ready/running,
  and SSH/42 remained the only public application listener. Removed the
  disabled `ksm.service`, all KRP config/app/state/log and retired worker
  runtime paths, both legacy users and all legacy groups. Final audit found no
  failed units or KRP residue; the local SSH tunnel again serves the UI on
  `127.0.0.1:8742`.
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
