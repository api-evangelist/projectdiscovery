---
name: Check a domain or email for credential leaks
description: Query the ProjectDiscovery Cloud Platform leak-monitoring API for exposure statistics on a domain or email, then manage leak status.
api: openapi/projectdiscovery-openapi-original.yml
operations:
- get-v1-leaks-stats-domain
- get-v1-leaks-stats-email
- post-v1-leaks-info
- post-v1-leaks-status
---

# Check a domain or email for credential leaks

Use the PDCP leaks API (`https://api.projectdiscovery.io`) for infostealer/dark-web
credential exposure.

## Auth
The two `stats` endpoints are **public** (no key required). Managing leak detail/status
requires `X-API-Key` and domain verification.

## Steps
1. **Domain exposure** — `GET /v1/leaks/stats/domain` (`get-v1-leaks-stats-domain`) for
   employee/customer leak counts, geographic distribution, and top compromised services.
2. **Email exposure** — `GET /v1/leaks/stats/email` (`get-v1-leaks-stats-email`) for a
   single address: leak classification, compromised services, risk timeline.
3. **Leak detail** — `POST /v1/leaks/info` (`post-v1-leaks-info`) for the underlying leak
   records (authenticated).
4. **Update status** — `POST /v1/leaks/status` (`post-v1-leaks-status`) to mark leaks
   fixed or reopen them (requires authentication + domain verification).

## Conventions
- Public stats endpoints still return the `{success, message}` envelope on error.
- Employee/customer leak management is gated on domain verification.
