---
name: Upload scan files to a Cintoo work zone
description: Create a project and work zone, obtain signed upload URLs, upload scan files, and register them.
api: openapi/cintoo-openapi-original.json
operations: [createProject, createWorkzone, getWorkzones, signUploadURL, registerFiles, getFiles]
---

# Upload scan files to a Cintoo work zone

The Cintoo ingestion flow uses signed URLs: you request upload URLs, PUT the file bytes to them, then register the uploaded files against the work zone.

## Auth
- `Authorization: Bearer <access_token>` on every call. Base URL `https://aec.cintoo.com/api/2/`. See `authentication/cintoo-authentication.yml`.

## Steps
1. `createProject` (accountRef) — create the project (or reuse an existing `projectRef` via `getProjects`).
2. `createWorkzone` (accountRef, projectRef) — create a work zone to hold the scans; or `getWorkzones` to reuse one.
3. `signUploadURL` (accountRef, projectRef, workzoneRef) — request signed URLs for the batch of files you intend to upload (`bulk-files`).
4. Upload each file's bytes directly to the returned signed URL (outside the API).
5. `registerFiles` (accountRef, projectRef, workzoneRef) — register the uploaded files so Cintoo begins processing them.
6. `getFiles` (accountRef, projectRef) — verify the files are present and track their state.

## Rules
- This is a two-phase (sign then register) upload; do not skip `registerFiles` or the bytes will not be ingested.
- Only project contributors may write to a work zone — otherwise `403 not-contributor-of-project`. See `errors/cintoo-problem-types.yml`.
- Sandbox tenants cap scans at 20 per user; test there first. See `sandbox/cintoo-sandbox.yml`.
