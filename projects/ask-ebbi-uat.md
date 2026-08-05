# ask-ebbi-uat — Ask Ebbi planning intelligence private UAT

STATUS: active UAT        UPDATED: 2026-08-05 by Rae from live source/runtime reconciliation

## What

Ask Ebbi is a Hermes-native planning research and drafting assistant for UK
planning consultants: cited Q&A over UK planning law, PINS appeal precedents,
casefiles, style-learned drafting, intelligence sources and structured tester
feedback. The deterministic Library owns facts and citations; the dedicated
Ebbi Hermes agent owns inference and workflow orchestration.

This is the current product and UAT source of truth. Ask Ebbi 2/3 and the other
legacy repositories were deleted on Tony's order.

## Where

- **Canonical GitHub repo:** private `k3ss-official/ask-ebbi-uat`.
- **Reconciled snapshot:** `1c3d257ea35adaea2c6f8cd755fc330f0d00646b`.
- **Live source:** `alwyzon-1:/home/anwhelan/ask-ebbi`.
- **Live runtime:** Docker Compose project `ask-ebbi`, container `ebbi`, image
  reference `ask-ebbi-ebbi`, loopback `127.0.0.1:8051`.
- **Public UAT:** `https://askebbi.com`, protected by Cloudflare Access.
- **Persistence:** `ask-ebbi_ebbi_data` and `ask-ebbi_ebbi_hermes`.
- **Coexistence boundary:** KSM remains host-native. Ask Ebbi is the isolated
  Docker workload; KSM identities have no Docker-group membership.

## Reconciliation — 2026-08-05

The live recovery source contained two application changes absent from every
GitHub branch: the allowlisted UK relay (`app/ingest/uk_relay.py`) and its
Lancaster integration. The host also carried stale tests/build files from an
older snapshot. Rae:

1. captured and verified a temporary source archive, later deleted on Tony's order;
2. preserved the live application/relay code;
3. restored the known-good build/test baseline from `ask-ebbi-3`;
4. merged the UAT relay environment into the current Compose policy;
5. removed stale deployment claims and established this private repository;
6. copied the reconciled tracked tree back to the host without rebuilding or
   restarting the service;
7. set the ignored host `DEPLOYED_COMMIT` marker to the repository snapshot.

Verification:

- GitHub `main` and the host marker both resolve to `1c3d257`;
- all 182 tracked files match the host byte-for-byte;
- all 64 application-bearing files match between host and running container;
- secret scan found no credential/token/private-key patterns;
- only `data/README.md` is tracked under `data/`; runtime databases, named-volume
  data, credentials, logs and caches are excluded;
- `python3 -m pytest tests/ -q`: 233 passed, 18 deprecation warnings;
- container remains running and healthy;
- `/api/health`: status ok, 59,902 live Chroma chunks, zero style documents;
- `/api/ready`: core/database/embeddings/corpus/Hermes ready;
- UK relay configured and healthy; intelligence poller disabled.

The current recovery image has no OCI revision label. Source parity is proven,
but the next deliberate build must set `APP_SOURCE_COMMIT` so image provenance
is machine-readable again.

## Outstanding

- Continue Tony/Rae UAT and land accepted changes on `ask-ebbi-uat/main`.
- Do not create another source checkout or backup unless Tony changes the
  GitHub-plus-VPS-only source boundary.
- Add off-host uptime/alerting and complete smoke/rollback acceptance.
- Decide whether to enable the intelligence poller after data/licensing review.
- Rebuild only through the controlled UAT process; do not do an informal live
  `up --build`.

## Next

1. **Tony + Rae:** continue UAT and close verified feedback.
2. **Rae:** preserve GitHub/host/container provenance after each release.
3. **Tony:** approve broader-beta gates only after UAT acceptance.
