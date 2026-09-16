---
title: AI-Native Messaging Infrastructure Overview
deprecated: false
hidden: false
metadata:
  title: AI-Native Messaging API Overview | Kudosity Docs
  description: >-
    Kudosity gives AI agents and plugin platforms like Claude and Gemini CLI
    direct access to messaging infrastructure. See what is supported.
  robots: index
---
Kudosity provides an AI-native interface to messaging infrastructure, allowing you to interact with messaging APIs using AI agents and plugin platforms such as Claude, Gemini CLI, Cursor, and OpenClaw.

Instead of building direct API integrations, you can use natural language to send messages, manage contacts, and automate communication workflows.

***

## What are AI agents?

AI agents are systems that can understand intent and take actions using external tools.

With Kudosity, AI agents can:

* Send SMS, MMS, WhatsApp, and RCS messages
* Manage contacts and messaging lists
* Configure messaging workflows and webhooks
* Automate communication tasks

This enables developers and teams to interact with messaging systems using natural language instead of writing code.

***

## Agent Skills

[Agent Skills](/docs/agent-skills) teach your AI coding agent how the Kudosity APIs actually work — which of the two APIs to use, how each authenticates, and the payload shapes for every channel. One command installs all eight skills across more than 70 coding agents:

```bash
npx skills add kudosity/skills
```

Skills pair with the MCP server below: the server gives your agent tools it can call, and skills give it the knowledge to call them correctly.

***

## Plugin integrations

Install Kudosity directly inside the host platform’s plugin system. Each plugin runs locally on your machine and uses your own Kudosity API credentials.

* [Claude Code Plugin](/docs/claude-plugin) — installed from the Claude Code marketplace
* [GitHub Copilot Extension](/docs/copilot-extension) — installed by opening the repository as a VS Code workspace or merging the MCP snippet into your VS Code user settings
* [Gemini CLI Extension](/docs/gemini-extension) — installed from the Gemini CLI Extension Gallery
* [OpenClaw Plugin](/docs/openclaw-plugin) — installed from ClawHub or npm

***

## Framework integrations

Building an agent in code rather than a chat tool? [LangChain tools](/docs/langchain) (`langchain-kudosity` on npm) give LangChain.js agents ready-made tools to send SMS, MMS, and RCS — including SMS fallback for RCS sends.

***

## Workflow automation

For CI/CD pipelines that need to send an SMS as part of a workflow (deploy notifications, failure alerts, scheduled reminders), use the [Kudosity SMS GitHub Action](/docs/sms-action). It drops into any GitHub Actions workflow as a single step and posts directly to the V2 SMS API.

***

## Model Context Protocol (MCP)

The [Kudosity MCP server](/docs/mcp) lets any MCP-compatible AI client connect directly to the Kudosity API, including:

* Cursor and other AI-powered development environments
* Custom AI agents using MCP

Install it locally with `npx -y kudosity-mcp` for 19 native messaging tools, or point your client at the hosted server at `developers.kudosity.com/mcp` for zero-install API exploration.

MCP is an open standard that allows AI models to connect to external tools and APIs in a consistent and secure way. By exposing the Kudosity API through MCP, AI agents can discover available messaging capabilities, execute actions like sending messages or managing contacts, and access structured data and workflows.

A single MCP integration works across multiple AI platforms without needing separate implementations.

***

## When to use AI integrations

AI integrations are best suited for:

* Prototyping messaging workflows quickly
* Automating repetitive communication tasks
* Building internal tools powered by AI agents
* Enabling non-technical users to interact with messaging systems
* Accelerating development using AI-assisted tools

***

## How it fits with existing APIs

Kudosity’s AI integrations complement existing REST APIs.

* Use **REST APIs** for deterministic, production-grade integrations
* Use **AI agents** for:
  * Rapid iteration
  * Workflow automation
  * Natural language interaction

Both approaches use the same underlying messaging platform and can be used together depending on your use case.

***

<br />
