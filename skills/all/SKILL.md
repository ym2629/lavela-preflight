---
name: all
description: Runs the seven read-only pre-deployment checks in order (secrets, access, server-ready, errors, cost, data, post-launch) and ends with the five most important things to fix. Optional payments and privacy check if the app needs it.
allowed-tools: Read Grep Glob Bash(git log *) Bash(git ls-files *) Bash(git check-ignore *)
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Full pre-deployment review

Run the checks below in this order. Each one is a skill in this plugin; follow the method and the ground rules of each:

1. `lavela-preflight:secrets`
2. `lavela-preflight:access`
3. `lavela-preflight:server-ready`
4. `lavela-preflight:errors`
5. `lavela-preflight:cost`
6. `lavela-preflight:data`
7. `lavela-preflight:post-launch`

If the project takes payments or collects personal data (email, name, phone), tell the user and offer `lavela-preflight:payments-privacy`; run it only if they say yes.

## Ground rules (read-only)

- This check is READ-ONLY. Do not create, edit, delete or move files. Do not run installs, builds, migrations, seed scripts, deploys, or any command that changes state or sends data over the network.
- Never print secret values (API keys, passwords, tokens, private keys, connection strings). Report only the file path and the kind of secret. Do not open `.env`-style files; check for them by name only.
- Use only reading tools (Read, Grep, Glob) and the few read-only git commands this skill allows. When searching for secret patterns, use Grep in `files_with_matches` mode only, never in content mode.
- This is an AI-assisted review, not a security audit. For each finding say whether you verified it in the code or only inferred it, and list what you could NOT check.
- Reply in the language the user writes in. Keep explanations short and non-technical; explain any jargon in a few words.

## Final summary

After the checks, give:

1. The five most important things to fix before launch, ranked, one line each, with the check they came from.
2. Everything that could not be verified from the code and needs a person or a dashboard.
3. A reminder that this is an AI-assisted review, not a security audit, and that important apps deserve a human review.

Do not fix anything unless the user asks afterwards.
