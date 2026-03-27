# American Airlines Info API (Mule 4)

Production-style MuleSoft implementation for airline flight operations, DataWeave transformation demos, scheduler-based health operations, CloudHub 2.0 deployment, and Exchange/Design Center lifecycle integration.

## Quick Start

- Runtime target: Mule 4.6.28 (Java 17)
- Deployment target: CloudHub 2.0
- Active app: `american-airlines-info-app-dev`

## Documentation Hub

- Master navigation: `docs/MASTER_DOCUMENTATION.md`
- Executive scorecard: `docs/EXECUTIVE_SCORECARD.md`
- Full resource index: `docs/RESOURCE_INDEX.md`
- HLD/LLD: `docs/HLD_LLD.md`
- Lifecycle and workflows: `docs/PROJECT_LIFECYCLE_AND_WORKFLOWS.md`
- Prompt ledger (commands, failures, fixes): `docs/PROMPT_EXECUTION_LEDGER.md`
- Platform operations runbook (assets/APIM/contracts): `docs/PLATFORM_OPERATIONS_RUNBOOK.md`
- Detailed regression matrix (input/expected/observed): `docs/REGRESSION_TEST_RESULTS_DETAILED.md`
- Filled TDLC metrics (Cursor): `docs/CURSOR_TDLC_METRICS_FILLED.md`
- Consolidated Word pack: `docs/MASTER_DOCUMENTATION.docx`
- Exchange-facing page: `exchange-docs/home.md`

## API Surface

- Business endpoints under `/api/*` (APIKit)
- DataWeave demo endpoints:
  - `POST /dw/simple`
  - `POST /dw/complex`
  - `POST /dw/multi1`
  - `POST /dw/multi2`
- Scheduler flow:
  - `scheduledOperationalHeartbeat` (DB heartbeat + optional external logging)

## Project Structure

- Mule flows: `src/main/mule/`
- RAML and examples: `src/main/resources/api/`
- DataWeave scripts: `src/main/resources/dwl/`
- Deployment and platform scripts: `scripts/`
- Postman collections: `postman/`
- Tests and MUnit resources: `src/test/`, `tests/`

## Operational Scripts

- Deploy CloudHub2: `scripts/deploy-cloudhub2.ps1`
- Check runtime/deployment: `scripts/check-cloudhub2.ps1`
- Publish API spec to Exchange: `scripts/publish-api-spec.ps1`
- Setup API Manager artifacts: `scripts/setup-api-manager.ps1`
- Generate Word doc pack: `scripts/generate-doc-pack.py`

## Notes

- Environment-specific properties are used:
  - `config-${mule.env}.properties`
  - `config-${mule.env}-secure.properties`
- Local CH2 investigation logs are intentionally ignored via `.gitignore`.

## Postman Import (Recommended)

- Primary playground collection (all services, descriptive names/purpose text):
  - `postman/american-airlines-info-api-playground.postman_collection.json`
- Environments:
  - `postman/american-airlines-info-api-cloudhub.postman_environment.json`
  - `postman/american-airlines-info-api-local.postman_environment.json`
