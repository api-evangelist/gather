---
name: Manage Gather email guestlist
description: Read and update the email guestlist (members/guests and roles) of a Gather space.
api: openapi/gather-http-api-openapi.yml
operations: [getEmailGuestlist, setEmailGuestlist]
---

# Manage Gather email guestlist

Control who may enter a Gather space and with what role, by email.

## Auth
- API key in the `apiKey` header; the user must have Admin/Builder permission on the space.
- Base URL: `https://api.gather.town`.

## Steps
1. Read the current list with `getEmailGuestlist` — `GET /api/getEmailGuestlist?spaceId=<id>`. The result is an object keyed by email, each value `{ name, affiliation, role }`.
2. Update with `setEmailGuestlist` — `POST /api/setEmailGuestlist` with `{ "spaceId": "<id>", "guestlist": { "person@example.com": { "name": "...", "role": "..." } }, "overwrite": false }`.
   - `overwrite: false` merges/appends entries.
   - `overwrite: true` **replaces** the entire guestlist — read first, then set, to avoid dropping existing members.

## Rules
- `400` usually means a malformed `guestlist` object; `403` means the key lacks permission; `404` means a bad `spaceId` or endpoint path.
- These are the legacy `/api/*` (non-v2) endpoints — do not prefix with `/api/v2`.
