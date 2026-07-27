---
name: Manage Gather space maps
description: Read and update the map/room data of a Gather space via the HTTP API.
api: openapi/gather-http-api-openapi.yml
operations: [createSpace, getMap, setMap]
---

# Manage Gather space maps

Use the Gather HTTP API to programmatically build and edit the rooms of a Gather space.

## Auth
- Generate an API key at https://gather.town/apiKeys.
- Send it in the `apiKey` request header on every call.
- The key's user must have **Admin** or **Builder** permission on the target space.
- Base URL: `https://api.gather.town`.

## Steps
1. (Optional) Create a space with `createSpace` — `POST /api/v2/spaces` with `{ "name": "...", "sourceSpace": "<clone-from-id>" }`. The response returns the new space id.
2. Read the current room with `getMap` — `GET /api/v2/spaces/{spaceId}/maps/{mapId}?useV2Map=true`. URL-encode `spaceId` (it contains a delimiter). Inspect `backgroundImagePath`, `spawns`, `objects`, `collisions`.
3. Write changes with `setMap` — `POST /api/v2/spaces/{spaceId}/maps/{mapId}?useV2Map=true` with `{ "content": { ...map... } }`. `setMap` **replaces** the map content, so read-modify-write: start from the `getMap` result rather than sending a partial map.

## Rules
- Errors are bare HTTP status codes: `400` malformed body, `403` insufficient permission, `404` unknown space/map (see `errors/gather-problem-types.yml`).
- No idempotency-key mechanism exists; `setMap` is a replace, so retries converge.
- There is no list endpoint — you must already know the `spaceId`/`mapId`.
