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
* A registered sender: a virtual number or alphanumeric sender ID for SMS and MMS, and your RCS agent ID for RCS.

Set your API key as an environment variable:

```bash
export KUDOSITY_API_KEY="your_api_key"
```

## Available tools

| Tool | Class | What it does |
| --- | --- | --- |
| `kudosity_send_sms` | `KudositySendSms` | Send a text message to a phone number |
| `kudosity_send_mms` | `KudositySendMms` | Send an image, GIF, video, or audio attachment, passed as public URLs |
| `kudosity_send_rcs` | `KudositySendRcs` | Send RCS as text, an image or video, a rich card or a carousel, with suggested replies, buttons and SMS fallback |

Each tool returns the message `id` and status as JSON, so your agent can match the send against [delivery webhooks](https://developers.kudosity.com/reference/about-webhooks). When a call fails, the tool returns the API's validation detail — every failed field at once — as JSON instead of throwing, so the agent can correct its input and retry.

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
    defaultSender: "YourAgentId", // your RCS agent ID, not a phone number
    defaultFallbackSender: "61481074185", // without it, no fallback SMS is sent
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

## Send RCS

`kudosity_send_rcs` sends text, an image or video, a rich card, or a carousel. Rich cards, carousels and suggestions need version 0.2.0 or later.

Set exactly one of `message`, `media_url` or `cards`. This example sends a rich card with a button, a suggested reply and an SMS fallback:

```ts
import { KudositySendRcs } from "langchain-kudosity";

const sendRcs = new KudositySendRcs({
  defaultSender: "YourAgentId", // your RCS agent ID, not a phone number
  defaultFallbackSender: "61481074185", // without it, no fallback SMS is sent
});

await sendRcs.invoke({
  recipient: "61491570156",
  cards: [
    {
      title: "Your order is out for delivery",
      description: "Order 12345 arrives today between 2pm and 4pm.",
      media_url: "https://example.com/images/van.jpg",
      suggestions: [
        { type: "open_url", text: "Track order", url: "https://example.com/track/12345" },
      ],
    },
  ],
  suggestions: [
    { type: "reply", text: "Reschedule", postback_data: "order-12345:reschedule" },
  ],
  sms_fallback_message:
    "Your order 12345 arrives today 2-4pm. Track: https://example.com/track/12345",
  message_ref: "order-12345",
});
```

| Input | What it does |
| --- | --- |
| `recipient` | The destination number in E.164 international format, such as `61491570156`. Local format is rejected. |
| `message` | Sends a text message, up to 3072 characters |
| `media_url` | Sends one image or video on its own, from a public `https` URL |
| `cards` | Sends a rich card when there is 1 card, and a carousel when there are 2 to 10. Each card takes a `title`, a `description`, a `media_url` and up to 4 `suggestions` |
| `suggestions` | Adds up to 11 chips under the message. Each is a `reply`, an `open_url` or a `dial`, with an optional `postback_data` |
| `sms_fallback_message` | The plain text to send by SMS when the RCS message can't be delivered |
| `sender` | Overrides `defaultSender` for one send |
| `message_ref` | Your own reference, returned in delivery webhooks |

If the input can't make a valid message, for example a card with no content, the tool returns every problem at once and sends nothing.

### Things to get right

* **The sender is your RCS agent ID, not a phone number.** You create, test and launch the agent in the Kudosity app. Until it's launched, it can message its test devices only. See [RCS onboarding](https://developers.kudosity.com/docs/rcs-onboarding).
* **An SMS fallback needs a sender.** Set `defaultFallbackSender` to a number or sender ID that your account can send SMS from. The fallback sender stays in your configuration, so the model never picks the number. If the model supplies a fallback message and no fallback sender is configured, the tool sends the RCS message without a fallback and returns a `warning`.
* **A returned `id` means the message was accepted, not delivered.** Delivery and read receipts arrive in `RCS_STATUS` webhook events. A message that falls back to SMS reports in `SMS_STATUS` events with the same `id`. See [Track an RCS message that falls back to SMS](https://developers.kudosity.com/docs/rcs-sms-fallback).
* **A tapped suggestion arrives as an `RCS_INBOUND` webhook event** with `mo.content_type` set to `SUGGESTION` and your `postback_data`. Route on `postback_data`, not on the label.
* **Media URLs must be public `https` URLs that don't redirect.** Kudosity fetches each file when you send, and the request fails if it can't.

The [RCS Message Guide](https://developers.kudosity.com/docs/rcs-message-guide) has the full rules for each format.

## Configuration reference

| Option | Default | Notes |
| --- | --- | --- |
| `apiKey` | `KUDOSITY_API_KEY` environment variable | Kudosity V2 API key, sent as `x-api-key` |
| `defaultSender` | — | Used when the model does not supply a sender |
| `defaultFallbackSender` | — | RCS tool only: the SMS sender for the fallback. Without it, no fallback SMS is sent |
| `baseUrl` | `https://api.transmitmessage.com` | Override for testing |

## Next steps

* [Agent Skills](https://developers.kudosity.com/docs/agent-skills) — teach coding agents how the whole API behaves, including WhatsApp and contact lists
* [Kudosity MCP server](https://developers.kudosity.com/docs/mcp) — the same capability for MCP-compatible clients
* [Send messages from AI agents](https://developers.kudosity.com/docs/send-messages-from-ai-agents) — runnable examples for every channel
