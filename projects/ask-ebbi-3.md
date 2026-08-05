# ask-ebbi-3 — Ask Ebbi planning intelligence product (the canonical repo)

STATUS: active UAT        UPDATED: 2026-08-05 by Rae from live deployment evidence

## What

Ask Ebbi is a Hermes-native planning research and drafting assistant for UK planning consultants:
cited Q&A over UK planning law, PINS appeal precedents, NotebookLM-style casefiles,
style-learned drafting and structured tester feedback, orchestrated by **Ebbi**, a dedicated
Hermes agent behind one dashboard. The Ask Ebbi Library owns deterministic retrieval and
citations. **This is one product** — no separate LandPro product or roadmap.

The project was reopened by Tony and is now in private UAT. The 2026-07-30 shelf decision is
historical provenance, not current operating state.

## Where

- **Canonical local repo:** `/Volumes/deep-1t/Users/k3ss/k3ss-official/ask-ebbi-3/`, checked out on
  `feat/uncle-demo-feedback`.
- **Canonical GitHub repo:** private `k3ss-official/ask-ebbi-3`.
- **Active UAT source branch:** `feat/uncle-demo-feedback`.
- **Deployed application revision:** `bd16dc04cc2ed3d658ad9feb0f524aedb8920598`
  (`restore casefile example and fix UAT links`, 2026-08-04). The image label and
  `/home/anwhelan/ask-ebbi/DEPLOYED_COMMIT` agree. At deployment this branch was five commits ahead
  of `main` and zero behind; `main` has not yet been promoted to the accepted UAT state.
- **Live UAT host:** `alwyzon-1` / hostname `alwyz-dock-01`, user `anwhelan`.
- **Runtime:** Docker Compose project `ask-ebbi`, container `ebbi`, image `ask-ebbi-ebbi`, loopback
  `127.0.0.1:8051`, using `/home/anwhelan/ask-ebbi/compose.yaml` plus
  `compose.uat.yaml` and deployment environment `/etc/ask-ebbi/ask-ebbi.env`.
- **Public UAT:** `https://askebbi.com`, protected by Cloudflare Access. Anonymous root and health
  requests receive the expected Access login redirect rather than the origin response.
- **Persistent state:** `ask-ebbi_ebbi_data` → `/app/data` and
  `ask-ebbi_ebbi_hermes` → `/home/ebbi/.hermes`.
- **Coexistence boundary:** KSM remains a host-native systemd stack. Ask Ebbi is the single Docker
  workload, isolated on bridge `ask-ebbi-uat`; no KSM identity is in the Docker group.

## Live verified state — 2026-08-05

- Container created and started 2026-08-04 16:05 UTC; status `running`, health `healthy`.
- `/api/health`: `status=ok`, 59,294 Chroma chunks, 0 loaded style documents.
- `/api/ready`: core, database, embeddings, corpus and Hermes all ready/configured.
- Appeals source note: 91,212 ingested cases; `/api/appeals/stats` currently reports 91,019
  outcome-bearing/statistical rows, 24,244 allowed and a 26.6% success rate.
- OpenAPI: 29 paths on the loopback origin; public API documentation remains blocked by the public
  host policy and Cloudflare Access.
- Runtime: Hermes Agent v0.19.1 (2026.7.30), Python 3.12.13 application runtime.
- Exact deployed application revision test suite: 233 passed, 18 deprecation
  warnings and no failures on 2026-08-05.
- Controls: edge authentication enabled; 10-minute sessions; inference concurrency 2; queue timeout
  5s; inference timeout 300s; request cap 12/minute; intelligence poller disabled for UAT.
- Container limits: 3.5 GB RAM, 1 GB reservation, 5 GB memory+swap ceiling, 2 CPUs, 512 PIDs,
  `on-failure:3`, Docker `local` logging at 10 MB × 3.
- Current use at inspection: about 669 MiB RAM and 35 PIDs; host root disk 44% used.
- Initial checksum comparison against the deployed GitHub revision found identical tracked content.
  Current operational docs were then reconciled on the same branch and synced to the host without a
  rebuild/restart; application code and the running image remain at `bd16dc04`. The live tree also
  contains `DEPLOYED_COMMIT` and one unreferenced duplicate transparent-logo asset.

## Done (newest first)

- **2026-08-05 — canonical truth reconciled from live UAT.** Replaced the stale paused/M4 record with
  the verified alwyzon-1 deployment, branch, commit, runtime, health, data counts and access boundary.
- **2026-08-04 — current UAT build deployed.** The five-commit
  `feat/uncle-demo-feedback` release added UAT authentication/feedback polish, mobile responsiveness,
  the pilot model, isolated Library MCP Python and restored casefile/UAT links; deployed revision is
  `bd16dc04`.
- **2026-07-30 — historical shelf/cleanup.** The prior Ask Ebbi tenant was removed and archived before
  the current UAT deployment was later installed. Do not mistake this historical event for current
  host state.
- **2026-07-12 — canonical repo consolidated** from the ask-ebbi-2 lineage plus recovered production
  assets. Historical predecessor: [ask-ebbi-2](ask-ebbi-2.md).

## Outstanding

- Continue the Tony/Rae UAT loop against the live Access-protected service and reconcile only verified
  findings.
- Decide after UAT acceptance whether to fast-forward/promote `feat/uncle-demo-feedback` to `main`.
  Do not treat stale `ui-console-alt` PR #2 as the promotion path; it conflicts with current `main`.
- Capture a current encrypted/restorable backup of both named volumes and verify restore manifests
  before broader beta use.
- Add off-host uptime/alerting and complete the remaining production smoke/rollback acceptance.
- Decide whether the UAT-disabled intelligence poller should remain disabled or be enabled after its
  data/licensing behavior is accepted.
- Remove or deliberately track the unreferenced duplicate static logo during a later hygiene pass;
  it is not a UAT blocker.

## Next (ordered)

1. **Tony + Rae:** continue the current Ask Ebbi UAT and close the verified feedback loop.
2. **Rae:** keep the source branch, deployed revision and project records aligned after each accepted
   UAT change.
3. **Tony:** after acceptance, approve branch promotion to `main` and the broader-beta cutover gates.
