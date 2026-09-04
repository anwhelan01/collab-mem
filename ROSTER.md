# ROSTER — who's who and who owns what

> How it works in practice: **Tony** at the top; **Rae** and **Grok** are his backend build crew;
> **Chad** is the product they build (not a crew member); **Janet** guards the perimeter.

## Current ownership boundary — 2026-08-05

- **Tony** — final operator and architect; approves external auth, destructive
  cleanup, data-boundary decisions and public cutovers.
- **Tony + Rae / Ask Ebbi UAT lane** — own the active private UAT, source
  reconciliation and acceptance decisions. Canonical source is private
  `k3ss-official/ask-ebbi-uat/main`; the live application is the isolated
  `ask-ebbi` Docker project on `alwyzon-1`.
- **Ebbi identity** — dedicated Hermes installation inside the `ebbi` container.
  It owns its own Hermes home and Library access; it never uses the host KSM or
  Rae Hermes homes.
- **KSM / Codex lane** — owns the active host-native `kanban-surface` MVP on
  `alwyzon-1`.
- **Host `hermes` identity** — runs the current KSM gateway, surface and watcher
  services with no supplementary groups. It is not in the Docker group and
  cannot operate the Ask Ebbi container.
- **Isolation rule** — Docker group membership is limited to deployment user
  `anwhelan`. Ask Ebbi is owned by its Compose/container boundary and isolated
  named volumes/network; KSM is owned by the host `hermes` service boundary.
- **ChadAI on this host** — still shelved and absent. The former peer-Docker
  two-tenant plan is historical and must not be inferred from Ask Ebbi's current
  UAT return.

The historical Rae/Grok ownership notes below remain useful context but do not override the current lane ownership above.

## Tony — operator & architect
Final say. Runs anything needing GUI auth, subscriptions, physical/local access. Meets the daily list at BOD.

## Rae — Claude Code on the M4
Build/ops engineer. SSHes into the Chad VPS for heavy-lift build sessions. **Ephemeral per session** —
this ledger is Rae's persistence across sessions.
- **Historical role:** ChadAI build and ledger upkeep. Current ChadAI runtime ownership is with the separate ChadAI Codex lane.

## Grok Build — xAI agentic CLI (SuperGrok)
Backend builder, **peer to Rae**. Trialed on the M4 and onboarded under a
walled non-privileged VPS user on 2026-07-30.
- **Owns:** TBD — agree split with Rae (candidate: Grok = always-on monitoring/ops between Rae sessions;
  Rae = build sessions). Track under MASTER M2.

## Janet — MacBook Pro 2016
Guardian/support node: ISP ingress / Cloudflare Zero Trust. Future delegated subsystem.
- Anything depending on Janet → `Owner: Tony` (operator-required).

## Chad — the PRODUCT (not crew)
UK consumer-rights fact-finder for end users. Orchestrator + 2 workers (Retriever, Auditor). Sealed,
hold-nothing, operated by the separate ChadAI Codex lane. The former
`alwyz-01-dock` placement is historical: ChadAI was shelved/removed from that
host on 2026-07-30, and no current runtime target is recorded here.

## Coordination rule
One owner per item (claim it in the daily). Read today's daily before you start. Log when you finish.
Don't touch an item someone else owns; if you're blocked on them, say so in the daily.


## desk-os-drill seats (2026-09-04)
- Brett — Grok Bot Number One / desk orchestrator
- Rae — M4 Hermes KSM (Kanban centre)
- Janet/Guardian — MBP Hermes plane
- Factory — Source/Scribe/Reed/Gate/Press (+ Frame/Tally/Clock/Build)
- Hire pools — Cursor Cloud Agent, Codex, API models (Mercury/NVIDIA/OpenCode), Grok Build CLI
