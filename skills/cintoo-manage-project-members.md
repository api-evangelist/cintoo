---
name: Manage Cintoo project members and roles
description: List, add, and remove users and groups on a Cintoo project, and manage account roles.
api: openapi/cintoo-openapi-original.json
operations: [getProjectMembers, addProjectMembers, removeMembersFromProject, removeUserFromProject, removeGroupFromProject, getRoles, updateUserAccountRoles]
---

# Manage Cintoo project members and roles

Control who can access a Cintoo project and with what role.

## Auth
- `Authorization: Bearer <access_token>`. Base URL `https://aec.cintoo.com/api/2/`. See `authentication/cintoo-authentication.yml`.

## Steps
1. `getRoles` (accountRef) — list the roles available in the account so you can assign the right one.
2. `getProjectMembers` (accountRef, projectRef) — see current members (users and groups).
3. `addProjectMembers` (accountRef, projectRef) — add users/groups to the project.
4. `removeMembersFromProject` (accountRef, projectRef) — bulk-remove members; or target one with `removeUserFromProject` / `removeGroupFromProject`.
5. `updateUserAccountRoles` (accountRef, userRef) — adjust a user's account-level roles.

## Rules
- Membership and role changes require the corresponding permission; lacking it returns `403 <action>-<resource>-forbidden` with `errorValues.requiredPermissions`. See `errors/cintoo-problem-types.yml`.
- References are URNs; a missing user/project returns `404 <type>-not-found`.
- Prefer the bulk endpoints (`bulk-invite`, `bulk-remove`) when acting on multiple members.
