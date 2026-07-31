---
name: Export a Mend SBOM report
description: Authenticate to the Mend AppSec Platform API, select an application, and request an SBOM report export.
api: openapi/mend-platform-openapi-original.json
operations: [login, getOrganizationApplications, getScanSummaries, exportImgSbomReport_2]
---

# Export a Mend SBOM report

Generate a Software Bill of Materials from Mend scan results via the AppSec Platform API 3.0.

## Steps

1. **Authenticate.** `POST /api/v3.0/login` (`login`) with email + user key; use the JWT as `Authorization: Bearer <jwt>`.
2. **Find the application.** `GET` `getOrganizationApplications` for your `orgUuid` to get the `applicationUuid`. Optionally `GET` `getScanSummaries` to confirm a recent scan exists.
3. **Request the SBOM export.** `POST` `exportImgSbomReport_2` for the `applicationUuid`. Report exports are asynchronous — the call returns a report process handle (`reportUuid`).
4. **Retrieve the result.** Poll the report status/download endpoint until the export completes, then fetch the SBOM (CycloneDX / SPDX).

## Rules

- Report generation is async; do not assume the SBOM is ready on the create response — poll for completion.
- The `Mend-SBOM-Export-CLI` (github.com/mend-toolkit/Mend-SBOM-Export-CLI) wraps this flow if you prefer a command-line path (see cli/mend-cli.yml).
- Bearer JWT is short-lived; refresh before long-running poll loops.
