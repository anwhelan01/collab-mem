# ROSTER — who's who and who owns what

> How it works in practice: **Tony** at the top; **Rae** and **Grok** are his backend build crew;
> **Chad** is the product they build (not a crew member); **Janet** guards the perimeter.

## Current ownership boundary — 2026-07-30

- **Tony** — final operator and architect; approves external auth, destructive
  cleanup, data-boundary decisions and public cutovers.
- **KSM / Codex lane** — owns the active `kanban-surface-manager` MVP and the
  dedicated `alwyzon-1` control host.
- **`ksm` identity** — deterministic control plane and only canonical SQLite
  writer. It grants, resumes and cancels work.
- **`hermes` identity** — planning runtime. It receives only a scoped KSM run
  capability and reaches KSM through the local KRP socket.
- **`ksm-worker` identity** — Codex coding runtime with a repo-scoped GitHub
  deploy key. It uses isolated worktrees and cannot read KSM state directly.
- **Isolation rule** — all three service identities are locked, have separate
  homes/credentials, run without any Docker runtime/group, and share only the
  `ksm-runtime` socket group where required.
- **Ask Ebbi / ChadAI on this host** — shelved and removed under Tony's explicit
  approval. Their previous two-tenant ownership notes are historical only.

The historical Rae/Grok ownership notes below remain useful context but do not override the current lane ownership above.

## Tony — operator & architect
Final say. Runs anything needing GUI auth, subscriptions, physical/local access. Meets the daily list at BOD.

## Rae — Claude Code on the M4
Build/ops engineer. SSHes into the Chad VPS for heavy-lift build sessions. **Ephemeral per session** —
this ledger is Rae's persistence across sessions.
- **Historical role:** ChadAI build and ledger upkeep. Current ChadAI runtime ownership is with the separate ChadAI Codex lane.

## Grok Build — xAI agentic CLI (SuperGrok)
Backend builder, **peer to Rae**. Trialed on the M4 now; will run as a walled non-privileged user on
the VPS once promoted.
- **Owns:** TBD — agree split with Rae (candidate: Grok = always-on monitoring/ops between Rae sessions;
  Rae = build sessions). Track under MASTER M2.

## Janet — MacBook Pro 2016
Guardian/support node: ISP ingress / Cloudflare Zero Trust. Future delegated subsystem.
- Anything depending on Janet → `Owner: Tony` (operator-required).

## Chad — the PRODUCT (not crew)
UK consumer-rights fact-finder for end users. Orchestrator + 2 workers (Retriever, Auditor). Sealed,
hold-nothing, on the ChadAI tenant of `alwyz-01-dock`. The ChadAI Codex operates it from outside the wall.

## Coordination rule
One owner per item (claim it in the daily). Read today's daily before you start. Log when you finish.
Don't touch an item someone else owns; if you're blocked on them, say so in the daily.
