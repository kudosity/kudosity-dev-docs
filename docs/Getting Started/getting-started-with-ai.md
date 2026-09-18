---
title: Getting started with AI
deprecated: false
hidden: false
metadata:
  robots: index
---
Connect your AI assistant to Kudosity to explore our APIs and build messaging integrations. Choose the setup for the assistant you already use, configure access, and check your connection before sending your first message.

## 1. Choose your AI assistant

Start with the guide for your assistant:

| Your assistant | Setup guide |
| --- | --- |
| Claude Code | [Claude Code Plugin](https://developers.kudosity.com/docs/claude-plugin) |
| Gemini CLI | [Gemini CLI Extension](https://developers.kudosity.com/docs/gemini-extension) |
| GitHub Copilot in VS Code | [GitHub Copilot Extension](https://developers.kudosity.com/docs/copilot-extension) - requires Agent Mode to run API calls |
| OpenClaw | [OpenClaw Plugin](https://developers.kudosity.com/docs/openclaw-plugin) - SMS integration |
| Cursor, Claude Desktop, Windsurf, or another client that supports local MCP servers | [Kudosity MCP Server](https://developers.kudosity.com/docs/mcp) - follow the installable-server setup |

Choose one starting route. Follow that guide before adding other integrations, and check its supported operations for the channel you plan to use.

Already connected? Skip to [Check your setup](#4-check-your-setup).

## 2. Install and configure your integration

Follow your chosen guide to install the integration and configure access. To run operations against your account, you will need a Kudosity account and the API credentials required by that integration. Some operations also require an API secret.

Enter credentials through the setup method described in the guide. Do not paste API keys or secrets into an AI conversation or commit them to source control.

To ask your assistant for help, replace the placeholder below with your chosen setup guide's URL:

```text
Help me set up Kudosity using this official guide:
[Paste the setup-guide URL here.]

Check whether Kudosity is already installed and configured.
Reuse a working setup rather than adding a duplicate.

Follow the documented steps. Ask before installing software
or changing configuration. Guide me to enter credentials through
the documented setup method without showing their values in chat.

If instructions are missing or unsupported in this environment,
explain the blocker rather than inventing a configuration.

Do not send messages or change data in my Kudosity account.
```

## 3. Give your assistant the API guidance it needs

### For AI coding assistants

[Kudosity Agent Skills](https://developers.kudosity.com/docs/agent-skills) provide instructions about authentication, messaging channels, request formats and webhooks. They complement MCP tools by explaining how to use the APIs.

Check whether your integration already includes the relevant skills. For a supported coding assistant that does not have them, install them with:

```bash
npx skills add kudosity/skills
```

Follow the Agent Skills guide for supported assistants and installation details. Installing skills does not replace credential configuration.

### For documentation-only help

You can also give your assistant a relevant developer guide or API reference without connecting your account. The [documentation index](https://developers.kudosity.com/llms.txt) helps it find the appropriate pages.

For example:

```text
Use the Kudosity documentation index:
https://developers.kudosity.com/llms.txt

Find the official guide for sending an SMS and explain the required
credentials and request fields. Base your answer on the documentation.
Do not execute any API requests.

If you cannot access the documentation, ask me to provide the page.
```

Reading documentation is separate from authenticating with your Kudosity account.

## 4. Check your setup

Before sending a message, ask your assistant to check the integration:

```text
Check my Kudosity setup without sending messages or changing account data.

Ask which messaging channel I intend to use. Identify the Kudosity
integration and the documentation or tools available in this environment.

Use the setup guide's documented read-only checks to verify access
for the API I need. Do not invent a test endpoint or treat access to
public documentation as proof that my credentials work.

Tell me what succeeded, what remains unverified, and any missing
configuration or channel prerequisites. Do not display credentials.
```

The result should distinguish between access to documentation, verified account access, and anything still needed for your chosen channel. Do not assume every channel is ready because one check succeeded.

## Next: Send your first message

Continue to **Get your first message working with your AI assistant** to prepare a test message, approve the send and check the result.

<!-- Add the link above when the first-message guide is published. -->

Building an agent inside your own application instead? See [LangChain Tools](https://developers.kudosity.com/docs/langchain) or [Send messages from AI agents & workflows](https://developers.kudosity.com/docs/send-messages-from-ai-agents).
