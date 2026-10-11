# Threat model: CodeFusion

## What this project does and where untrusted input enters
A client-side web tool that merges uploaded code files into one downloadable
text file. Untrusted input: the files a user uploads (read via the File API
and rendered back into the page / offered as a download). Everything runs in
the browser; there is no server.

## Components that matter most / least
- Most: `script.js` (file reading, content rendering, download generation),
  `index.html` (where uploaded content is injected into the DOM).
- Least: `styles.css`, `img/`, `icon.svg`.

## How to exercise it
- No build step; open `index.html` in a browser. `service-worker.js` and
  `manifest.json` make it installable as a PWA.

## How you rate severity
DOM XSS via uploaded file content rendered as HTML is the main concern
(high if attacker-controlled content executes in the victim's session).
Otherwise the ceiling is low: no server, no secrets, no accounts.

## Anything to leave alone
- Do not report the absence of a backend, authentication, or CSP headers as
  findings; the tool is intentionally fully client-side.
