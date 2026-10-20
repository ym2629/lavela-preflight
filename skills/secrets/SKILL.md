---
name: secrets
description: Read-only check for API keys, passwords and tokens hard-coded in a project or left in git history, and for secrets exposed to the browser. Use before deploying. Never prints secret values.
allowed-tools: Read Grep Glob Bash(git log *) Bash(git ls-files *) Bash(git check-ignore *)
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Check 1: secrets in code and history

Goal: find credentials that would let a stranger use or damage the owner's accounts.

1. Identify the project type and whether it is a git repository.
2. Find files that usually hold secrets BY NAME with Glob (`.env*`, `*.pem`, `*.key`, `credentials*.json`, `serviceAccount*.json`, `id_rsa*`). Do not open them. For each, say whether git tracks it (`git ls-files`) or ignores it (`git check-ignore`).
3. Search source and config files with Grep (`files_with_matches`) for common secret shapes: `sk_live_`, `sk_test_`, `sk-`, `AKIA`, `ghp_`, `gho_`, `github_pat_`, `xox[bp]-`, `AIza`, `-----BEGIN .* PRIVATE KEY-----`, `service_role`, connection strings with a password (`postgres://user:pass@`, `mongodb+srv://user:pass@`), long `eyJ...` tokens.
4. Classify what you find. Public by design (not a leak): Stripe `pk_` publishable keys, Supabase anon keys, Firebase web `apiKey`. Must stay secret: Stripe `sk_` and webhook secrets, Supabase `service_role`, database URLs, AI provider keys (OpenAI, Anthropic, Google), JWT/session secrets, private keys.
5. Browser exposure: variables with a public prefix (`NEXT_PUBLIC_`, `VITE_`, `REACT_APP_`, `PUBLIC_`, `EXPO_PUBLIC_`) whose names or usage suggest a secret, and secrets used inside code that runs in the browser. Anything shipped to the browser is public even if it was never committed. Because you must not open `.env` files, you cannot see variable names defined only there. Never report "no public-prefix variables" as clean in that case. Instead tell the user they can safely list the names (no values) themselves with `cut -d= -f1 .env` and check whether any secret-looking name starts with a public prefix, and put this under "Not verified".
6. Git history: if this is a git repo, use `git log --all --oneline -G "<pattern>"` (commit hashes and messages only, never diffs) to see whether a secret pattern or an `.env` file was ever committed. If git is not used, say so.
7. If a real secret was ever committed or shipped to the browser: explain that deleting it from the file is not enough, the key must be revoked and re-issued, and ask whether the repository has ever been public.

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
