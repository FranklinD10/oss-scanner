# Threat model: SchoolGo

## What this project does and where untrusted input enters
A Flask web chatbot for school/university topics, backed by the OpenAI API.
Untrusted input: the chat text a visitor submits (forwarded into an OpenAI
prompt), plus any query/form parameters on the Flask routes. The OpenAI API
key is server-side; the browser never sees it.

## Components that matter most / least
- Most: `app.py` (all routes, prompt construction, API key handling),
  `config.py` (secret loading via environment).
- Least: `static/` and `templates/` presentation assets, `environment.yml`
  (conda mirror of requirements).

## How to exercise it
- `pip install -r requirements.txt` (Python 3.10; numpy 1.22 pins predate 3.11).
- `python -m compileall app.py config.py` for a syntax smoke check.
- Running needs `OPENAI_API_KEY` in the environment; static analysis does not.

## How you rate severity
API key exfiltration to the client, server-side request forgery, or prompt
injection that leaks the system prompt / key material is high. Reflected XSS
in chat output is high. Missing CSRF on the chat form is medium.

## Anything to leave alone
- Do not report that the OpenAI dependency is old (0.19.0) or that
  text-davinci-003 is deprecated; that is a known maintenance backlog, not a
  vulnerability finding.
- `__pycache__/` bytecode, if present, is generated; audit the `.py` sources.
