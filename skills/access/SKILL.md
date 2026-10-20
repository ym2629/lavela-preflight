---
name: access
description: Read-only check of who can see or change data: public pages and endpoints, missing ownership checks, admin features hidden only in the UI, and database access rules (Supabase RLS, Firebase rules). Use before deploying.
allowed-tools: Read Grep Glob Bash(git log *) Bash(git ls-files *) Bash(git check-ignore *)
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Check 2: who can see or change what

Goal: find places where a stranger, or a logged-in user, can reach data or actions that are not theirs.

1. Identify the framework and list routes and API endpoints (for example Next.js `app/` and `pages/api`, Express routes, FastAPI/Flask/Django views, Rails routes).
2. For each endpoint or page, note whether it requires login. Produce a table of everything reachable WITHOUT login, and flag any that return personal data or another user's data.
3. Ownership checks: where an endpoint takes an id from the URL or request and returns or changes a record, check whether the code verifies that the record belongs to the current user. Changing a number in the address must never reveal someone else's data.
4. Privileged actions (edit, delete, pay, admin): confirm they are enforced on the server or in database rules, not only by hiding a button in the page.
5. Values the browser sends that the server should not trust: prices, roles, user ids, scores, discounts.
6. Backend-as-a-service: for Supabase look for row level security in migrations/SQL (`enable row level security`, policies), tables without it, and any use of the `service_role` key in client code. For Firebase read `firestore.rules` and `storage.rules` and flag `allow read, write: if true` style rules. If the rules live only in a dashboard, say you cannot verify them and say where to look.
7. Also look at: CORS set to `*` together with credentials, file uploads without type or size limits, public storage buckets.

Work from the code only. Do NOT send any request to a running server. Split every finding into "verified in code" and "cannot verify from code".

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
