# collab-mem

Private operational memory repo for Tony and Rae.

## Purpose

`collab-mem` is the single daily point of reference shared across different AI surfaces, models, tools, and sessions while the larger global memory system is still evolving.

It is intentionally simple:
- one private GitHub repo
- one overview file
- one JSON file per day
- one todo list per day
- maximum 5 active todos unless Tony explicitly approves more

This repo is not a diary and not a dumping ground. It is an operational memory spine.

## Relationship to `my-hermes`

`my-hermes` is the actual AI operating system build: Hermes Agent, Rae, gateway, skills, Google Workspace, cron, voice, memory plumbing, and system orchestration.

`collab-mem` is the lightweight daily coordination layer that sits beside it.

Think of it like this:
- `my-hermes` = the machine
- `collab-mem` = the shared working memory ledger

## How it works

Each day gets exactly one JSON file under `daily/`.

Example:
- `daily/2026-04-06.json`

That file contains:
1. top-level summary
2. one approved todo list for the day
3. tasks completed that day
4. current overall status
5. candidates for tomorrow
6. a handoff section from Rae to any other AI/model/agent that may need to pick up work Rae cannot directly do
7. an operator-required / Janet-required section for tasks that need Tony, Janet, local machine access, credentials, OS-level changes, GUI work, or anything outside Rae's current execution surface

## Rules

- one file per day
- one todo list per day
- never duplicate the same active todo as a new todo entry
- unfinished todos may carry forward, but keep the same ID where possible
- maximum 5 active todos unless Tony explicitly approves more
- completed items belong in `completed_today`, not back in `todo`
- `tomorrow_candidates` are ideas, not commitments
- this repo is private and intended only for Tony and credentialed AI agents working on Tony's systems

## JSON schema

Each daily file should follow this shape:

```json
{
  "date": "YYYY-MM-DD",
  "project": "my-hermes",
  "summary": "Short current-state paragraph",
  "todo": [
    {
      "id": "YYYY-MM-DD-01",
      "task": "Clear actionable task",
      "status": "pending",
      "source": "new | carried_over | operator_directive",
      "notes": "Optional short note"
    }
  ],
  "completed_today": [
    "Completed item"
  ],
  "current_status": {
    "overall": "Current state",
    "blockers": ["Blocking issue"],
    "active_focus": ["Focus area"]
  },
  "tomorrow_candidates": [
    "Candidate item"
  ],
  "ai_handoff": {
    "message": "Instructions or context for any other AI taking over some work",
    "tasks_best_done_by_other_ai": ["Task"],
    "constraints": ["Constraint"]
  },
  "operator_required": {
    "message": "Tasks requiring Tony, Janet, GUI access, local auth, physical presence, or OS-level action",
    "items": ["Item"]
  }
}
```

## Update policy

When updating a daily file:
- preserve the todo IDs
- do not create duplicate todos for the same unresolved task
- move done work into `completed_today`
- keep `current_status` tight and real
- keep the repo boring, readable, and machine-safe

## Janet

Janet refers to Tony's guardian/support node in the wider system design. In current known context, Janet is the MacBook Pro 2016 handling ISP ingress / Cloudflare Zero Trust and is also treated conceptually as a daughter subsystem that will later take on work Rae cannot or should not do directly.

In this repo, anything that depends on Janet being online, available, authenticated, or delegated should go under `operator_required.items` or be explicitly mentioned in `ai_handoff`.
