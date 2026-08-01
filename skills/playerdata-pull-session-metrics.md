---
name: Pull PlayerData session performance metrics
description: Authenticate to the PlayerData GraphQL API and retrieve a club's sessions and per-athlete performance metrics.
api: graphql/playerdata-schema.graphql
operations: [club, sessions, session, sessionParticipations, athlete]
---

# Pull PlayerData session performance metrics

Use the PlayerData GraphQL API (`https://app.playerdata.co.uk/api/graphql`) to read a
club's training/match sessions and the performance metrics captured for each athlete.

## Authenticate
1. Obtain OAuth 2.0 credentials from PlayerData (support@playerdata.com).
2. For server-to-server use, run the **client credentials** grant against
   `https://app.playerdata.co.uk/oauth/token` (`grant_type=client_credentials`,
   `client_id`, `client_secret`). For user-based access use the **authorization code**
   grant (`https://app.playerdata.co.uk/oauth/authorize` → token endpoint).
3. Send `Authorization: Bearer <access_token>` on every GraphQL request. Access
   tokens expire after 2 hours (7200s) — refresh before expiry.

## Steps
1. Resolve the club with the `club` query to confirm access (Authorisation Code
   access is scoped to clubs where the user is listed as staff).
2. List sessions with the `sessions` query. Page with Relay cursor pagination
   (`pageInfo.endCursor` / `pageInfo.hasNextPage`).
3. For a chosen session, fetch detail with the `session` query.
4. Read per-athlete metrics via `sessionParticipations`.
5. Resolve athlete details with the `athlete` query as needed.

## Conventions
- GraphQL over HTTPS POST; errors arrive in the top-level `errors` array (not RFC 9457).
- List fields use Relay Connections — never assume offset paging.
- Prefer the official `playerdatapy` (Python) or `playerdatar` (R) client libraries.
