---
name: server-ready
description: Read-only check of what breaks when an app moves from the developer machine to a server: build and start commands, versions, port, required environment variable names, localhost addresses, debug leftovers, and file-name capitalization. Use before deploying.
allowed-tools: Read Grep Glob Bash(git log *) Bash(git ls-files *) Bash(git check-ignore *)
disallowed-tools: Edit Write NotebookEdit
disable-model-invocation: true
---

# Check 3: will it work on a server?

Goal: list everything that works on the developer's computer but commonly breaks online.

1. Build and start: infer the install, build and start commands and the runtime version (package.json scripts and engines, .nvmrc, Dockerfile, Procfile, pyproject, runtime.txt). Do NOT run them; only report them.
2. Port and host: does the app read the port from the environment, and listen on all interfaces rather than only localhost?
3. Environment variables: list every variable name the code reads (`process.env.X`, `import.meta.env.X`, `os.environ`), what it is for, and whether it is needed at build time, at run time, or in the browser. Names and purposes only, never values. Is there an example file such as `.env.example`?
4. Addresses to change after launch: hard-coded `localhost`, `127.0.0.1`, plain `http://` URLs; login callback / redirect URLs, allowed origins (CORS), webhook URLs, site or base URL settings.
5. Development leftovers: debug mode switched on, test accounts, default admin credentials (report the names of the places, not the values), seed or reset scripts that run automatically, verbose error pages that would show server paths or stack traces to visitors.
6. File-name capitalization: imports whose capitalization differs from the real file name (works on macOS, fails on Linux servers).
7. Lockfile present, and anything needed at build time that is only in devDependencies.

Only report what affects a live service; skip harmless console messages. Do NOT run builds or tests.

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
