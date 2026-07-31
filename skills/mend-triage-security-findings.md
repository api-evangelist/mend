---
name: Triage Mend security findings
description: Authenticate to the Mend AppSec Platform API, list a project's open security vulnerability findings, and bulk-update their status.
api: openapi/mend-platform-openapi-original.json
operations: [login, getOrganizationProjects, getSecurityVulnerabilityFindings, listProjectFindingsV3.0, bulkPatchProjectFindingV3.0]
---

# Triage Mend security findings

Use the Mend AppSec Platform API 3.0 (`https://api-saas.mend.io`, region-specific host) to review and action open vulnerability findings.

## Steps

1. **Authenticate.** `POST /api/v3.0/login` (`login`) with your email and user key (from your Mend Platform profile page). Store the returned JWT as `Authorization: Bearer <jwt>`. The token is short-lived — refresh with `POST /api/v3.0/login/accessToken` (`refreshAccessToken`) rather than logging in repeatedly.
2. **List projects.** `GET` `getOrganizationProjects` for your `orgUuid`. Page with the `cursor` + `limit` parameters. Pick the `projectUuid` to triage.
3. **List findings.** `GET` `getSecurityVulnerabilityFindings` (or `listProjectFindingsV3.0`) for the `projectUuid` to retrieve open security vulnerability findings.
4. **Action findings.** `PATCH` `bulkPatchProjectFindingV3.0` to bulk-update finding status (e.g. mark ignored/resolved with justification).

## Rules

- Auth is Bearer JWT; a 401 means the token expired — re-authenticate or refresh.
- A 403 means the principal lacks permission for that org/project scope.
- Errors return a DWR-style envelope with a `supportToken`; quote it when contacting Mend support (see errors/mend-problem-types.yml).
- No idempotency-key mechanism exists — do not blind-retry write operations; re-fetch to confirm state.
