---
name: Authenticate and list Cintoo projects
description: Acquire a JWT access token, resolve the account, and list projects in a Cintoo Cloud account.
api: openapi/cintoo-openapi-original.json
operations: [getAccounts, getAccount, getProjects, getProject]
---

# Authenticate and list Cintoo projects

Use this to get oriented in a Cintoo Cloud tenant: authenticate, find the account, then enumerate its projects.

## Auth
- Obtain the initial JWT Access Token + Refresh Token pair once (via the `cintoo-login` CLI or the OAuth authorization-code flow at `https://aec.cintoo.com/oauth/authorize`). See `authentication/cintoo-authentication.yml`.
- Send `Authorization: Bearer <access_token>` on every call. Access tokens live ~3h; refresh before expiry (a used/expired token returns `401 unauthorized`).
- Base URL: `https://aec.cintoo.com/api/2/` (or `https://eu.cintoo.cloud/api/2/` for the EU region).

## Steps
1. `getAccounts` — list the accounts the authenticated user can access; pick the target `accountRef`.
2. `getAccount` (accountRef) — confirm the account details/region.
3. `getProjects` (accountRef) — list projects in the account.
4. `getProject` (accountRef, projectRef) — fetch a single project when you have its ref.

## Rules
- References are URNs (accountRef, projectRef). A malformed ref returns `400 invalid-<type>-urn`; a valid-but-missing ref returns `404 <type>-not-found`. See `errors/cintoo-problem-types.yml`.
- Permission errors surface as `403` with `errorValues.requiredPermissions`.
- No pagination parameters are defined — responses return the full collection.
