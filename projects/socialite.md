# socialite — Socialite Design client pipeline (FIND→RESEARCH→BIBLE→BUILD→PITCH)

STATUS: active        UPDATED: 2026-07-20 by Rae (Claude Code)

## What

Internal pipeline for Socialite Design: finds small hospitality businesses running on a bare
Facebook page with no real website, researches them deeply, hand-builds a landing page, arms a
sales rep with a pitch sheet on a residual ladder, then deploys on Socialite's own VPS and tracks
recurring services. One-command run (`./run.sh "<biz>, <city>" [locale]`) drives the full
FIND → RESEARCH → BIBLE → BUILD → PITCH chain. Picked as the cash-flow-now lane in the
2026-06-06 GitHub estate audit; backlog detail lives in MASTER M12–M14 (Field Closer Pack,
Ghost outreach layer, Titan intelligence feed).

## Where

- **Repo:** `k3ss-official/socialite`, branch **main** — latest `b71646a` ("brining it all upto
  date"), pushed 2026-07-20.
- **Hosting:** deploys to Socialite's own VPS; nginx/mail/DNS for the domain shares the alwyzon
  HestiaCP box (see FACTS.md → Access — "also runs nginx/mail/DNS (socialite) — treat as shared").

## Done (newest first)

- **2026-07-20** — `b71646a` "brining it all upto date" — pushed and backed up.
- `7568d13` — REBUILD.md playbook + Scran Away bible v2 (fresh-system dry run).
- `bae3bb4` — Scrapling field test: consent-wall diagnosis, MCP-vs-library doctrine, serial-only
  rule, agent scoping.

## Outstanding

- This card was written from a repo sync, not a build session — Field Closer Pack v0 (`pack.py`
  generator, `field.socialite.design/<lead-id>` pocket UI) and the Ghost outreach wiring (MASTER
  M12/M13) haven't been re-verified against this commit.
- First-paying-client gate (Sunday 10-visit field push, ≥1 closed deal, MASTER M12) — outcome not
  recorded here; check with Tony / field team before assuming it landed.

## Next (ordered)

1. Confirm MASTER M12–M14 status against current `main` and update this card + MASTER together
   (they've drifted apart since the last build-session daily).
2. Owner: whoever next picks up Socialite — read MASTER M12–M14 before continuing.
