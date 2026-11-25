---
title: Bolt
deprecated: false
hidden: false
metadata:
  title: Use Kudosity with Bolt | Kudosity Docs
  description: >-
    Send SMS, MMS, RCS and WhatsApp from Bolt. Connect Kudosity through One,
    Bolt's integration layer, or have Bolt generate app code against the
    Kudosity API.
  robots: index
---
[Bolt](https://bolt.new) builds full-stack apps from a prompt. Kudosity reaches Bolt through [One](https://withone.ai) (formerly Pica), the integration layer Bolt partners with, where Kudosity is a [listed platform](https://www.withone.ai/knowledge/kudosity) with every V2 API action documented and verified. You can also simply ask Bolt to write code against the Kudosity API.

Pick the path that matches what you are doing:

| You want to… | Use |
| --- | --- |
| Send messages, look up delivery or manage webhooks **while you build**, from the Bolt chat | [One as a connector](#connect-one-to-bolt) |
| Ship an app that sends messages **from your own Kudosity account** | [Generated code](#have-bolt-write-the-code) |
| Ship an app where **each user connects their own Kudosity account** | [One Connect](#let-your-users-connect-their-own-kudosity) |

One's own guide for this page's first path is at [withone.ai/connect/bolt/kudosity](https://www.withone.ai/connect/bolt/kudosity).

## Before you begin

* A Kudosity account and an **API key** (dashboard → **Settings → API Settings**).
* A registered **sender** for the channel you will use.
* A free [One](https://app.withone.ai) account for the connector path.

## Connect One to Bolt

This gives the Bolt agent three tools — list your connected platforms, find the right Kudosity action for a task, and run it — with your key held by One, never by Bolt.

1. **Connect Kudosity in One.** Sign in at [app.withone.ai](https://app.withone.ai), find **Kudosity** in the apps picker (Messaging) and connect it with your API key.
2. **Add One to Bolt.** In Bolt, open the chatbox **+** → **Connectors** → **Manage connectors** (or **Settings → Connectors (MCP)**) and choose **Custom MCP server**:
   * **Name:** One
   * **URL:** `https://mcp.withone.ai/mcp`
   * **Transport:** HTTP
   * **Authentication:** MCP OAuth
3. Select **Connect** and sign in to One. On the consent screen choose the Kudosity connection and the access you want to grant (read-only, read-write, or specific actions).
4. In a project, open **+ → Connectors** and switch **One** on. Tick *Automatically enable for new projects* in the connector's settings if you want it everywhere.

Then ask in plain language:

* *"Using One, send an SMS through Kudosity to 61400000000 saying your order has shipped, then show me its delivery status."*
* *"List my last ten Kudosity SMS messages and summarise the failures."*
* *"Register a Kudosity webhook for inbound SMS pointing at this app's /api/sms-inbound route."*

Bolt calls One's `find_one_actions` to pick the Kudosity action, then `execute_one_action` to run it. Keep other connectors off in that project — each one adds tools to the context and Bolt's own documentation warns it degrades output.

> 📘 This path makes the **agent** able to use Kudosity while you work. It does not add messaging to the app Bolt is building — for that, use one of the next two paths.

## Have Bolt write the code

For an app that sends from your own Kudosity account, ask Bolt directly and keep the key server-side:

1. Add your API key as a **server-side secret** in Bolt (never in client code).
2. Prompt, for example: *"Add SMS appointment reminders using the Kudosity V2 API: POST https://api.transmitmessage.com/v2/sms with an x-api-key header, sender from an env var, and store the returned message id."*

Two things to check in generated code:

* **Host.** V2 endpoints live on `https://api.transmitmessage.com`. If Bolt writes `api.transmitsms.com` with a `/v2/` path, that is the legacy V1 host and the call will fail — fix the host.
* **Sender.** Every send needs a registered `sender`. Alphanumeric senders work for SMS; MMS needs a number; RCS takes an agent ID.

The copy-paste examples in [Send messages from AI agents](/docs/send-messages-from-ai-agents) are the shapes to point Bolt at, and `npx skills add kudosity/skills` installs the same knowledge into agents that support [Agent Skills](/docs/agent-skills).

## Let your users connect their own Kudosity

If you are building a product where each customer messages from **their** Kudosity account, use **One Connect**: One holds each user's credentials, your app stores one ID per user, and One checks the grant on every call.

1. In One, go to **Developers → Connect**, create a Connect app, and select Kudosity with the access level you want.
2. Open the ready-made Bolt prompt from [One's Kudosity guide for Bolt](https://www.withone.ai/connect/bolt/kudosity); it scaffolds sign-in, the `/api/one-connect` routes and a connect button.
3. Add the One client ID, secret and Connect key as server-side secrets in Bolt, publish, and register the `/callback` URL on your published address.

Calls outside a user's grant are rejected by One before they reach Kudosity.

## Next steps

* [Kudosity on One](https://www.withone.ai/knowledge/kudosity) — all 22 V2 actions, with One's setup guides for [Bolt](https://www.withone.ai/connect/bolt/kudosity), [Claude Code](https://www.withone.ai/connect/claude-code/kudosity) and [Cursor](https://www.withone.ai/connect/cursor/kudosity)
* Bolt's guide to [custom MCP connectors](https://support.bolt.new/building/using-bolt/connect-mcp)
* Building on Vercel or v0 instead? See [Vercel Connect & v0](/docs/vercel-connect)
