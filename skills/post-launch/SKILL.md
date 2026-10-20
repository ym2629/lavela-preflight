---
name: post-launch
description: "Builds a project-specific checklist to follow right after going live: open the site as a stranger, test sign-up and key flows with a fresh account, mobile view, emails, errors, monitoring and rollback. Use after deploying."
allowed-tools: Read Grep Glob
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Check 7: the first 30 minutes after launch

Goal: produce a short checklist the owner can tick through personally, tailored to this project.

1. Read the project to learn its main flows: sign-up and login method, key pages, payments (if any), emails it sends, background jobs.
2. Write a numbered checklist in plain language covering: the address opens over https (and the www / non-www version); open it in a private window and on a phone; sign up with a brand-new account (not the developer's logged-in one); password-reset or confirmation email arrives and is not in spam; the main action of the app works end to end; the payment flow with a test card if payments exist; what visitors see on an error page; where to look at logs; the cost and error alerts are switched on.
3. Add "if something is wrong": how to roll back to the previous version and who or what to check first.
4. Keep it to one screen of text so it can be pasted into notes.

This skill produces a checklist only; it does not test the live site.

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
