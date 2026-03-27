# American Airlines API - Master Documentation Pack

This is the consolidated index and navigation page for the full documentation set.

## Document Map

1. `docs/RESOURCE_INDEX.md`
   - Complete inventory of code, scripts, tests, platform artifacts, and docs.
2. `exchange-docs/home.md`
   - Exchange-facing solution overview and architecture diagram.
3. `docs/HLD_LLD.md`
   - High-level and low-level design (architecture, components, flows, deployment).
4. `docs/PROJECT_LIFECYCLE_AND_WORKFLOWS.md`
   - Delivery lifecycle and workflow-level implementation details.
5. `docs/PROMPT_EXECUTION_LEDGER.md`
   - Successful prompt log, execution windows, commands, and failure resolutions.
6. `docs/CURSOR_TDLC_METRICS_FILLED.md`
   - Filled Cursor matrix for attached TDLC capability/task evaluation sheets.
7. `docs/EXECUTIVE_SCORECARD.md`
   - Presentation-ready one-page leadership summary of Cursor TDLC performance.

## Postman Playbook

- Primary import-ready collection:
  - `postman/american-airlines-info-api-playground.postman_collection.json`
- Alternate collections:
  - `postman/american-airlines-info-api-cloudhub.postman_collection.json`
  - `postman/american-airlines-info-api.postman_collection.json`

Each request includes purpose-oriented naming and descriptive usage text.

## Exchange Documentation Publication

- Source markdown:
  - `exchange-docs/home.md`
- Publish path used:
  1. Update Exchange draft page `home`
  2. Publish asset documentation state

## Operational Verification Snapshot

- Actual app retained and running on CloudHub: `american-airlines-info-app-dev`
- Minimal validation app removed.
- Scheduler (`scheduledOperationalHeartbeat`) repeatedly validated from runtime logs.
- DataWeave endpoints validated with correct payload forms.
- API specification synced to Design Center and published to Exchange (`1.0.1`).

## Word Version

All above documentation is also consolidated into:

- `docs/MASTER_DOCUMENTATION.docx`
- `docs/MASTER_DOCUMENTATION_latest.docx` (lock-safe latest export)

This Word file is generated from this documentation set for stakeholder sharing.
