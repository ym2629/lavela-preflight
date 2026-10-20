# lavela-preflight

Free, **read-only** checks for an app or game you built with AI, to run *before* you put it online.

Type one command in Claude Code and your assistant reviews the project for the things that most often go wrong at launch: keys left in the code, data anyone can open, settings that break on a real server, errors you would never hear about, surprise bills, and data that disappears on redeploy. It tells you what it found and what it could not check. It never changes your files.

> This is an AI-assisted review, not a security audit. It can miss things. For anything important, ask a person to review it too.

## Install

```bash
claude plugin marketplace add ym2629/lavela-preflight
claude plugin install lavela-preflight@lavela
```

Or inside a Claude Code session: `/plugin marketplace add ym2629/lavela-preflight`, then `/plugin install lavela-preflight@lavela`.

## Use

Open Claude Code in your project folder and run:

| Command | What it checks |
|---|---|
| `/lavela-preflight:all` | Runs checks 1–7 in order and ends with the five most important fixes |
| `/lavela-preflight:secrets` | API keys, passwords and tokens in code, in the browser bundle, and in git history |
| `/lavela-preflight:access` | Who can see or change what: pages without login, missing ownership checks, admin features hidden only in the UI, database access rules |
| `/lavela-preflight:server-ready` | Build and start commands, port, required settings (names only), `localhost` addresses, debug leftovers |
| `/lavela-preflight:errors` | Would you find out when something breaks? Logging, health check, alerts |
| `/lavela-preflight:cost` | Places where usage can make the bill explode, such as AI endpoints open to strangers |
| `/lavela-preflight:data` | Where data lives, what survives a restart, dev versus production data, backup and rollback |
| `/lavela-preflight:post-launch` | A checklist for the first 30 minutes after launch, tailored to your project |
| `/lavela-preflight:payments-privacy` | Optional: payments and personal data (test vs live by key type, webhook checks, policy pages). Not legal advice |

Example prompts:

- `/lavela-preflight:secrets` in a freshly generated Next.js app before the first deploy
- `/lavela-preflight:access` on a Supabase or Firebase project made with an AI builder
- `/lavela-preflight:all` the night before you share the link with friends

## What it will and will not do

- **Read-only.** The checks read files and a few read-only git commands. They do not edit, create or delete files, run builds, installs or migrations, or contact any server. Edit tools are switched off while a check runs.
- **No secret output.** Keys, passwords and tokens are reported by file and kind only, never printed. `.env` files are checked by name and never opened.
- **Honest limits.** Every report separates what was verified in the code from what needs a person or a dashboard.
- **No data leaves your machine.** The plugin has no hooks, no MCP servers and no network access of its own. Your assistant processes your code the same way it does when you ask it any question.

## Privacy

lavela-preflight does not collect, store or send any data, and the authors receive nothing from it. It has no server, no hooks, no MCP servers and no network access of its own. While a check runs it only reads files in your project folder and runs a few read-only git commands; the report appears in your own Claude Code session. Secret values are never printed, and `.env` files are not opened.

As with any request you make to Claude, the file contents Claude reads in that session are handled under the terms of your own Claude plan and [Anthropic's privacy policy](https://www.anthropic.com/legal/privacy). If your project contains personal data (for example sample records), the plugin may read it while checking, but it does not copy or keep it. Questions: open an issue on this repository.

## Limits

It reads code; it cannot see your hosting, database or payment dashboards, whether your keys are real, or what runs in front of your server. It is not a replacement for a penetration test or a legal review.

## Support

Open an issue on this repository. Pull requests that improve a check are welcome.

## About

Made by the team behind [lavela](https://lavela.dev), a service that lets an AI agent deploy an app from a GitHub repository and report on it afterwards. This plugin works on any project and does not need lavela.

License: MIT. 한국어 안내는 [README.ko.md](README.ko.md)를 보세요.
