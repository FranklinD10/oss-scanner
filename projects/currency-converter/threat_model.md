# Threat model: currency-converter

## What this project does and where untrusted input enters
A small React currency converter (USD/GHS/LRD) that fetches live rates from a
free public API. Untrusted input: the exchange-rate API responses, and the
amount/currency values the user types. All logic runs client-side; there is no
server component and no authentication.

## Components that matter most / least
- Most: `src/` (fetch logic, rate parsing, rendering of API-supplied numbers).
- Least: styling and static assets.

## How to exercise it
- `npm run build` (vite build) produces `dist/`.
- `npx eslint .` is the project's static check.

## How you rate severity
There is no secret, session, or server here, so the realistic ceiling is low:
DOM XSS via unsanitized API data rendered into the page is the main concern
(medium). Incorrect rate math is a correctness bug, not a security finding.

## Anything to leave alone
- Do not report the absence of authentication, CSRF, or rate limiting: there
  is no server or user account by design.
- `dist/` build output, if present, is generated; audit `src/` instead.
