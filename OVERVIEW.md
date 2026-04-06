# collab-mem

Private operational memory repo for Tony and Rae.

## Purpose

This repo exists so any person, AI, or agent can open one file and immediately understand:
- what's going on
- what's been done
- what's next

That is the whole point.

## Relationship to `my-hermes`

`my-hermes` is the actual system build.
`collab-mem` is the simple daily shared memory ledger beside it.

Think of it as:
- `my-hermes` = the machine
- `collab-mem` = the daily operational snapshot

## Structure

- `OVERVIEW.md` = what this repo is and how to read it
- `daily/YYYY-MM-DD.json` = the one daily file for that date

## Golden rule

Every daily file must let a reader see, in this order:
1. what's going on
2. what's been done
3. what's next

If that is not instantly obvious, the file is wrong.

## Rules

- one file per day
- one todo list per day
- max 5 active todos unless Tony approves more
- no duplicate active todos
- unfinished work can carry forward, but do not duplicate it as a new task
- keep it operational, not literary
- keep it simple enough that another AI can continue without guesswork

## Daily JSON shape

```json
{
  "date": "YYYY-MM-DD",
  "project": "my-hermes",
  "going_on": {
    "summary": "Current state in one tight paragraph",
    "status": [
      "Important current fact",
      "Important current fact"
    ],
    "blockers": [
      "Current blocker"
    ]
  },
  "done_today": [
    "Completed item",
    "Completed item"
  ],
  "next": {
    "todo": [
      {
        "id": "YYYY-MM-DD-01",
        "task": "Clear actionable task",
        "status": "pending",
        "notes": "Optional short note"
      }
    ],
    "later": [
      "Not for today, but relevant next"
    ]
  },
  "handoff": {
    "for_other_ai": [
      "Work another AI/model/agent may need to pick up"
    ],
    "operator_required": [
      "Work that needs Tony, Janet, GUI auth, OS action, or physical/local access"
    ]
  }
}
```

## Janet

Janet is Tony's guardian/support node in the wider system design. In current known context, Janet is the MacBook Pro 2016 handling ISP ingress / Cloudflare Zero Trust and is also treated as a future delegated subsystem.

If work depends on Janet being online, available, authenticated, or used as a separate execution surface, put it under `handoff.operator_required`.
