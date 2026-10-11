# Threat model: ellin-goals

## What this project does and where untrusted input enters
A personal goals-tracking web app (React + TypeScript + Vite, Firebase backend).
Untrusted input: anything a signed-in user types into goal/task forms, plus
data read back from Firestore. The app is single-user oriented; there is no
multi-tenant trust boundary beyond the Firebase security rules.

## Components that matter most / least
- Most: `src/lib` (Firebase/Auth/Firestore wiring), `src/contexts` (auth state),
  `src/pages` and `src/components` (form handling, rendering user content).
- Least: styling (`global.css`), static assets. `package-lock.json` is vendored
  dependency metadata, out of scope for logic bugs.

## How to exercise it
- `npm run build` (tsc + vite build) produces `dist/`.
- `npx vitest run` runs the unit tests (see `vitest.config.ts`).
- The app needs Firebase project config at runtime; static analysis does not.

## How you rate severity
Anything enabling account takeover, Firestore data exfiltration across users,
or XSS that executes in another user's session is high+. Broken auth checks
are high. UI-only defects are low.

## Anything to leave alone
- Do not report missing rate-limiting on Firebase client calls (handled by
  Firebase rules/quotas, not app code).
- `dist/` build output, if present, is generated; audit `src/` instead.
