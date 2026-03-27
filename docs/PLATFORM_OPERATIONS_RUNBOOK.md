# Platform Operations Runbook (Assets, Design Center, API Manager, Contracts)

This runbook captures the exact operational steps used for Exchange/Design Center/API Manager operations, including what was completed and what remains deferred.

## 1) Design Center Sync Steps

### Goal
Sync RAML sources to Design Center project.

### Steps Executed

1. Create project:
   - `anypoint-cli-v4 designcenter project create "american-airlines-info-api" --type raml --client_id <connectedAppId> --client_secret <connectedAppSecret> --organization "Accenture" --environment "Sandbox"`
2. Upload RAML folder:
   - `anypoint-cli-v4 designcenter project upload "american-airlines-info-api" "src/main/resources/api" --client_id <connectedAppId> --client_secret <connectedAppSecret> --organization "Accenture" --environment "Sandbox"`

### Outcome
- Project created and uploaded successfully.

## 2) Exchange Asset Publication Steps

### Goal
Publish API specification as Exchange `rest-api` asset.

### Steps Executed

1. Initial publish attempt for existing version (`1.0.0`) returned conflict (`409`) because lifecycle state was already published.
2. Published next version (`1.0.1`) using:
   - `./scripts/publish-api-spec.ps1 -ConnectedAppClientId <id> -ConnectedAppClientSecret <secret> -OrgId <orgId> -GroupId <orgId> -AssetId "american-airlines-info-api" -AssetVersion "1.0.1"`

### Outcome
- Exchange asset version `1.0.1` published.

## 3) Exchange Home-Page Documentation Steps

### Goal
Ensure documentation is visible on Exchange home page.

### Steps Executed

1. Update draft home page:
   - `PUT /exchange/api/v1/assets/{groupId}/{assetId}/{version}/draft/pages/home`
2. Publish documentation state:
   - `PATCH /exchange/api/v1/assets/{groupId}/{assetId}/{version}` with `{"status":"published"}`

### Outcome
- Exchange home page now contains architecture + workflow + error-handling + lifecycle diagrams.

## 4) API Manager Instance and Policy Steps

### Goal
Create/manage API instances, policies, and contract prerequisites.

### Steps Executed

- Used `scripts/setup-api-manager.ps1` and direct APIM API operations for:
  - API instance creation attempts
  - policy setup attempts (basic auth / client-id patterns)
  - SLA tier creation
  - contract revoke and duplicate cleanup

### State Summary

- Duplicate/obsolete API instances were removed.
- Minimal validation instance and related deployment were cleaned up.
- Single target instance retained for actual app follow-up (`20817134`).
- APIM registration pairing is intentionally deferred as agreed.

## 5) Client App and Contracts

### What was done

- Existing contracts on obsolete instances were revoked before deletion.
- This prevented stale instance buildup.

### What remains (deferred by scope)

- Create new client apps per SLA tier against final stabilized API instance.
- Generate and distribute final `client_id` / `client_secret` for consumers.
- Validate contract visibility and policy enforcement post-registration.

## 6) Asset/Type Governance Pain Point (Important)

- Exchange enforces asset type boundaries (`rest-api` vs app deployable asset).
- A `rest-api` identity cannot always be reused as deployable app artifact type.
- This was a key root cause in platform alignment friction.

## 7) Recommended Finalization Checklist

1. Stabilize APIM registration for final instance.
2. Create client applications (Bronze/Silver) and approve contracts.
3. Run policy-enforced Postman suite with issued client credentials.
4. Record final credentials handoff and API governance state.
