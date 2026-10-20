---
name: cost
description: "Read-only check for places where usage can make the bill grow unexpectedly: unauthenticated AI calls, missing rate limits and size limits, retries without a cap, and provider spending limits. Use before deploying."
allowed-tools: Read Grep Glob Bash(git log *) Bash(git ls-files *) Bash(git check-ignore *)
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Check 5: what could make the bill explode?

Goal: find the cost an abuser, a bug, or sudden popularity could create.

1. Find every metered or paid action: calls to AI APIs, heavy database queries, file storage and uploads, email or SMS sending, third-party APIs billed per call, scheduled jobs.
2. For each, check: does it require login? Is there a per-user or per-IP limit? Is input length or file size capped? Can a failure trigger endless retries or loops?
3. Rank the worst cases by "cost per month if abused". Pay special attention to AI endpoints reachable without login, because they let strangers spend the owner's money.
4. Limits in the page do not count. A limit must be enforced on the server. In serverless or multi-instance setups an in-memory counter does not work; say so and suggest a shared store or provider-level limits.
5. Outside the code: tell the user where to set monthly spending limits and billing alerts at each provider the project uses (AI provider consoles, hosting, database, email). Say that you cannot see their accounts and they must check.

Suggest fixes in words only; do not change code.

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
