# American Airlines API - Master Resource Index

This index catalogs all major project resources, operational artifacts, and governance documentation for the MuleSoft delivery.

## 1) Core Project Definition

- `pom.xml` - Mule application build, dependency, deployment, and profile configuration.
- `mule-artifact.json` - Mule app metadata (`name`, `requiredProduct`, Java compatibility).
- `settings.xml` - Maven repository settings for local build/deploy flows.
- `munit-settings.xml` - Maven repository settings specialized for MUnit and Mule repos.

## 2) Mule Flow Implementations

- `src/main/mule/interface.xml`
  - APIKit listener/router and API console flow.
  - Global HTTP response/error behavior.
  - Autodiscovery binding configuration (deferred registration follow-up path).
- `src/main/mule/implementation.xml`
  - Business logic for CRUD, batching, validation, and integration-side logic.
- `src/main/mule/global.xml`
  - Shared config: DB, HTTP listener, object stores, outbound request configs, env props.
- `src/main/mule/ops.xml`
  - Scheduler service (`scheduledOperationalHeartbeat`) with DB check and optional external logging.
- `src/main/mule/dw-demo-flows.xml`
  - DataWeave endpoint wrappers for `/dw/simple`, `/dw/complex`, `/dw/multi1`, `/dw/multi2`.

## 3) API Specification and Design Assets

- `src/main/resources/api/american-airlines-info-api.raml` - Main API contract.
- `src/main/resources/api/examples/flight-example.raml` - Example payloads.
- `src/main/resources/api/examples/flightByIdexample.raml` - Example response by ID.
- `src/main/resources/api/exchange_modules/...` - Exchange module references used by RAML.

## 4) DataWeave Resources

- `src/main/resources/dwl/*.dwl` - Core functional transforms used by implementation flows.
- `src/main/resources/dwl/usecases/*.dwl` - Demo/Use-case transforms for DataWeave endpoints.

## 5) Environment and Security Configuration

- `src/main/resources/application.yaml` - Main Mule application includes/imports.
- `src/main/resources/config-dev.properties` - Non-sensitive dev configuration.
- `src/main/resources/config-dev-secure.properties` - Sensitive dev configuration (encrypted/secure values).
- `scripts/encrypt-secure-properties.ps1` - Helper for secure property encryption workflow.

## 6) Operations, Deployment, and Platform Automation

- `scripts/deploy-cloudhub2.ps1` - Build and deploy to CloudHub 2.0.
- `scripts/check-cloudhub2.ps1` - Deployment/runtime validation script.
- `scripts/publish-api-spec.ps1` - Exchange publication automation for API spec.
- `scripts/setup-api-manager.ps1` - API Manager automation bootstrap (policies/SLA seed path).

## 7) Testing and Validation

- `src/test/munit/american-airlines-info-api-suite.xml` - MUnit test suite.
- `src/test/resources/**` - MUnit fixtures and DataWeave assertions.
- `tests/local-api-regression.ps1` - Local regression test automation.
- `postman/american-airlines-info-api.postman_collection.json` - Local/Postman collection.
- `postman/american-airlines-info-api-cloudhub.postman_collection.json` - CloudHub Postman collection.
- `postman/american-airlines-info-api-playground.postman_collection.json` - Import-ready, purpose-driven collection with descriptive naming for all services.

## 8) Generated Reports

- `reports/summary.html` - Consolidated implementation summary.
- `reports/interface-report.html` - Interface/API-level report.
- `reports/implementation-report.html` - Implementation-level report.
- `reports/assets/**` - Report supporting static assets.

## 9) Published Documentation

- `exchange-docs/home.md` - Exchange-facing product document and architecture diagram.
- `docs/HLD_LLD.md` - High-level and low-level design.
- `docs/PROJECT_LIFECYCLE_AND_WORKFLOWS.md` - TDLC and workflow implementation narrative.
- `docs/RESOURCE_INDEX.md` - This file.
- `docs/PROMPT_EXECUTION_LEDGER.md` - Successful prompt ledger, timelines, commands, failures, fixes.
- `docs/CURSOR_TDLC_METRICS_FILLED.md` - Filled Cursor-specific TDLC metrics from attached matrix.
- `docs/EXECUTIVE_SCORECARD.md` - Executive one-page scorecard for leadership review.
- `docs/MASTER_DOCUMENTATION_latest.docx` - Latest lock-safe Word export of full documentation pack.

## 10) Exchange Publication and Documentation Operations

- API spec publish script:
  - `scripts/publish-api-spec.ps1`
- Exchange home-page content source:
  - `exchange-docs/home.md`
- Exchange home-page publish (API flow):
  - `PUT /exchange/api/v1/assets/{groupId}/{assetId}/{version}/draft/pages/home`
  - `PATCH /exchange/api/v1/assets/{groupId}/{assetId}/{version}`

## 11) Runtime-Only Local Investigation Artifacts

- `ch2-logs*/` - CloudHub log downloads used for troubleshooting and verification.
  - Ignored in git (`.gitignore`) to avoid noise and path-length issues.

## 12) Current Operational State Snapshot

- CloudHub deployments retained for this scope:
  - `american-airlines-info-app-dev` (actual app, active)
- Minimal/temporary validation deployment removed:
  - `american-airlines-minimal-api-dev` (deleted)
- Duplicate API instances removed; only active tracking instance retained for deferred APIM registration follow-up.
