# FACTS — durable truths (state once; update when they change; never re-paste into daily/LOG)

## Current as of 2026-07-30
- **KSM host boundary:** `ssh alwyzon-1` → `anwhelan@203.34.137.49`, port
  **42**, live hostname `alwyz-dock-01`. Tony shelved Ask Ebbi and explicitly
  approved removal of all Ask Ebbi/ChadAI/Cloudflare/Docker MVP residue from this
  host. It is now the dedicated, dark Kanban Surface Manager MVP host.
- **Recoverability:** the verified shelf archive is local at
  `/Volumes/hotblack-2tb/.config-bak/server-shelves/alwyzon-1/2026-07-30/ask-ebbi-shelf-20260730.tar.zst`;
  SHA-256 is
  `9befda91a191a65418b5bfa2d9ffd179f114247808112bd6b3ccdff63b8f4fab`.
  The purged VPS copies require that archive or another existing backup to
  recover.
- **KSM runtime:** `ksm.service` is enabled and active with no network address
  family. Canonical state is `/var/lib/ksm/state/ksm.sqlite3`
  (`0640 ksm:ksm`). Agent IPC is `/run/ksm/krp.sock`
  (`0660 ksm:ksm-runtime`). Agents can use the socket only when KSM gives them
  a run capability; they cannot read the database.
- **Agent isolation:** `ksm`, `hermes`, and `ksm-worker` are separate locked
  service identities and none belongs to the Docker group. Hermes and Codex
  keep separate OAuth stores. Hermes is the planning runtime; Codex is the
  coding runtime.
- **Canonical implementation:** private GitHub repo
  `k3ss-official/kanban-surface-manager`. KRP/1 is the management protocol; ACP
  is a replaceable runtime adapter; MCP is deliberately outside the core loop.
- **Accepted release:** `9b98356249e4fbdd0c447e7e0930abb0e6d70df5`
  from merged PR #2. The live deterministic health check is read-only and
  returns `{"ok":true,"quick_check":"ok","schema":"ready"}`.
- **Ingress:** only SSH TCP 42 is externally listening. UFW default-deny,
  Fail2ban, auditd and unattended security updates remain active.
- **Superseded boundary:** the 2026-07-17 two-tenant ChadAI/Ask Ebbi plan for
  this VPS is historical. Ask Ebbi is shelved; no ChadAI or Ask Ebbi workload
  remains on `alwyzon-1`.

Older entries below are retained as historical provenance; when they conflict with this dated section, this section wins.

## Access
- **Historical alwyzon access:** `ssh alwyzon` → `anwhelan@46.102.156.75`, **port 42**, key `~/.ssh/old/cmh-master-2026`.
  **sudo is NOPASSWD.** HestiaCP box: also runs nginx/mail/DNS (socialite) — treat as shared.
  Always wrap remote commands in **`bash -lc`** (so `~/.local/bin`/`hermes` is on PATH).
  Interactive Hermes CLI needs a TTY: **`ssh -tt`**.
- **⚠ SUPERSEDED 2026-07-06:** the old `~/.hermes` arrangement on alwyzon is historical and must not be treated as the current two-tenant deployment.
  (Ask Ebbi ops agent; see `projects/ask-ebbi-2.md`). Chad's entire runtime is preserved at
  `alwyzon:~/quarantine/` (chad-hermes-20260706, 9.3GB + gateway unit + wrapper) — restorable by `mv`.
  The Telegram allowlist / gateway facts below
  belong to the quarantined Chad install.
- (Chad-era, quarantined) Gateway: `systemctl --user restart hermes-gateway.service`;
  Telegram allowlist Tony `7288169467`, Rae `7933677559`, Janet `8671002174`.

## Agent isolation (non-negotiable — two layers)
1. **One agent = one Hermes install = one home** (Tony, 2026-07-06). Profiles are for an agent's
   OWN sub-agents only. Never create/configure an agent inside another agent's `~/.hermes`
   (Rae on the M4; Ebbi on alwyzon). Check whose SOUL.md it is before touching any `~/.hermes`.
2. **GDPR wall (Chad):** wherever Chad runs, his install is sealed + hold-nothing; the backend
   crew operates from outside it. Control plane stays out of the data plane.

## Architecture (locked 2026-05-31)
- **Chad** = isolated, hold-nothing Hermes box (the PRODUCT). No memory between user sessions, by design.
- **Backend crew** = Tony + Rae + Grok, coordinating via **this git ledger** (off-box, agnostic) — NOT
  co-located inside Chad.
- **Grok Build** installs as its **own non-privileged VPS user** (when promoted from the M4 trial), no `~/.hermes` access.
- **Shared memory** = this repo. **Per-agent recall** (mem0 / holographic / a runtime's own) is each
  agent's own concern, NOT this repo. (Decided: don't put the shared ledger on mem0 — it would break
  the off-box, surface-agnostic property and add a dependency + GDPR processor.)

## Models
- **Chad + its delegate workers:** Nemotron (`nvidia/nemotron-3-super-120b-a12b`, provider `nous`) —
  the only authed inference provider on the box. `delegation.model` pinned to Nemotron.
- **grok-4 *inference* in Hermes** needs `XAI_API_KEY` (absent on the box; the "xAI OAuth" the doctor
  reports does NOT feed `provider:xai` inference). **Grok Build is separate** — auths via `grok-build login` (SuperGrok subscription).

## Hold-nothing status (Chad)
- Built-in memory off, user profiling off, `memory` + `session_search` toolsets disabled (CLI + Telegram),
  external provider none, `sessions.auto_prune` on. **Still TODO (M4):** purge `state.db`/`sessions/`/`pairing/` at session end.

## Time / locale
- Tony is in **Zanzibar (EAT, UTC+3)**. The EOD "03:00" cutoff = local time. _(confirm if travelling.)_

## Canonical pointers
- **ChadAI deep build cursor:** `/Volumes/deep-1t/Users/k3ss/k3ss-official/ChadAI/HANDOFF.md` (on the M4).
- **Chad worker skills:** `~/chad-skills` on the VPS (source of truth: `ChadAI/skills/` in the repo).
