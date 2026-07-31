---
name: Monitor Mend zero-day events
description: Authenticate to the Mend AppSec Platform API and list active zero-day events and the findings they affect.
api: openapi/mend-platform-openapi-original.json
operations: [login, getZeroDayEvents, getZeroDayEventFindings]
---

# Monitor Mend zero-day events

Track newly-disclosed zero-day events affecting your organization's inventory via the AppSec Platform API 3.0.

## Steps

1. **Authenticate.** `POST /api/v3.0/login` (`login`) with email + user key; use the JWT as `Authorization: Bearer <jwt>`.
2. **List zero-day events.** `GET` `getZeroDayEvents` for your `orgUuid`. Page with `cursor` + `limit`. Each event carries a `zeroDayIdentifier`.
3. **Inspect affected findings.** `GET` `getZeroDayEventFindings` for a `zeroDayIdentifier` to see which projects/libraries are impacted, then hand off to the triage skill.

## Rules

- Bearer JWT auth; handle 401 by refreshing the token.
- Poll on a cadence appropriate to your risk posture; there is no webhook/event push documented for this surface.
- Errors carry a `supportToken` in the DWR-style envelope.
