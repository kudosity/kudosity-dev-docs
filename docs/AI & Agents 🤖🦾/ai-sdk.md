---
title: Vercel AI SDK Tools
deprecated: false
hidden: false
metadata:
  title: Vercel AI SDK Tools for Messaging | Kudosity Docs
  description: >-
    Give your Vercel AI SDK agent tools to send SMS, MMS and RCS, manage
    contact lists, and read delivery results with the ai-sdk-kudosity npm
    package.
  robots: index
---
The [`ai-sdk-kudosity`](https://www.npmjs.com/package/ai-sdk-kudosity) package gives your [Vercel AI SDK](https://ai-sdk.dev) agent the full Kudosity messaging loop: check the account, build contact lists, send **SMS, MMS, and RCS**, and read back delivery results. The agent can text a customer when its logic decides to — and then confirm the message actually arrived.

The package is open source under the MIT licence at [github.com/kudosity/ai-sdk-kudosity](https://github.com/kudosity/ai-sdk-kudosity).

## Install

```bash
npm install ai-sdk-kudosity ai
```

The package works with AI SDK 5.

## Before you begin

You need:

* A Kudosity account. [Sign up](https://kudosity.com/signup) if you do not have one.
* Your API key and API secret, from the dashboard under **Developers** > **API Settings**.
* A registered sender: a virtual number or alphanumeric sender ID for SMS and MMS, and your RCS agent ID for RCS.

Set your credentials as environment variables:

```bash
export KUDOSITY_API_KEY="your_api_key"
export KUDOSITY_API_SECRET="your_api_secret"
```

The key alone covers sending and delivery results. The secret additionally unlocks the account, contact-list, and batch-send tools.

## Available tools

| Tool | Factory | What it does |
| --- | --- | --- |
| `kudosity_send_sms` | `kudositySendSms` | Send a text message to a phone number |
| `kudosity_send_mms` | `kudositySendMms` | Send an image, GIF, video, or audio attachment, passed as public URLs |
| `kudosity_send_rcs` | `kudositySendRcs` | Send RCS as text, an image or video, a rich card or a carousel, with suggested replies, buttons and SMS fallback |
| `kudosity_send_sms_batch` | `kudositySendSmsBatch` | Send to a whole contact list or up to 500 numbers, now or scheduled |
| `kudosity_get_sms` | `kudosityGetSms` | Get a sent message by `id`, including its delivery status |
| `kudosity_list_sms` | `kudosityListSms` | Query sent messages by date, recipient, status, or `message_ref` |
| `kudosity_get_balance` | `kudosityGetBalance` | Get the account balance and currency |
| `kudosity_create_list` | `kudosityCreateList` | Create a contact list, with up to 10 custom fields |
| `kudosity_add_to_list` | `kudosityAddToList` | Add a contact to a list |
| `kudosity_get_lists` | `kudosityGetLists` | Get the account's contact lists and their ids |

Each tool returns a structured object — the message `id`, status, and echo fields like `message_ref` — so your agent can match sends against [delivery webhooks](https://developers.kudosity.com/reference/about-webhooks) or poll for the outcome. When a call fails, the tool returns the API's validation detail — every failed field at once — instead of throwing, so the agent can correct its input and retry.

## Register the tools with an agent

`kudosityTools()` returns every tool keyed by name, ready to pass to `generateText` or `streamText`:

```ts
import { generateText } from "ai";
import { kudosityTools } from "ai-sdk-kudosity";

const result = await generateText({
  model, // any AI SDK model
  tools: kudosityTools({
    // a virtual number on your account, or an alphanumeric sender ID (max 11 chars)
    defaultSender: "61481074185",
  }),
  prompt:
    "Create a list called 'VIP customers', add 61491570156 to it, " +
    "text the list that the sale starts tomorrow, then confirm the send went out.",
});
```

The bundle always includes the sending and delivery-result tools. The account, contact-list, and batch tools are included when an API secret is configured, and omitted otherwise.

## Use individual tools

Each factory takes your account defaults and returns a standard AI SDK tool:

```ts
import { kudositySendSms, kudosityGetSms } from "ai-sdk-kudosity";

const tools = {
  kudosity_send_sms: kudositySendSms({ defaultSender: "61481074185" }),
  kudosity_get_sms: kudosityGetSms(),
};
```

The model supplies the recipient and message. The sender comes from your configuration by default, so the model cannot invent one — it can only override it with another sender you have registered.

## Close the loop on delivery

Every send tool returns a message `id`. A send result of `pending` or `queued` means the message was accepted, not delivered. Your agent can confirm the outcome two ways:

* **Poll:** `kudosity_get_sms` returns the current status — `pending`, `sent`, `accepted`, `delivered`, or `failed`. `kudosity_list_sms` queries in bulk — for example, every failed message since a date, or all messages sharing a `message_ref`.
* **Push:** pass `message_ref` at send time and match it in [delivery webhooks](https://developers.kudosity.com/reference/about-webhooks).

## Send RCS

`kudosity_send_rcs` sends text, an image or video, a rich card, or a carousel. Rich cards, carousels and suggestions need version 0.2.0 or later.

Register the tool with your RCS agent ID and an SMS sender for the fallback:

```ts
import { kudositySendRcs } from "ai-sdk-kudosity";

const tools = {
  kudosity_send_rcs: kudositySendRcs({
    defaultSender: "YourAgentId", // your RCS agent ID, not a phone number
    defaultFallbackSender: "61481074185", // without it, no fallback SMS is sent
  }),
};
```

The model sets exactly one of `message`, `media_url` or `cards`. This tool input sends a rich card with a button, a suggested reply and an SMS fallback:

```ts
{
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
}
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
| `apiKey` | `KUDOSITY_API_KEY` environment variable | Kudosity API key, sent as `x-api-key` |
| `apiSecret` | `KUDOSITY_API_SECRET` environment variable | Unlocks the account, contact-list, and batch tools |
| `defaultSender` | — | Used when the model does not supply a sender |
| `defaultFallbackSender` | — | RCS tool and `kudosityTools` only: the SMS sender for the fallback. Without it, no fallback SMS is sent |
| `baseUrl` | `https://api.transmitmessage.com` | Override for testing |
| `v1BaseUrl` | `https://api.transmitsms.com` | Override for testing |

## Next steps

* [LangChain tools](https://developers.kudosity.com/docs/langchain) — the same messaging tools for LangChain.js agents
* [Agent Skills](https://developers.kudosity.com/docs/agent-skills) — teach coding agents how the whole API behaves, including WhatsApp
* [Send messages from AI agents](https://developers.kudosity.com/docs/send-messages-from-ai-agents) — runnable examples for every channel
