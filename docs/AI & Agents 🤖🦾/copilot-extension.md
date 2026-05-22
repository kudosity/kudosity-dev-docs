---
title: GitHub Copilot Extension
deprecated: false
hidden: false
metadata:
  robots: index
---
The Kudosity GitHub Copilot Extension allows you to send messages, manage contacts, and build messaging workflows directly from GitHub Copilot in VS Code using natural language.

It provides an AI-powered interface to the Kudosity messaging platform, enabling rapid development, automation, and experimentation without writing code.

## Features

With the Copilot Extension, you can:

* Send SMS and MMS messages
* Create and manage contact lists
* Configure messaging workflows
* Manage webhooks for inbound messages and delivery tracking

## Installation

> 📘 Requires Copilot Agent Mode
> The extension runs API calls through terminal commands, which requires GitHub Copilot in **Agent Mode** (the mode dropdown at the bottom of Copilot Chat). Regular Copilot Chat can read the instructions but cannot run the commands.

Choose one of two installation methods.

### Option 1: Clone the repository as a VS Code workspace

```bash
git clone https://github.com/kudosity/kudosity-copilot-extension.git
cd kudosity-copilot-extension
code .
```

VS Code automatically picks up the workspace MCP config and the Copilot instructions file. The Kudosity MCP server appears in the Copilot Chat MCP indicator within a few seconds.

### Option 2: Merge the MCP snippet into your VS Code user settings

If you want Kudosity messaging available in every workspace:

1. Open the command palette (Cmd+Shift+P / Ctrl+Shift+P).
2. Run **Preferences: Open User Settings (JSON)**.
3. Merge the `mcp.servers.kudosity` block from [`configs/vscode-user-settings.json`](https://github.com/kudosity/kudosity-copilot-extension/blob/main/configs/vscode-user-settings.json) into your user `settings.json`.
4. Reload VS Code.

## Authentication

To use the extension, you will need your Kudosity API credentials.

1. Get your API Key and Secret from the Kudosity dashboard under **Developers → API Settings**.
2. Set environment variables in your shell profile (`~/.zshrc` or `~/.bashrc`):

   ```bash
   export KUDOSITY_API_KEY="your_api_key"
   export KUDOSITY_API_SECRET="your_api_secret"
   ```

3. Restart VS Code so Copilot's terminal picks up the new environment.

To verify the credentials are loaded, ask Copilot Chat (Agent Mode) to check your Kudosity account balance — it will run a request against the V1 `/get-balance.json` endpoint and confirm both auth paths work.

## Example usage

You can interact with the extension using natural language in Copilot Chat (Agent Mode).

### Send a message

> Send an SMS to +61400000000 saying "Your order has been shipped"

### Create a contact list

> Create a contact list called "Customers" with these phone numbers

### Configure a webhook

> Set up a webhook for incoming messages to https://example.com/webhook

## When to use

Use the Copilot Extension when you want to:

* Quickly send messages without writing code
* Prototype messaging workflows
* Automate communication tasks
* Build internal tools powered by AI
* Experiment with messaging using natural language

## How it works

The Copilot Extension uses the Model Context Protocol (MCP) to connect Copilot to the Kudosity API.

The extension connects to the Kudosity MCP server for API documentation lookup only. All API operations are executed **locally on your machine** using `curl` with your own credentials.

Your credentials are never sent to the MCP server or to GitHub Copilot. They are used only in direct API calls to Kudosity.
