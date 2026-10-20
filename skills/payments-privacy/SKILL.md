---
name: payments-privacy
description: Read-only check for apps that take payments or collect personal data: test versus live mode (by key type only), server-side price calculation, webhook verification, where personal data is stored, and which policy pages exist. Not legal advice.
allowed-tools: Read Grep Glob Bash(git log *) Bash(git ls-files *) Bash(git check-ignore *)
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Optional check 8: payments and personal data

Run this only if the app takes payments or collects personal data (email, name, phone, address). Not legal advice.

1. Payments: identify the provider. Determine test versus live mode ONLY from the key type prefix in code or config (for example `sk_test_` versus `sk_live_`); never print the key and never open `.env` files.
2. Is the amount charged calculated on the server, or taken from what the browser sends? Is card data entered on the provider's hosted page or fields, never on the app's own server?
3. Webhooks: is the provider's signature verified? Are duplicate events handled (idempotency)? Are the webhook secret and endpoint different for test and live?
4. Personal data: list what is collected, where it is stored, and which third parties receive it (analytics, email, AI providers).
5. Pages: do privacy-policy and terms pages exist and are they linked from sign-up? Is there a way for a user to delete their account or data?
6. State clearly that you cannot judge legal requirements for any country. Tell the user to check the rules that apply to where their users live. Do NOT write policy documents.

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
