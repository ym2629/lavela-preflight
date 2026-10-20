---
name: data
description: "Read-only check of where an app keeps its data and whether it survives restarts, redeploys and mistakes: databases, local files, browser storage, dev versus production data, backups and rollback. Use before deploying."
allowed-tools: Read Grep Glob Bash(git log *) Bash(git ls-files *) Bash(git check-ignore *)
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Check 6: where does the data live, and can you get it back?

Goal: prevent "everything disappeared after I redeployed".

1. Find every place data is kept: database type and ORM, migration files, files written by the app (uploads, SQLite files, JSON files, `/tmp`), in-memory state (sessions, rankings, game progress), browser storage (localStorage, IndexedDB).
2. Ask where the app will run. If unknown, explain by case: many hosting services give the app a disposable disk, so files written by the app can vanish on restart or redeploy; browser-only storage disappears when the user clears the browser.
3. Development versus production: is there a way the same database could serve both? Look at config files and `.env.example` (never real `.env` values). Could test data end up in production?
4. Migrations and seeds: are they safe to run on a live database, and are any run automatically on start?
5. Backups and rollback: what would need to be backed up, how to restore it, and how to return to the previous version of the app if a release goes wrong.

Look at structure only (schema and migration files). Never open real data or connect to a database.

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
