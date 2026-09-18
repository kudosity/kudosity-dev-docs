---
title: Getting started with AI
deprecated: false
hidden: false
metadata:
  robots: index
---
Use your AI assistant to build messaging integrations with Kudosity. Start by connecting to our MCP server, then check your setup before sending your first message.

## 1. Connect to Kudosity

The Kudosity MCP server gives your assistant tools for messaging and access to the latest API specifications. Follow the recommended setup in the [Kudosity MCP Server guide](https://developers.kudosity.com/docs/mcp), or paste the prompt below into your assistant to get help.

```text
Help me set up the recommended Kudosity MCP server using this guide:
https://developers.kudosity.com/docs/mcp

Reuse an existing Kudosity connection if one is already configured.
Otherwise, guide me through installation and credential setup.
Ask before installing software or changing configuration.

Keep API keys and secrets out of this conversation.
Do not send messages or change account data.
```

Already connected? Continue to **Check your setup**.

## 2. Check your setup

Before sending a message, ask your assistant to check the connection:

```text
Check my Kudosity setup without sending messages or changing account data.

Ask which messaging channel I intend to use. Identify the Kudosity MCP
connection and the documentation or tools available in this environment.

Use the MCP guide's documented read-only checks to verify access
for the API I need. Do not invent a test endpoint or treat access to
public documentation as proof that my credentials work.

Tell me what succeeded, what remains unverified, and any missing
configuration or channel prerequisites. Do not display credentials.
```

The result should distinguish between **access to documentation**, **verified account access**, and **anything still needed for your chosen channel**. Do not assume every channel is ready because one check succeeded.

## Next: Get your first message working

Continue to [Get your first message working with your AI assistant](https://developers.kudosity.com/docs/first-message-with-ai) to prepare a test message, approve the send and check the result.

## Optional resources

### Add API guidance with Agent Skills

[Kudosity Agent Skills](https://developers.kudosity.com/docs/agent-skills) provide instructions about authentication, messaging channels, request formats and webhooks. They complement MCP tools and are not required to complete the setup above.

Check whether your existing integration already includes the relevant skills before adding them separately.

### Explore other integrations

For assistant-specific plugins, framework integrations and workflow automation, see the [AI and agents overview](https://developers.kudosity.com/docs/overview).

Building an agent inside your own application? See [LangChain Tools](https://developers.kudosity.com/docs/langchain) or [Send messages from AI agents & workflows](https://developers.kudosity.com/docs/send-messages-from-ai-agents).
