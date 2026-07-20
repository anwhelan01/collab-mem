# ROSTER — who's who and who owns what

> How it works in practice: **Tony** at the top; **Rae** and **Grok** are his backend build crew;
> **Chad** is the product they build (not a crew member); **Janet** guards the perimeter.

## Current ownership boundary — 2026-07-17

- **Tony** — final operator and architect; approves external auth, destructive cleanup, data-boundary decisions and public cutovers.
- **Ask Ebbi Codex** — owns `/Volumes/deep-1t/Users/k3ss/k3ss-official/ask-ebbi-3`, the M4 Ask Ebbi containers (`8051` primary, `8052` demo), and the future Ask Ebbi Compose project on `alwyz-01-dock`.
- **ChadAI Codex** — owns `/home/anwhelan/apps/chadai-prod` and the ChadAI containers, networks, volumes and internal Open Notebook service on `alwyz-01-dock`.
- **Isolation rule** — the two lanes do not modify the other tenant's Compose project, Hermes home, networks, volumes, databases or secrets. Coordination happens here in Git, not by sharing runtime state.

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
