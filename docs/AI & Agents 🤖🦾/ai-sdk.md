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
* A registered sender: a virtual number or alphanumeric sender ID for SMS and MMS, and a registered RCS agent ID for RCS.

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
| `kudosity_send_rcs` | `kudositySendRcs` | Send a rich, branded RCS message, with optional SMS fallback for devices that cannot receive RCS |
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

## RCS: two things to get right

* **The RCS `sender` is a registered RCS agent ID, never a phone number.** Registration is not part of the API — see the [RCS agent registration process](https://developers.kudosity.com/docs/rcs-onboarding), then use the agent ID here.
* **Include an SMS fallback on almost every send.** Not every handset can receive RCS. The tool's input schema prompts the model to supply `sms_fallback_message`; the fallback *sender* stays in your configuration (`defaultFallbackSender`), so the model never picks the number.

## Configuration reference

| Option | Default | Notes |
| --- | --- | --- |
| `apiKey` | `KUDOSITY_API_KEY` environment variable | Kudosity API key, sent as `x-api-key` |
| `apiSecret` | `KUDOSITY_API_SECRET` environment variable | Unlocks the account, contact-list, and batch tools |
| `defaultSender` | — | Used when the model does not supply a sender |
| `defaultFallbackSender` | — | RCS tool and `kudosityTools` only: the SMS sender used when the model adds a fallback message |
| `baseUrl` | `https://api.transmitmessage.com` | Override for testing |
| `v1BaseUrl` | `https://api.transmitsms.com` | Override for testing |

## Next steps

* [LangChain tools](https://developers.kudosity.com/docs/langchain) — the same messaging tools for LangChain.js agents
* [Agent Skills](https://developers.kudosity.com/docs/agent-skills) — teach coding agents how the whole API behaves, including WhatsApp
* [Send messages from AI agents](https://developers.kudosity.com/docs/send-messages-from-ai-agents) — runnable examples for every channel
