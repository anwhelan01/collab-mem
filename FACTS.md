# FACTS — durable truths (state once; update when they change; never re-paste into daily/LOG)

## Current as of 2026-07-17
- **Shared VPS boundary:** `ssh alwyz-01-dock` → `anwhelan@203.34.137.49`, port **42**. This host is intentionally limited to two isolated Docker installations: ChadAI and Ask Ebbi.
- **ChadAI tenant:** `/home/anwhelan/apps/chadai-prod`, owned by the separate ChadAI Codex lane. Its healthy Compose project includes the internal Open Notebook service and Factbox/SurrealDB resources. Current private networks are `chadai_core`, `chadai_edge`, and `chadai_admin_egress`; persistent resources are ChadAI-owned `chadai-*` volumes. Do not attach Ask Ebbi to them.
- **Ask Ebbi tenant:** not deployed to this VPS yet. Its future Compose project will have its own project name, private networks, persistent volumes, Hermes home, secrets and resource limits. It must not reuse ChadAI's Open Notebook, databases, networks or credentials.
- **Ask Ebbi M4 staging:** local primary container `ebbi` on `127.0.0.1:8051`; UI demo `ebbi-rae` on `127.0.0.1:8052`. Public `https://askebbi.com` currently returns the health endpoint successfully. Canonical local repo: `/Volumes/deep-1t/Users/k3ss/k3ss-official/ask-ebbi-3`; canonical GitHub repo: `k3ss-official/ask-ebbi-3` (private).
- **Ask Ebbi ownership:** this Ask Ebbi Codex lane owns `ask-ebbi-3`, its M4 containers and its future VPS deployment. The separate ChadAI Codex lane owns ChadAI only. Neither lane changes the other tenant's Compose project, volumes, networks or secrets.
- **SOT boundary:** ChadAI's internal Open Notebook is not an Ask Ebbi source-of-truth vault. Any future legacy-citation vault must preserve source URL/publisher, retrieval timestamp, SHA-256, case number and page/span provenance, with append-only corrections.

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
