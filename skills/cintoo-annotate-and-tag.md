---
name: Annotate and tag a Cintoo project
description: Create annotations with comments and organize scan data with tag lists and tags.
api: openapi/cintoo-openapi-original.json
operations: [getAnnotationsFromProject, createAnnotation, createAnnotationComment, getAnnotationById, getTagsOnProject, createTagList, bulkUpdateTagLists]
---

# Annotate and tag a Cintoo project

Add review annotations to a reality-capture project and structure its data with tag lists.

## Auth
- `Authorization: Bearer <access_token>`. Base URL `https://aec.cintoo.com/api/2/`. See `authentication/cintoo-authentication.yml`.

## Annotations
1. `getAnnotationsFromProject` (accountRef, projectRef) — list existing annotations.
2. `createAnnotation` (accountRef, projectRef) — create a new annotation on the scene.
3. `createAnnotationComment` (accountRef, projectRef, annotationRef) — thread a comment on the annotation.
4. `getAnnotationById` (accountRef, projectRef, annotationRef) — read back one annotation.

## Tags
5. `getTagsOnProject` (accountRef, projectRef) — list the project's tag lists.
6. `createTagList` (accountRef, projectRef) — create a tag list.
7. `bulkUpdateTagLists` (accountRef, projectRef) — apply tag-list changes in bulk.

## Rules
- Writes require project-contributor permissions — otherwise `403`. Missing refs return `404`. See `errors/cintoo-problem-types.yml`.
- Tags belong to tag lists (see the data model in `data-model/cintoo-data-model.yml`); create the list before adding tags.
