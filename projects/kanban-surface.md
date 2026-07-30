# Kanban Surface Manager — Hermes-native execution surface over collab-mem
STATUS: active        UPDATED: 2026-07-30 by Codex

## What

The dedicated Hermes Agent installation is the KSM. Hermes native Kanban owns the
live execution projection: boards, cards, atomic claims, resident worker profiles,
dependencies, dispatch and event history. The thin Kanban Surface layer adds
collab-mem sync, scoped identities, receipts and the drag-to-dispatch UI.

collab-mem remains the canonical source for project facts, priorities, history and
ordered next actions. Every executable card carries a `cm_ref` and `source` link
back to this Git ledger; opening a card shows the full linked source file. Routing,
WIP admission, ACKs and state transitions are deterministic and consume no model
tokens. The separate KRP daemon was a useful validated detour, not the final MVP.

## Where

- Active GitHub repo: `k3ss-official/kanban-surface` (private)
- Historical protocol spike: `k3ss-official/kanban-surface-manager` (private)
- Control host: `ssh alwyzon-1` → `alwyz-dock-01`, SSH port 42
- Hermes/KSM home: `/var/lib/hermes`; source: `/var/lib/hermes/kanban-surface`;
  ledger checkout: `/var/lib/hermes/collab-mem`
- Native board: `kanban-surface`; database:
  `/var/lib/hermes/.hermes/kanban/boards/kanban-surface/kanban.db`
- Loopback UI/gateway: `127.0.0.1:8742` (view through an SSH tunnel)
- Boot services: `hermes-ksm.service`, `kanban-surface.service`,
  `kanban-watcher.service`, `kanban-intake.timer`, `kanban-snapshot.timer`
- Resident profiles: `kba-intake`, `kba-build`, `kba-input`, `kba-verify`,
  `kba-done`
- Scoped Tony, Rae and Grok gateway credentials are concealed in 1Password.

## Done

- 2026-07-30 — Restored the original architecture after Tony corrected the KRP
  detour: Hermes native Kanban is the KSM, `kanban-surface` is active, and
  collab-mem remains project truth. PRs #1–#3 adapted Hermes v0.19, added the
  source-linked card panel, defined the KSM identity and added boot-safe units;
  all GitHub CI passed.
- 2026-07-30 — Deployed the exact merged `kanban-surface` release to
  `alwyzon-1`, created the named native board/project, installed five real
  cloned Hermes profiles, configured repo-scoped deploy keys, and passed all
  on-box offline suites against the real collab-mem schema.
- 2026-07-30 — Projected 28 live collab-mem objects into native cards with
  stable idempotency keys. The dispatcher remained off during import; a live
  version drift briefly reported the imported cards ready, so all 28 were
  parked before any worker ran. The second sync proved 0 duplicates and
  admitted exactly one card at WIP 1.
- 2026-07-30 — Opened the authenticated loopback surface in a real Chrome
  session through the SSH tunnel. The selected card displayed its full
  canonical `projects/kanban-surface.md` source, GitHub backlink and native
  receipt history; the UI was not serving a copied project summary.
- 2026-07-30 — Ran the first bounded native intake dispatch. It claimed,
  heartbeated and left receipts correctly, but treated an archived predecessor
  as a live duplicate. Rejected that routing result, fixed intake to ignore
  `done`/`archived` history, fixed the installer to create complete native
  profiles, and merged/deployed `kanban-surface` PR #4
  (`43b8561992ebc4f43e4f2bc7ba35727e0159af7f`).
- 2026-07-30 — Replayed the same canonical work. `kba-intake` proved there was
  no live duplicate and routed the card Intake → Build with
  `mutated_external_state: false`; `kba-build` independently verified the
  claim/spawn/receipt/handoff ledger. The abstract profile wording then caused
  Build to complete rather than hand off. Proved Hermes'
  `reassign --reclaim` and active-run archive primitives without a model,
  encoded exact terminal commands in all five profiles, and merged/deployed
  PR #5 (`0f25a53b322c6bdae6f897f86af011234d49900a`).
- 2026-07-30 — Activated the native gateway, UI, watcher and both timers. The
  first activation also exposed Hermes' upstream defaults: auto-decompose
  consumed one unselected Socialite triage card while the selected Build card
  ran; that run made no external mutation. Locked the KSM to
  `max_in_progress=1`, `max_spawn=1`, one worker per profile,
  `auto_decompose=false`, and a 60-second dispatcher tick. Final live state:
  all five units active, no ready/running cards, no failed units, UI loopback
  only, public listener still SSH/42 only.
