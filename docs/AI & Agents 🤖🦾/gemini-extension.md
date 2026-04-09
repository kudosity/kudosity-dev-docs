---
title: Gemini CLI Extension
deprecated: false
hidden: false
metadata:
  robots: index
---
The Kudosity Gemini CLI Extension allows you to send messages, manage contacts, and build messaging workflows directly from Gemini CLI using natural language.

It provides an AI-powered interface to the Kudosity messaging platform, enabling rapid development, automation, and experimentation without writing code.

## Features

With the Gemini CLI Extension, you can:

* Send SMS and MMS messages
* Create and manage contact lists
* Configure messaging workflows
* Manage webhooks for inbound messages and delivery tracking

## Installation

Install from the [Gemini CLI Extension Gallery](https://geminicli.com/extensions/?name=kudositygemini-kudosity-sms-extension):

```
gemini extensions install https://github.com/kudosity/gemini-kudosity-sms-extension
```

## Authentication

To use the extension, you will need your Kudosity API credentials.

1. Get your API Key and Secret from the Kudosity dashboard under **Developers → API Settings**
2. When you install the extension, Gemini CLI will prompt for your credentials — these are stored securely in your system keychain
3. Run the `/setup` command to verify your credentials against both V1 and V2 APIs

To reconfigure credentials later:

```
gemini extensions config gemini-kudosity-sms-extension
```

Alternatively, set environment variables in your shell profile:

```bash
export KUDOSITY_API_KEY="your_api_key"
export KUDOSITY_API_SECRET="your_api_secret"
```

## Example usage

You can interact with the extension using natural language inside Gemini CLI.

### Send a message

> Send an SMS to +61400000000 saying "Your order has been shipped"

### Create a contact list

> Create a contact list called "Customers" with these phone numbers

### Configure a webhook

> Set up a webhook for incoming messages to https://example.com/webhook

## When to use

Use the Gemini CLI Extension when you want to:

* Quickly send messages without writing code
* Prototype messaging workflows
* Automate communication tasks
* Build internal tools powered by AI
* Experiment with messaging using natural language

## How it works

The Gemini CLI Extension uses the Model Context Protocol (MCP) to connect Gemini to the Kudosity API.

The extension connects to the Kudosity MCP server for API documentation lookup only. All API operations are executed **locally on your machine** using `curl` with your own credentials.

Your credentials are never sent to the MCP server or to Gemini. They are used only in direct API calls to Kudosity.
