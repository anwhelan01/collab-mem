# FACTS — durable truths (state once; update when they change; never re-paste into daily/LOG)

## Current as of 2026-08-05
- **Host boundary:** `ssh alwyzon-1` → `anwhelan@203.34.137.49`, port **42**,
  live hostname `alwyz-dock-01`, Ubuntu 24.04.4 LTS. The 2026-07-30
  KSM-only/dark-host statement is superseded: KSM remains host-native and Ask
  Ebbi has since returned as one isolated Docker UAT workload. ChadAI remains
  absent.
- **Ask Ebbi UAT runtime:** Docker Compose project `ask-ebbi`, container `ebbi`,
  image `ask-ebbi-ebbi`, loopback `127.0.0.1:8051`, bridge `ask-ebbi-uat`.
  Compose uses `/home/anwhelan/ask-ebbi/compose.yaml` plus
  `compose.uat.yaml` and deployment environment
  `/etc/ask-ebbi/ask-ebbi.env`. Never print the environment file.
- **Ask Ebbi source identity:** private repo `k3ss-official/ask-ebbi-uat`, branch
  `main`, reconciled snapshot `1c3d257ea35adaea2c6f8cd755fc330f0d00646b`.
  GitHub, local checkout, all 182 host tracked files and the host
  `DEPLOYED_COMMIT` marker agree. Tony ordered the remote historical repositories
  `askebbi-planner-tool`, `Ask-Ebbi`, `ask-ebbi-2` and `ask-ebbi-3` deleted on
  2026-08-05; verified GitHub API responses are now 404. Local checkouts were
  outside that deletion order and remain untouched. The current recovery image
  has no OCI revision label; the next deliberate
  build must restore machine-readable `APP_SOURCE_COMMIT` provenance.
- **Ask Ebbi health/data:** current container is running and healthy.
  `/api/health` reports 59,902 live Chroma chunks;
  `/api/ready` reports database, embeddings, corpus and Hermes ready/configured.
  The appeals source note records 91,212 ingested cases; current stats contain
  91,019 rows, 24,244 allowed, 53,586 dismissed and 26.6% success. Loopback
  OpenAPI contains 29 paths.
- **Ask Ebbi access/control:** `https://askebbi.com` is private UAT behind
  Cloudflare Access; anonymous root and health requests receive the expected
  Access login redirect. Edge auth and Hermes gateway are enabled. Sessions are
  10 minutes; inference concurrency is 2, queue timeout 5 seconds, request
  timeout 300 seconds and request cap 12/minute. The intelligence poller is
  disabled for current UAT.
- **Ask Ebbi isolation/resources:** named volumes `ask-ebbi_ebbi_data` and
  `ask-ebbi_ebbi_hermes` own app and agent state. The container is capped at
  3.5 GB RAM, 2 CPUs and 512 PIDs, with Docker `local` logs at 10 MB × 3.
  Ebbi runs Hermes Agent v0.19.1 inside the container and never uses host KSM or
  Rae Hermes homes.
- **Ask Ebbi source reconciliation:** all 182 tracked repository files match
  `/home/anwhelan/ask-ebbi` byte-for-byte; all 64 application-bearing files
  match between host and running container. Runtime databases, named-volume
  state, credentials, logs, caches and `DEPLOYED_COMMIT` are deliberately
  untracked. The reconciled suite passes 233 tests. Reversible pre-sync archive:
  `/home/anwhelan/ask-ebbi-backups/source-before-uat-repo-20260805T125430Z.tar.gz`.
- **KSM runtime:** `hermes-ksm.service`, `kanban-surface.service` and
  `kanban-watcher.service` remain enabled and active. Canonical state is
  `/var/lib/hermes/.hermes/kanban/boards/kanban-surface/kanban.db`.
  The retired `/run/ksm/krp.sock` is absent.
- **KSM identity/isolation:** all three current KSM services run as the locked
  host `hermes` identity with no supplementary groups. The Docker group contains
  only deployment user `anwhelan`; host `hermes` cannot operate Docker. Ask
  Ebbi's container and named volumes remain outside the KSM identity boundary.
- **KSM implementation:** private GitHub repo `k3ss-official/kanban-surface`.
  Accepted release `346b1e851169` from active `main`; the former
  `kanban-surface-manager` repo and KRP pilot release are historical.
- **Ingress:** SSH TCP 42 remains the direct public listener. Ask Ebbi binds only
  to loopback and is reached through the outbound Cloudflare connector; KSM UI
  remains loopback-only at 8742. UFW default-deny, Fail2ban, auditd and
  unattended security updates remain active.
- **Host capacity at inspection:** about 6.3 GiB RAM available, 4 GiB swap and
  44% of the 232 GiB root filesystem used. Ask Ebbi used about 669 MiB and 35
  PIDs.

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
