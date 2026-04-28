---
title: OpenClaw Plugin
deprecated: false
hidden: false
metadata:
  robots: index
---
The Kudosity OpenClaw Plugin enables you to send SMS messages from any OpenClaw-powered agent or workflow using natural language, with a real Kudosity sender number and no app required on the recipient's phone.

It provides an AI-powered SMS channel for the OpenClaw ecosystem, enabling rapid development, automation, and experimentation without writing code.

## Features

With the OpenClaw Plugin, you can:

* Send outbound SMS via the Kudosity v2 API
* Configure your sender and API key through an interactive setup wizard
* Validate API keys against the Kudosity API during setup
* Parse inbound webhook payloads using built-in utilities
* Automatically split long messages into concatenated SMS segments (up to 1,600 characters)

## Installation

Install from [ClawHub](https://clawhub.ai/plugins/kudosity-openclaw-sms) (preferred) or [npm](https://www.npmjs.com/package/kudosity-openclaw-sms), then restart the OpenClaw gateway:

```bash
openclaw plugins install kudosity-openclaw-sms
openclaw gateway restart
```

OpenClaw checks ClawHub first and falls back to npm automatically.

## Authentication

To use the plugin, you need:

* A Kudosity API key
* A sender number

### Option 1: Setup wizard (recommended)

1. Get your API key from the Kudosity dashboard under **Settings → API Keys → Create Key**
2. Get a sender number under **Numbers → Lease a virtual number**, or use a number you already own
3. Run `openclaw setup`, select **SMS Kudosity** from the channel list, and enter your API key and sender

The wizard validates your API key against the Kudosity API and writes the credentials into your OpenClaw config.

### Option 2: Environment variables

Set the following variables in your shell profile:

```bash
export KUDOSITY_API_KEY="your_api_key"
export KUDOSITY_SENDER="your_sender_id_or_number"
```

Both variables are required together. You also need to enable the plugin in your OpenClaw config by setting `plugins.entries["kudosity-sms"].enabled: true`.

## Example usage

You can interact with the plugin using natural language inside any OpenClaw agent.

### Send a message

> Send an SMS to +61400000000 saying "Your order has been shipped"

### Send a notification from a workflow

> When a new order comes in, text the customer their tracking link

### Send directly from the command line

```bash
openclaw send --channel kudosity-sms --to +61400000000 --text "hello from openclaw"
```

## When to use

Use the OpenClaw Plugin when you want to:

* Add SMS as a channel inside an OpenClaw agent or workflow
* Send transactional or notification messages from OpenClaw automations
* Prototype SMS-driven flows without writing API integration code
* Combine SMS with other OpenClaw plugins in a single agent

## How it works

The OpenClaw Plugin implements the OpenClaw `ChannelPlugin` interface, registering Kudosity SMS as a first-class channel that any OpenClaw agent can route messages through.

Outbound calls go directly from your machine to the Kudosity v2 API using your own credentials. The plugin uses OpenClaw's built-in SSRF-guarded fetch runtime, so external requests are validated against OpenClaw's safety policy.

For inbound messaging, the plugin exports helper utilities:

* `parseWebhookPayload`
* `toInboundMessage`
* `handleWebhookRequest`

Use these to build webhook handlers for inbound SMS. Auto-registered webhook routes are planned for a future release.

Source code: [github.com/kudosity/openclaw-sms](https://github.com/kudosity/openclaw-sms)
