---
name: errors
description: Read-only check of whether the owner would find out when something breaks after launch: error handling, logging, health checks and alerts. Use before deploying.
allowed-tools: Read Grep Glob Bash(git log *) Bash(git ls-files *) Bash(git check-ignore *)
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Check 4: will you know when it breaks?

Goal: make sure errors after launch are visible to the owner, not only to visitors.

1. Find error handling: a global error handler, API error responses, empty `catch` blocks that swallow errors, places that log nothing.
2. Logging: is there a real logger or only `console.log`/`print`? Does anything log personal data or secrets (report where, never the values)?
3. Error tracking: is an error-tracking service wired in (Sentry or similar)? If not, say so.
4. Health and uptime: is there a simple health endpoint a monitor could call?
5. Where logs live depends on the host. Ask where the app will run; if unknown, explain the main cases briefly: serverless platforms (logs in the provider dashboard, often short retention), containers and servers (standard output), static hosting (no server logs).
6. Alerts: describe the simplest way the owner could be told when the site is down or erroring (a free uptime monitor on the health endpoint, an error-tracking free tier, email alerts from the host). Present these as options, not as a product recommendation.

Suggest methods only. Do not add code or install anything.

## Ground rules (read-only)

- This check is READ-ONLY. Do not create, edit, delete or move files. Do not run installs, builds, migrations, seed scripts, deploys, or any command that changes state or sends data over the network.
- Never print secret values (API keys, passwords, tokens, private keys, connection strings). Report only the file path and the kind of secret. Do not open `.env`-style files; check for them by name only.
- Use only reading tools (Read, Grep, Glob) and the few read-only git commands this skill allows. When searching for secret patterns, use Grep in `files_with_matches` mode only, never in content mode.
- This is an AI-assisted review, not a security audit. For each finding say whether you verified it in the code or only inferred it, and list what you could NOT check.
- Reply in the language the user writes in. Keep explanations short and non-technical; explain any jargon in a few words.

## Output

1. One-line verdict: "OK to launch", "Fix first", or "Could not tell".
2. A table: Finding | Where (file path) | Risk (High / Medium / Low) | What to do.
3. "Not verified": what needs a human or a dashboard check.

Do not fix anything. Offer to help fix only after the user asks.
