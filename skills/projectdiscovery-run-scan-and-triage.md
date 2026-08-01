---
name: Run a cloud scan and triage vulnerabilities
description: Trigger a nuclei scan on the ProjectDiscovery Cloud Platform, poll it to completion, and pull the vulnerabilities it found for triage.
api: openapi/projectdiscovery-openapi-original.yml
operations:
- post-v1-scans
- get-v1-scans-scan_id
- get-v1-results
- get-v1-scans-vuln-:vulnId
- patch-v1-scans-vulns
- post-v1-scans-vuln_id-retest
---

# Run a cloud scan and triage vulnerabilities

Use the PDCP REST API (`https://api.projectdiscovery.io`) to run a scan and work its results.

## Auth
Send `X-API-Key: <your key>` on every request over HTTPS. Create/rotate the key at
`https://cloud.projectdiscovery.io/settings/api-key`.

## Steps
1. **Start the scan** — `POST /v1/scans` (`post-v1-scans`) with the targets and a scan
   configuration id. Capture the returned `scan_id`.
2. **Poll status** — `GET /v1/scans/{scan_id}` (`get-v1-scans-scan_id`) until the scan
   status is finished. Back off between polls.
3. **List findings** — `GET /v1/scans/results` (`get-v1-results`), paging with
   `limit`/`offset`, to enumerate the vulnerabilities produced.
4. **Inspect a finding** — `GET /v1/scans/vuln/{vuln_id}` (`get-v1-scans-vuln-:vulnId`)
   for the full detail of any vulnerability of interest.
5. **Update status in bulk** — `PATCH /v1/scans/vulns` (`patch-v1-scans-vulns`) to mark
   findings (e.g. fixed / false-positive) after triage.
6. **Retest a fix** — `POST /v1/scans/{vuln_id}/retest` (`post-v1-scans-vuln_id-retest`)
   to re-verify a specific vulnerability.

## Conventions
- Pagination: `limit` / `offset` / `page`.
- Errors return `application/json` `{success: false, message: "..."}` — read `message`
  on any 4xx (see `errors/projectdiscovery-problem-types.yml`).
- No idempotency key: do not blindly retry `POST /v1/scans`; check existing scans first.
