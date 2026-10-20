---
title: Vercel Connect & v0
deprecated: false
hidden: false
metadata:
  title: Use Kudosity with Vercel Connect and v0 | Kudosity Docs
  description: >-
    Kudosity is listed in Vercel Connect. Create a Kudosity connector once,
    then let your Vercel-hosted apps, agents and v0 projects send SMS, MMS,
    RCS and WhatsApp without handling the API key in code.
  robots: index
---
Kudosity is a listed service in [Vercel Connect](https://vercel.com/connect/kudosity), Vercel's credential layer for apps and agents. You create a **Kudosity connector** once in your Vercel team and paste in your API key. From then on, any linked project — including apps built in **v0** — asks Connect for the key at runtime and never carries it in code, environment variables or chat.

## Before you begin

You need:

* A Kudosity account. [Sign up](https://kudosity.com/signup) if you do not have one.
* An **API key**, from the Kudosity dashboard under **Settings → API Settings**.
* A registered **sender**: a virtual number or alphanumeric sender ID for SMS, a number for MMS, a registered RCS agent ID for RCS.
* A Vercel team with permission to create connectors, and the [Vercel CLI](https://vercel.com/docs/cli) if you prefer the terminal.

## Create the connector

**In the dashboard:** open your team, choose **Connect** → **Browse Connectors**, search for *Kudosity*, and select **Create Connector**. Give it a name (for example `my-kudosity`) and paste your API key when asked.

**Or from the terminal:**

```bash
vercel connect create kudosity --name my-kudosity
```

Either way the connector is identified as `kudosity/<name>` — `kudosity/my-kudosity` in this example. Link the projects that should be allowed to use it (**Connect** → your connector → **Projects**), and link each environment you want it available in.

## Use it from your code

Install the Connect SDK in your project:

```bash
npm install @vercel/connect
```

Ask Connect for the key and send a message. Kudosity authenticates with an `x-api-key` header, so the value goes in that header — not in `Authorization: Bearer`:

```ts
import { getToken } from '@vercel/connect';

export async function sendSms(recipient: string, message: string) {
  const apiKey = await getToken('kudosity/my-kudosity', { subject: { type: 'app' } });

  const res = await fetch('https://api.transmitmessage.com/v2/sms', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'x-api-key': apiKey },
    body: JSON.stringify({
      sender: process.env.KUDOSITY_SENDER,   // configuration, not a credential — fine as an env var
      recipient,                              // E.164, e.g. 61412345678
      message,
    }),
  });
  if (!res.ok) throw new Error(`Kudosity SMS failed: ${res.status} ${await res.text()}`);
  return res.json();                          // includes the message id for delivery lookups
}
```

The same pattern covers every V2 endpoint — swap the path for `/v2/mms`, `/v2/whatsapp/messages` or `/v2/rcs/messages` and the body for that channel's payload. See [Send messages from AI agents](/docs/send-messages-from-ai-agents) for each channel's shape.

**Local development:** the SDK reads `VERCEL_OIDC_TOKEN`, which a Vercel deployment provides automatically. Locally, run `vercel link` and then `vercel env pull` to fetch a short-lived development token into `.env.local`; re-run `vercel env pull` if you see authentication errors.

## Use it in v0

v0 builds on Vercel Connect, so a Kudosity connector is how a v0 app sends messages without the key ever appearing in the chat. Vercel Connect in v0 is currently in beta.

1. Create the Kudosity connector in your Vercel team as above, and link the project v0 deploys to.
2. In v0, open the **Project menu → Settings → Integrations** to see the connections attached to the project.
3. Tell v0 what you want, naming the connector:

   *"Send an SMS confirmation through the Kudosity connector when a booking is saved, and show the delivery status on the admin page."*

v0 writes the `@vercel/connect` call for you and keeps the key out of the generated code. If v0 proposes a different messaging service, say "use Kudosity" — it uses whatever connector the project is linked to.

## Give your coding agent the know-how

Two skill packs make agents fluent in both halves of this setup:

```bash
npx skills add vercel/vercel-plugin --skill vercel-connect   # how Vercel Connect works
npx skills add kudosity/skills                               # how the Kudosity APIs work
```

The Kudosity [Agent Skills](/docs/agent-skills) cover every channel, authentication and payload shape, so the agent writes correct calls the first time.

## Good to know

* **One connector, many apps.** Link the same connector to several projects. Rotate the key in Kudosity, update it on the connector, and every linked app picks up the new key — no redeploys.
* **Delivery results.** Every send returns an `id`; `GET /v2/sms/{id}` (and the per-channel equivalents) returns the status, or register a [webhook](/docs/webhooks-v2) to be told when it changes.
* **Rate limits.** Connect's `getToken` is cached in-process; the SDK refreshes it as needed. Kudosity's own [rate limits](/reference/about-our-apis) apply to the API calls.

## Next steps

* Kudosity in the Vercel Connect directory: [vercel.com/connect/kudosity](https://vercel.com/connect/kudosity)
* [Vercel Connect documentation](https://vercel.com/docs/connect) and the [TypeScript SDK reference](https://vercel.com/docs/connect/ts-sdk-reference)
* Building an agent rather than an app? The [Vercel AI SDK tools](/docs/ai-sdk) package gives an AI SDK agent native Kudosity tools.
