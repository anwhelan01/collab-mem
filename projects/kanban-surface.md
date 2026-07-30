# Kanban Surface Manager — KISS execution layer over collab-mem
STATUS: active        UPDATED: 2026-07-30 by Codex

## What

KSM makes collab-mem's project catalog executable without replacing the
surface-agnostic Git ledger. KSM is the single control-plane writer: it grants one
run-scoped capability, owns task revisions and transitions, and records useful
events. Workers never negotiate ownership conversationally.

KRP/1 is the tiny local management contract (`task`, `event`, `finish`, plus
KSM-owned control). ACP is only the replaceable boundary used to start a real AI
runtime. MCP is not in the core loop. Deterministic ACKs, leases, retries,
heartbeats and state changes never consume model tokens.

## Where

- Canonical GitHub repo: `k3ss-official/kanban-surface-manager` (private)
- Current local build checkout:
  `/Volumes/deep-1t/Users/k3ss/Documents/Codex/2026-07-30/tell-me-all-about-higgsfield-apps-2/outputs/ksm-krp-sandbox`
- Deep evidence: `README.md`, `TEST_REPORT.md`, and `VPS_ACCEPTANCE.md` in that repo
- Control host: `ssh alwyzon-1` → `alwyz-dock-01`, SSH port 42
- Service: `ksm.service`; store: `/var/lib/ksm/state/ksm.sqlite3`;
  IPC: `/run/ksm/krp.sock`
- Planner identity: `hermes`; coding identity: `ksm-worker`
- The earlier `k3ss-official/kanban-surface` native-board/UI/MCP build is a
  retained design spike, not the active MVP control loop.

## Done

- 2026-07-30 — Shelved Ask Ebbi/ChadAI from `alwyzon-1` under Tony's explicit
  approval. Verified the local archive before purging the VPS copy:
  `/Volumes/hotblack-2tb/.config-bak/server-shelves/alwyzon-1/2026-07-30/ask-ebbi-shelf-20260730.tar.zst`,
  SHA-256 `9befda91a191a65418b5bfa2d9ffd179f114247808112bd6b3ccdff63b8f4fab`.
- 2026-07-30 — Re-established the host as a dark KSM MVP box: no application
  listener, zero Docker workload residue, UFW default-deny, Fail2ban/auditd
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

- `KSM-PILOT-1` is back at `READY`; the reviewed one-line correction is not yet
  applied. The isolated coding worktree remains clean.
- The current vertical slice still needs tests, commit, push, PR and final KSM
  transition to `REVIEW`.
- collab-mem projection is manual today. KSM needs a read/write projection that
  preserves this Git repo as the durable cross-surface source while avoiding
  duplicate writers and token-heavy polling.
- Prove in-flight lease recovery across a KSM restart.
- Add the execution policy around coding worktrees: explicit writable paths,
  command policy and default-deny egress.
- Decide whether to archive or selectively reuse the earlier
  `kanban-surface` UI/MCP spike after this vertical slice is accepted.

## Next

1. Codex: resume `KSM-PILOT-1`, apply the reviewed corrected patch through the
   explicit approval gate, run the full suite, push the worker branch, open a PR,
   and verify the KSM card ends in `REVIEW`.
2. Codex: implement and test the collab-mem projection against this project card;
   do not add a public endpoint or MCP server.
3. Tony + Codex: review the first real PR and decide the smallest useful surface
   UI after the projection and restart-recovery tests pass.
