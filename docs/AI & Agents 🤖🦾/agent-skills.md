---
title: Agent Skills
deprecated: false
hidden: false
metadata:
  robots: index
---
Kudosity Agent Skills teach your AI coding agent how the Kudosity messaging APIs actually work. Install them once, and your agent sends SMS, MMS, WhatsApp, and RCS correctly the first time — with the right endpoint, the right authentication, and the right payload shape.

Skills use the open [Agent Skills](https://github.com/kudosity/skills) format: a folder containing a `SKILL.md` file that your agent loads only when the task calls for it. The same file works across Claude Code, Cursor, GitHub Copilot, Windsurf, Zed, Cline, and OpenCode.

## Skills compared to the MCP server

Skills and the [Kudosity MCP server](https://developers.kudosity.com/docs/mcp) solve different problems, and they work well together.

| | What it gives your agent |
|---|---|
| **MCP server** | Tools it can call — `send_sms`, `list_messages`, `create_webhook` |
| **Agent Skills** | Knowledge of how the API behaves — which of the two APIs to use, why RCS sends through an agent ID instead of a phone number, why a WhatsApp message that returns `200` can still never arrive |

Use both. Your agent then calls the right tool with the right arguments.

## Install

Install every skill with one command:

```bash
npx skills add kudosity/skills
```

This works with more than 70 coding agents.

To install manually, clone the repository into your agent's skills directory:

```bash
git clone https://github.com/kudosity/skills.git ~/.claude/skills/kudosity
```

Use `~/.claude/skills/` for Claude Code, `.cursor/skills/` for Cursor, or `.agents/skills/` for agents that follow that convention. Cursor reads skills from the project directory only.

## Available skills

| Skill | What it covers |
|---|---|
| `kudosity-setup` | Create an account, find your API credentials, get a sender, and verify everything works |
| `kudosity-sms` | Send to one recipient or to a contact list, schedule sends, track links |
| `kudosity-mms` | Send images, GIFs, video, and audio |
| `kudosity-whatsapp` | Send templates and free-form text, handle the 24-hour service window, set SMS fallback |
| `kudosity-whatsapp-templates` | Template names, positional parameters, locales, and media headers |
| `kudosity-rcs` | Send RCS through an agent ID, set SMS fallback, check device capability |
| `kudosity-contacts-lists` | Create lists, add and remove members, handle opt-outs |
| `kudosity-webhooks` | Receive delivery status, inbound replies, link hits, and opt-outs |

Each skill works on its own. Install one, and the task it describes works from end to end.

## Before you begin

You need:

* A Kudosity account. [Sign up](https://kudosity.com/signup) if you do not have one.
* Your API key, and your API secret if you plan to use the V1 API. Find both in the dashboard under **Developers** > **API Settings**.
* A registered sender for the channel you want to use.

Set your credentials as environment variables:

```bash
export KUDOSITY_API_KEY="your_api_key"
export KUDOSITY_API_SECRET="your_api_secret"
```

> 📘 Not sure where to start?
> Install `kudosity-setup` first. It walks you through each of these steps and verifies your credentials with two read-only API calls before you send anything.

## Example usage

After you install the skills, describe what you want in your own words. Your agent loads the relevant skill and writes the call.

### Send a message

> Send an SMS to +61400000000 saying "Your order has shipped"

### Build an integration

> Add WhatsApp order-status notifications to my checkout flow using Kudosity

### Diagnose a problem

> My Kudosity WhatsApp send returns 200 but the message never arrives. Why?

Your agent loads `kudosity-whatsapp`, checks the 24-hour service window and opt-in rules, and explains which one applies.

## How skills handle the two APIs

Kudosity runs two APIs, and they authenticate differently. Getting this wrong is the most common cause of a `401`.

| API | Base URL | Authentication |
|---|---|---|
| V2 — single-recipient SMS, MMS, WhatsApp, RCS, webhooks | `api.transmitmessage.com` | `x-api-key` header |
| V1 — contact lists, bulk sends, scheduling, reporting | `api.transmitsms.com` | HTTP Basic, using your key and secret |

Every skill states which API it uses and how to authenticate against it, so your agent does not have to guess.

> ⚠️ Keep your credentials local
> Your API key and secret stay on your machine as environment variables. Skills instruct your agent to read them at call time. Never commit credentials to source control.

## Contribute

The skills are open source under the MIT licence at [github.com/kudosity/skills](https://github.com/kudosity/skills). If a skill sends your agent down the wrong path, open an issue — that is a bug worth reporting.

## Next steps

* [Kudosity MCP server](https://developers.kudosity.com/docs/mcp) — give your agent callable tools to pair with these skills
* [Send messages from AI agents](https://developers.kudosity.com/docs/send-messages-from-ai-agents) — runnable examples for every channel
* [Claude Code plugin](https://developers.kudosity.com/docs/claude-plugin) — a packaged Claude Code experience that bundles several of these skills
