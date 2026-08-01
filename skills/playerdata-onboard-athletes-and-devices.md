---
name: Onboard PlayerData athletes and assign devices
description: Add athletes to a club and assign wearable tracking devices via the PlayerData GraphQL API.
api: graphql/playerdata-schema.graphql
operations: [club, addNewPersonToClub, addExistingPersonToClub, addAthleteGroup, assignDevice, assignEdge]
---

# Onboard PlayerData athletes and assign devices

Set up a club roster and wearable hardware using the PlayerData GraphQL API
(`https://app.playerdata.co.uk/api/graphql`).

## Authenticate
1. Use OAuth 2.0 (see `authentication/playerdata-authentication.yml`). Client
   credentials for org-level automation; authorization code for staff-scoped access.
2. Send `Authorization: Bearer <access_token>` on every request.

## Steps
1. Confirm the target club with the `club` query.
2. Create athlete groups with `addAthleteGroup` if you need to organise the roster.
3. Add athletes:
   - New people: `addNewPersonToClub`.
   - Existing people: `addExistingPersonToClub`.
4. Assign wearable hardware to athletes:
   - `assignDevice` to link a tracking device.
   - `assignEdge` to link an EDGE tracker unit (or `assignDefaultEdges` in bulk).

## Conventions
- All writes are GraphQL mutations; there is no idempotency-key contract, so avoid
  blind retries — re-query to confirm state before re-issuing a mutation.
- Bulk imports (`createBulkAthleteImport`) emit progress via the
  `bulkAthleteImportEvent` GraphQL subscription (see `asyncapi/`).