- 2026-07-30 — Completed the strict host residue pass and two-boot acceptance.
  Removed the empty Docker/containerd stack, Snap/LXD activation, cron,
  cloud/provisioning agents, PackageKit/Polkit, irrelevant device/storage
  daemons, VMware tools on KVM, pilot/rejected-release material and stale
  identities/caches. The final host exposed only SSH/42, used 367 MiB RAM,
  reported no failed units or orphan packages, and returned the exact KSM
  health result after the second boot.
- 2026-07-30 — Re-ran both real agent boundaries after cleanup. Hermes returned
  `HERMES_ACP_SMOKE_OK`; Codex returned `CODEX_ACP_SMOKE_OK`; both ended
  `end_turn` without workspace mutation. A local Codex smoke-harness timeout
  was traced to buffered stdout plus `select()`, not to ACP or Codex, and the
  corrected threaded reader completed normally.
- 2026-07-30 — Completed the first full production-shaped vertical slice.
  `KSM-PILOT-1` flowed through Hermes planning, KSM plan validation, Codex patch
  proposal, two explicit rejection gates, human correction, 15 passing tests,
  GitHub PR #2, merge, immutable deployment, and deterministic verification.
  Release `9b98356249e4fbdd0c447e7e0930abb0e6d70df5` returned
  `{"ok":true,"quick_check":"ok","schema":"ready"}` without changing the
  database hash or mtime. KSM closed the card `DONE` at revision 15.
- 2026-07-30 — Shelved Ask Ebbi/ChadAI from `alwyzon-1` under Tony's explicit
  approval. Verified the local archive before purging the VPS copy:
  `/Volumes/hotblack-2tb/.config-bak/server-shelves/alwyzon-1/2026-07-30/ask-ebbi-shelf-20260730.tar.zst`,
  SHA-256 `9befda91a191a65418b5bfa2d9ffd179f114247808112bd6b3ccdff63b8f4fab`.
- 2026-07-30 — Re-established the host as a dark KSM MVP box: no application
  listener, no installed container runtime, UFW default-deny, Fail2ban/auditd
  active, and separate locked `ksm`, `hermes`, and `ksm-worker` identities.
- 2026-07-30 — Installed Hermes v0.19.0, Node v22.23.2, Codex CLI v0.146.0 and
  `codex-acp` v1.1.7. Hermes and Codex have separate ChatGPT OAuth stores.
- 2026-07-30 — Proved both real ACP runtimes. Codex returned
  `KSM_WORKER_READY` with 16,555 tokens; Hermes returned it with 11,653 tokens.
  Both sessions ended normally and left their acceptance workspaces unchanged.
  The bootstrap cost confirms ACP sessions are for coherent work, never ACKs or
  heartbeats.
- 2026-07-30 — Deployed immutable KSM release
  `3ec9a563af52cab13242d36c1f287af6e8bbaf96`; merged foundation PR #1 to main
  as `becf3fcbe7c0c2bc4183ad5c98d1326c288a54f4`.
- 2026-07-30 — Live service lifecycle passed. The first acceptance run exposed
  worker-readable SQLite state; that release was rejected, the state/socket
  groups were split, and the repeated run proved socket access allowed,
  database read denied, one completion, and canonical `REVIEW`.
- 2026-07-30 — `KSM-PILOT-1` proved recovery gates: an over-budget Hermes plan
  and a malformed Codex diff were cancelled without leaving an active lease.
  Hermes' validated 1,288-byte plan is stored as a KSM artifact. Codex proposed
  a deterministic health command; human review caught one missing test import
  before any workspace mutation.

## Outstanding

- Reboot twice, verify timers/dispatcher/UI, then retire the superseded
  `ksm.service`, KRP state and unused `ksm`/`ksm-worker` host identities.
- With Tony watching, use one deliberate UI drag to prove the scoped audit
  identity and move permission path; keep every other card parked.
- Define the smallest external-AI handshake after the surface is visible; do
  not deploy the optional MCP shim merely because it exists.

## Next

1. ~~Codex: select this project's current card, start Hermes, and verify one
   native intake dispatch end to end.~~ Completed 2026-07-30.
2. Codex: open and verify the loopback UI, then complete two-boot acceptance and
   remove the superseded KRP host components.
3. Tony + Codex: watch one drag-to-dispatch run, then lock the minimal external
   AI announcement/intent contract.
