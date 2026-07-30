# Kanban Surface — cross-project AgentOS execution layer over collab-mem
STATUS: active        UPDATED: 2026-07-30 by Codex

## What

Kanban Surface makes collab-mem's project catalog visible and executable without
replacing it. Each `projects/<slug>.md` card defines one isolated native Hermes
project board. Every board contains the same five worker stations: intake, build,
needs-input, verify, and done. Authenticated crew entities can read every project
board; project, action, and station policy constrains mutations.

## Where

- GitHub: `k3ss-official/kanban-surface`
- Canonical local checkout:
  `/Volumes/deep-1t/Users/k3ss/k3ss-official/kanban-surface`
- Implementation branch: `codex/agentos-multiboard`
- Durable project/work source: this collab-mem repository
- Architecture contract: `kanban-surface/docs/spec.md`
- Host/service endpoint: not selected or deployed

## Done

- 2026-07-30 — Rebased the design on Hermes Agent v0.19 native named boards:
  project board = workstream; station = assignee; move = dispatch.
- 2026-07-30 — Added collab-mem project discovery, live catalog refresh,
  project-namespaced idempotency, cross-project snapshots, UI selection, gateway
  routes, and six MCP tools.
- 2026-07-30 — Split each KBA into `SOUL.md` identity and `AGENTS.md` operating
  contract; installer registers the profiles through Hermes.
- 2026-07-30 — Removed literal credentials from configuration. `.env` is
  non-secret; `.op.env` accepts only 1Password references resolved by `op run`.
- 2026-07-30 — All offline, real-git integration, real-ledger drift, demo gateway,
  and isolated Hermes v0.19.0 lifecycle tests pass. The isolated proof caught and
  fixed Hermes `create`/`show` adapter mismatches before deployment.

## Outstanding

- No KSM host, public TLS surface, or production service is deployed.
- The five KBA profiles still need final AgentOS model and narrow toolset policy;
  the installer currently creates valid empty config overlays.
- The actual 1Password vault item references must be verified and placed in the
  ignored `.op.env`; no resolved value belongs in the repo.
- The target host must pass profile discovery plus all five live smoke paths before
  the board can be trusted for production dispatch.
- Kanban Surface MCP placement must remain selective: coordination entities get it;
  unrelated or sealed chats do not.

## Next

1. Tony + Codex: select the dedicated KSM host/security context, finalize the five
   KBA model/toolset policies and verified 1Password references, deploy locally on
   that host, then run smoke paths 1–5 before exposing any shared endpoint.
