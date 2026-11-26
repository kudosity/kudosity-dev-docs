---
title: Send messages from AI agents & workflows
hidden: false
---

Kudosity is the **messaging action layer for AI agents**. When an AI agent or AI workflow decides
it needs to contact a customer — confirm an order, send a reminder, follow up a lead — Kudosity
turns that decision into a real **SMS, MMS, WhatsApp, or RCS** message with a single API call.

This guide shows the pattern and gives you a copy-paste starting point for each channel. Every
example is plain Node.js with no dependencies, so it drops straight into an agent tool, a workflow
step, or a function call.

> 📦 **Runnable examples:** Everything here is available as a working repo —
> [github.com/kudosity/ai-agent-examples](https://github.com/kudosity/ai-agent-examples).

## Before you start

1. [Sign up for a trial](https://kudosity.com/trial).
2. Grab your **API key** from the dashboard under **Settings → API Settings**.
3. Register a **sender** (a number or alphanumeric sender ID) for the country you're sending to.

All requests share the same basics:

- **Base URL:** `https://api.transmitmessage.com`
- **Auth:** an `x-api-key` header
- **Recipients:** [E.164 format](https://en.wikipedia.org/wiki/E.164) (e.g. `61412345678`)

## The pattern

Every example follows the same shape — an agent decision becomes a function call becomes a message:

```
AI agent / workflow decision  →  send<Channel>()  →  Kudosity API
```

Expose that `send<Channel>()` function to your model as a tool (OpenAI function calling, an MCP
tool, or our ready-made [LangChain tools](/docs/langchain)) and the agent can reach customers whenever its logic says to.

## Send an SMS from an AI agent

The core pattern — an agent decides to text a customer:

```js
async function sendSMS(recipient, message) {
  const res = await fetch("https://api.transmitmessage.com/v2/sms", {
    method: "POST",
    headers: { "Content-Type": "application/json", "x-api-key": process.env.KUDOSITY_API_KEY },
    body: JSON.stringify({ sender: process.env.KUDOSITY_SENDER, recipient, message }),
  });
  if (!res.ok) throw new Error(`Kudosity SMS failed: ${res.status} ${await res.text()}`);
  return res.json();
}
```

Required fields: `message`, `sender`, `recipient`. Add an optional `message_ref` (≤ 500 chars) to
correlate delivery webhooks back to a record in your system.

## Send an MMS (image) from an AI workflow

Useful for AI-generated marketing or rich notifications:

```js
await fetch("https://api.transmitmessage.com/v2/mms", {
  method: "POST",
  headers: { "Content-Type": "application/json", "x-api-key": process.env.KUDOSITY_API_KEY },
  body: JSON.stringify({
    sender: process.env.KUDOSITY_SENDER,
    recipient: "61412345678",
    subject: "New launch",                 // max 20 ASCII characters
    message: "Introducing AeroPro — tap to explore.",
    content_urls: ["https://example.com/launch.jpg"], // one file, absolute URL, up to 400 KB
  }),
});
```

> 🇦🇺 **MMS currently delivers to Australia only.**

## Send a WhatsApp template from an AI workflow

WhatsApp business messaging uses **pre-approved templates**. The body is a nested envelope, and
`parameters` is a positional array of strings that fills the `{1}`, `{2}` placeholders in order:

```js
await fetch("https://api.transmitmessage.com/v2/whatsapp/messages", {
  method: "POST",
  headers: { "Content-Type": "application/json", "x-api-key": process.env.KUDOSITY_API_KEY },
  body: JSON.stringify({
    sender: process.env.KUDOSITY_WHATSAPP_SENDER, // registered WhatsApp number, E.164
    recipient: "61411122211",
    content_type: "template",
    content: { template: { name: "order_update", parameters: ["#12345", "shipped"] } },
    message_ref: "order-12345",
  }),
});
```

Register and get your templates approved before sending. The template `name` must match exactly
(lowercase, alphanumeric, underscores).

## Send an RCS message with SMS fallback

RCS gives you a richer, branded experience: text, images and video, rich cards and carousels, with
buttons the recipient can tap. Three things make it different:

- **`sender` is your RCS agent ID, not a phone number.** You create, test and launch the agent in
  the Kudosity app. See [RCS onboarding](/docs/rcs-onboarding).
- Set **`sms_fallback`** with a `message` and a `sender`, so recipients on non-RCS devices still get
  the message. Without `sms_fallback.sender`, the fallback SMS isn't sent.
- A `200` response means the message was accepted, not delivered. Delivery and read receipts arrive
  in `RCS_STATUS` [webhook events](https://developers.kudosity.com/reference/about-webhooks).

```js
await fetch("https://api.transmitmessage.com/v2/rcs/messages", {
  method: "POST",
  headers: { "Content-Type": "application/json", "x-api-key": process.env.KUDOSITY_API_KEY },
  body: JSON.stringify({
    sender: process.env.KUDOSITY_RCS_AGENT_ID,   // RCS agent ID
    recipient: "61438333061",
    content_type: "text",
    content: { text: { message: "Your order #12345 is out for delivery, ETA 2pm." } },
    sms_fallback: { sender: process.env.KUDOSITY_SENDER, message: "Your order #12345 is out for delivery, ETA 2pm." },
    message_ref: "order-12345",
  }),
});
```

### Send a rich card with buttons

A rich card combines an image, a title, text and up to four buttons. Suggestions under the card give
the recipient one-tap replies.

```js
await fetch("https://api.transmitmessage.com/v2/rcs/messages", {
  method: "POST",
  headers: { "Content-Type": "application/json", "x-api-key": process.env.KUDOSITY_API_KEY },
  body: JSON.stringify({
    sender: process.env.KUDOSITY_RCS_AGENT_ID,   // RCS agent ID
    recipient: "61438333061",
    content_type: "rich_card",
    content: {
      rich_card: {
        standalone: {
          card_content: {
            title: "Your order is out for delivery",
            description: "Order #12345 arrives today between 2pm and 4pm.",
            media: { file_url: "https://example.com/images/van.jpg" },
            suggestions: [
              { action: { text: "Track order", open_url_action: { url: "https://example.com/track/12345" } } },
            ],
          },
        },
      },
      suggestions: [
        { reply: { text: "Reschedule", postback_data: "order-12345:reschedule" } },
      ],
    },
    sms_fallback: {
      sender: process.env.KUDOSITY_SENDER,
      message: "Your order #12345 arrives today 2-4pm. Track: https://example.com/track/12345",
    },
    message_ref: "order-12345",
  }),
});
```

When the recipient taps **Reschedule**, your webhook receives an `RCS_INBOUND` event with
`mo.content_type` set to `SUGGESTION` and your `postback_data`. Route on `postback_data`, not on the
label.

The image URL is a placeholder. A media URL must be a public `https` URL that doesn't redirect, and
the request fails if Kudosity can't fetch the file. See the
[RCS Message Guide](/docs/rcs-message-guide) for carousels and every other format.

## No-code: use Kudosity as an MCP tool

If you're working in Claude Desktop, Cursor, or Windsurf, connect the Kudosity MCP server and your
assistant can send messages as a native tool — no code required.

```bash
claude mcp add kudosity --scope project \
  -- npx mcp-remote https://developers.kudosity.com/mcp --header "x-api-key: YOUR_API_KEY"
```

The Kudosity MCP server authenticates with your API key in the `x-api-key` header — for clients
that only take a bare URL, use the `mcp-remote` bridge above to inject it.

Then just ask: *"Send an SMS to +61412345678 letting them know their order has shipped."*

## A note on transactional messaging

Reminders, order updates, and payment notifications are typically **transactional** and should stay
free of promotional content. Messaging consent, sender registration, and opt-out requirements vary
by country and channel — follow the rules that apply to your recipients, and where required include
an appropriate opt-out mechanism (such as "Reply STOP to opt out"). Keep marketing on a separate,
consented channel.

## Next steps

- Clone the working examples: [github.com/kudosity/ai-agent-examples](https://github.com/kudosity/ai-agent-examples)
- Explore the full [API reference](https://developers.kudosity.com/)
- Connect the [MCP server](https://developers.kudosity.com/mcp)
- Building in an AI app builder? See [Vercel Connect & v0](/docs/vercel-connect) and [Bolt](/docs/bolt)
