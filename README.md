# collab-mem

The crew's shared brain. **Off-box on purpose** — it lives in git on GitHub, not on the M4,
the VPS, or Janet — so no single machine going down takes out coordination, and every surface
(any AI, any tool, any human) reaches the same neutral ground. **Surface-agnostic by design**:
it's just markdown in git. No SDK, no API, no key.

## The one job
Let Tony, the orchestrator, and **anyone** — human or AI — instantly know:
**what's being worked on · what's been done · what the very next step is** — and pick up the
relay baton and continue seamlessly.

## Picking this up? Read in this order
1. **[daily/](daily/) → today's file** (`daily/YYYY-MM-DD.md`) — the current task, its status, the
   next step, today's list. **This is where you start.**
2. **[projects/](projects/) → the project you were sent for** (`projects/<name>.md`) — one card per
   project: what · where · done · outstanding · **next**. "Go look in collab-mem for `<name>` and
   tell me what's next" resolves here.
3. **[MASTER.md](MASTER.md)** — the master todo list (the durable backlog every daily hangs off).
4. **[FACTS.md](FACTS.md)** — durable where/why: access, the isolation walls, architecture, models.
5. **[ROSTER.md](ROSTER.md)** — who's who and who owns what.
6. **[LOG.md](LOG.md)** — what's been done so far (history, newest on top; rarely needed).

## The daily ritual (this is the engine)
- **EOD** — when Tony calls it, or **03:00 local (whichever is sooner)** — the active AI generates
  **tomorrow's** `daily/YYYY-MM-DD.md`:
  1. Carry over any unfinished items from today.
  2. Note **which item is the current task** and its **exact status**.
  3. Add any new items (synced against MASTER.md).
- **BOD** — Tony meets the list with the crew: add / edit / adjust.
- Today's daily file IS the "now" — its header always answers what's-being-worked-on / next-step.

## Protocol (KISS — don't break it)
- **Claim before you work.** Put your name on the item's `Owner` and commit. One owner per item.
- **Master is the backlog; daily is today's slice.** Daily items cite their MASTER id (e.g. `M3`).
- **State durable facts once** in FACTS.md — never re-paste them into daily/LOG.
- **Log what you finished** — one LOG.md line, newest on top. Operational, not literary.
- **Max 5 active items** in a daily unless Tony approves more. **No duplicate items.**
- Anything needing Tony / Janet / GUI auth / a subscription / physical access → `Owner: Tony`.

## What this is NOT
Not an agent's private memory. Per-agent recall (mem0, holographic, a runtime's own memory) is
each agent's own business. This repo is the **shared** layer only.
