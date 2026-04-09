---
title: Claude Code Plugin
deprecated: false
hidden: false
metadata:
  robots: index
---
The Kudosity Claude Code Plugin is available on the official Claude Code marketplace. Send messages, manage contacts, and build messaging workflows directly from Claude Code using natural language.

It provides an AI-powered interface to the Kudosity messaging platform, enabling rapid development, automation, and experimentation without writing code.

## Features

With the Claude Plugin, you can:

* Send SMS and MMS messages
* Create and manage contact lists
* Configure messaging workflows
* Manage webhooks for inbound messages and delivery tracking

## Installation

Install from the official Claude Code marketplace:

```
/plugin install kudosity-sms
```

Select "Install for you (user scope)".

## Authentication

To use the plugin, you will need a Kudosity API key.

1. Get your API key from the Kudosity dashboard
2. Configure the plugin with your API credentials
3. Ensure your account has messaging permissions enabled

## Example usage

You can interact with the plugin using natural language inside Claude.

### Send a message

> Send an SMS to +61400000000 saying "Your order has been shipped"

### Create a contact list

> Create a contact list called "Customers" with these phone numbers

### Configure a webhook

> Set up a webhook for incoming messages to [https://example.com/webhook](https://example.com/webhook)

## When to use

Use the Claude Plugin when you want to:

* Quickly send messages without writing code
* Prototype messaging workflows
* Automate communication tasks
* Build internal tools powered by AI
* Experiment with messaging using natural language

## How it works

The Claude Code Plugin uses the Model Context Protocol (MCP) to connect Claude to the Kudosity API.

This allows Claude to:

* Discover available messaging capabilities
* Execute actions on your behalf
* Interact with structured data and workflows

All actions are performed securely using your API credentials.