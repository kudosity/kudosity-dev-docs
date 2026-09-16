---
title: LangChain Tools
deprecated: false
hidden: false
metadata:
  title: LangChain Tools for Messaging | Kudosity Docs
  description: >-
    Give your LangChain.js agent tools to send SMS, MMS and RCS through
    Kudosity with the langchain-kudosity npm package.
  robots: index
---
The [`langchain-kudosity`](https://www.npmjs.com/package/langchain-kudosity) package gives your LangChain.js agent tools to send **SMS, MMS, and RCS** through Kudosity. Hand the tools to an agent, and it can text a customer whenever its logic decides to — an appointment reminder after a booking, a delivery update, a follow-up when a lead goes quiet.

The package is open source under the MIT licence at [github.com/kudosity/langchain-kudosity](https://github.com/kudosity/langchain-kudosity).

## Install

```bash
npm install langchain-kudosity @langchain/core
```

The package works with `@langchain/core` 0.3 and 1.x.

## Before you begin

You need:

* A Kudosity account. [Sign up](https://kudosity.com/signup) if you do not have one.
* Your API key, from the dashboard under **Developers** > **API Settings**.
* A registered sender: a virtual number or alphanumeric sender ID for SMS and MMS, and a registered RCS agent ID for RCS.

Set your API key as an environment variable:

```bash
export KUDOSITY_API_KEY="your_api_key"
```

## Available tools

| Tool | Class | What it does |
| --- | --- | --- |
| `kudosity_send_sms` | `KudositySendSms` | Send a text message to a phone number |
| `kudosity_send_mms` | `KudositySendMms` | Send an image, GIF, video, or audio attachment, passed as public URLs |
| `kudosity_send_rcs` | `KudositySendRcs` | Send a rich, branded RCS message, with optional SMS fallback for devices that cannot receive RCS |

Each tool returns the message `id` and status as JSON, so your agent can match the send against [delivery webhooks](https://developers.kudosity.com/docs/validating-webhook-signatures-from-kudosity). When a call fails, the tool returns the API's validation detail — every failed field at once — as JSON instead of throwing, so the agent can correct its input and retry.

## Use a tool directly

```ts
import { KudositySendSms } from "langchain-kudosity";

const sendSms = new KudositySendSms({
  // a virtual number on your account, or an alphanumeric sender ID (max 11 chars)
  defaultSender: "61481074185",
});

const result = await sendSms.invoke({
  recipient: "61491570156",
  message: "Your appointment is tomorrow at 10am. Reply STOP to opt out.",
});
// → {"id":"2d2c8fb6-…","status":"queued","recipient":"61491570156",…}
```

## Hand the tools to an agent

```ts
import { createAgent } from "langchain";
import {
  KudositySendSms,
  KudositySendMms,
  KudositySendRcs,
} from "langchain-kudosity";

const tools = [
  new KudositySendSms({ defaultSender: "61481074185" }),
  new KudositySendMms({ defaultSender: "61481074185" }),
  new KudositySendRcs({
    defaultSender: "YourAgentId", // a registered RCS agent ID, not a phone number
    defaultFallbackSender: "61481074185",
  }),
];

const agent = createAgent({ model: "anthropic:claude-sonnet-5", tools });

await agent.invoke({
  messages: [
    { role: "user", content: "Text 61491570156 that their delivery arrives Thursday." },
  ],
});
```

The model supplies the recipient and message. The sender comes from your configuration by default, so the model cannot invent one — it can only override it with another sender you have registered.

## RCS: two things to get right

* **The RCS `sender` is a registered RCS agent ID, never a phone number.** Registration is not part of the API — see the [RCS agent registration process](https://developers.kudosity.com/docs/rcs-onboarding), then use the agent ID here.
* **Include an SMS fallback on almost every send.** Not every handset can receive RCS. The tool's input schema prompts the model to supply `sms_fallback_message`; the fallback *sender* stays in your configuration (`defaultFallbackSender`), so the model never picks the number.

## Configuration reference

| Option | Default | Notes |
| --- | --- | --- |
| `apiKey` | `KUDOSITY_API_KEY` environment variable | Kudosity V2 API key, sent as `x-api-key` |
| `defaultSender` | — | Used when the model does not supply a sender |
| `defaultFallbackSender` | — | RCS tool only: the SMS sender used when the model adds a fallback message |
| `baseUrl` | `https://api.transmitmessage.com` | Override for testing |

## Next steps

* [Agent Skills](https://developers.kudosity.com/docs/agent-skills) — teach coding agents how the whole API behaves, including WhatsApp and contact lists
* [Kudosity MCP server](https://developers.kudosity.com/docs/mcp) — the same capability for MCP-compatible clients
* [Send messages from AI agents](https://developers.kudosity.com/docs/send-messages-from-ai-agents) — runnable examples for every channel
