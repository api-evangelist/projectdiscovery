---
name: Discover an attack surface via asset enumeration
description: Enumerate an organization's external assets on the ProjectDiscovery Cloud Platform and export the discovered inventory.
api: openapi/projectdiscovery-openapi-original.yml
operations:
- post-v1-asset-enumerate
- get-v1-asset-enumerate-enumerate_id
- get-v1-asset-enumerate-enumerate_id-contents
- get-enumerate-id-export
- associated-domains
---

# Discover an attack surface via asset enumeration

Map external assets with the PDCP enumeration API (`https://api.projectdiscovery.io`).

## Auth
`X-API-Key: <your key>` header, HTTPS only.

## Steps
1. **Start enumeration** — `POST /v1/asset/enumerate` (`post-v1-asset-enumerate`) with the
   seed domain(s). Capture the `enumerate_id`.
2. **Poll status** — `GET /v1/asset/enumerate/{enumerate_id}`
   (`get-v1-asset-enumerate-enumerate_id`) until discovery completes.
3. **Read the inventory** — `GET /v1/asset/enumerate/{enumerate_id}/contents`
   (`get-v1-asset-enumerate-enumerate_id-contents`), paged with `limit`/`offset`.
4. **Export** — `GET /v1/asset/enumerate/{enum_id}/export` (`get-enumerate-id-export`) to
   pull the full discovered asset list for downstream use.
5. **Expand scope (optional)** — use the associated-domains endpoint to discover related
   domains via cert transparency / WHOIS before enumerating those too.

## Conventions
- Enumerations are asynchronous — always poll before reading contents.
- Same `{success, message}` JSON error envelope; page with `limit`/`offset`/`page`.
