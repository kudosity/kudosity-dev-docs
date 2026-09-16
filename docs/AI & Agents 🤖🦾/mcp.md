---
title: Kudosity MCP Server
deprecated: false
hidden: false
metadata:
  robots: index
---
The Kudosity Model Context Protocol (MCP) server gives AI assistants and agents native tools for messaging — send SMS, MMS, and WhatsApp, manage contacts and webhooks, and check your balance — plus live discovery of the Kudosity API specs.

There are two ways to use it:

|  | Best for | Install |
| --- | --- | --- |
| **Installable server** (`kudosity-mcp` on npm) | Day-to-day use: 19 native tools your agent calls directly, with your API key | `npx -y kudosity-mcp` |
| **Hosted server** (`developers.kudosity.com/mcp`) | Zero-install API exploration: search specs, generate code snippets, run ad-hoc requests | Point your client at the URL |

## What is MCP?

Model Context Protocol is an open standard that lets AI applications securely access external tools and data. One MCP integration works across Claude Desktop, Claude Code, Cursor, Windsurf, and any other MCP-compatible client — no per-platform implementation.

## Installable server (recommended)

The open-source [`kudosity-mcp`](https://github.com/kudosity/mcp) package is published on npm and listed on the [official MCP registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.kudosity/mcp) as `com.kudosity/mcp` with DNS-verified domain ownership. It runs locally, with your credentials staying on your machine.

### Tools

| Area | Tools |
| --- | --- |
| Messaging (5) | `send_sms`, `send_mms`, `send_whatsapp`, `get_message`, `list_messages` |
| Contacts & lists (6) | `create_list`, `get_lists`, `get_list`, `add_contact_to_list`, `remove_contact_from_list`, `delete_list` |
| Webhooks (3) | `create_webhook`, `list_webhooks`, `delete_webhook` |
| Account (1) | `get_balance` |
| API discovery (4) | `list_specs`, `list_endpoints`, `search_endpoints`, `get_endpoint` |

The discovery tools read the live Kudosity OpenAPI specs at runtime, so the tools never drift from the documentation.

### Setup

Add to your MCP client configuration (`claude_desktop_config.json`, `~/.cursor/mcp.json`, or equivalent):

```json
{
  "mcpServers": {
    "kudosity": {
      "command": "npx",
      "args": ["-y", "kudosity-mcp"],
      "env": {
        "KUDOSITY_API_KEY": "your-api-key",
        "KUDOSITY_API_SECRET": "your-api-secret"
      }
    }
  }
}
```

Or with Claude Code:

```bash
claude mcp add kudosity -e KUDOSITY_API_KEY=your-api-key -- npx -y kudosity-mcp
```

`KUDOSITY_API_KEY` covers messaging, webhooks, and discovery. Add `KUDOSITY_API_SECRET` for the contact-list and balance tools, which use the V1 API. Both are in the dashboard under **Developers** > **API Settings**.

> 📘 Pair it with Agent Skills
> The MCP server gives your agent tools it can call; [Agent Skills](https://developers.kudosity.com/docs/agent-skills) give it knowledge of how the API behaves. Together, your agent calls the right tool with the right arguments the first time.

## Hosted server

Kudosity also hosts a remote MCP server at `https://developers.kudosity.com/mcp` for zero-install API exploration:

* **API discovery** — `list-specs`, `list-endpoints`, `get-endpoint`, `search-specs`
* **Schema & documentation** — `get-request-body`, `get-response-schema`, `list-security-schemes`, `search-documentation`
* **Code generation** — `get-code-snippet` in curl, JavaScript, Python, and more
* **Live execution** — `execute-request` runs real API calls

### Authentication for live execution

* **api.transmitmessage.com (V2 endpoints)** — your API key in the `x-api-key` header
* **api.transmitsms.com (V1 endpoints)** — HTTP Basic with Base64-encoded `API_KEY:API_SECRET` (`echo -n "API_KEY:API_SECRET" | base64`)

<Tabs>
  <Tab title="Cursor">
    **Add to `~/.cursor/mcp.json`:**

    ```json
    {
      "mcpServers": {
        "kudosity-docs": {
          "url": "https://developers.kudosity.com/mcp"
        }
      }
    }
    ```
  </Tab>

  <Tab title="Claude Desktop (v2 API key)">
    **Add to `claude_desktop_config.json`:**

    ```json
    {
      "mcpServers": {
        "kudosity-docs": {
          "command": "npx",
          "args": [
            "mcp-remote",
            "https://developers.kudosity.com/mcp",
            "--header",
            "x-api-key: ${KUDOSITY_API_KEY}"
          ],
          "env": {
            "KUDOSITY_API_KEY": "YOUR_API_KEY"
          }
        }
      }
    }
    ```
  </Tab>

  <Tab title="Claude Desktop (v1 Basic auth)">
    **Add to `claude_desktop_config.json`:**

    ```json
    {
      "mcpServers": {
        "kudosity-docs": {
          "command": "npx",
          "args": [
            "mcp-remote",
            "https://developers.kudosity.com/mcp",
            "--header",
            "Authorization: Basic ${KUDOSITY_AUTH_HEADER}"
          ],
          "env": {
            "KUDOSITY_AUTH_HEADER": "YOUR_BASE64_ENCODED_CREDENTIALS"
          }
        }
      }
    }
    ```
  </Tab>

  <Tab title="Windsurf">
    **Add to `~/.codeium/windsurf/mcp_config.json`:**

    ```json
    {
      "mcpServers": {
        "kudosity-docs": {
          "url": "https://developers.kudosity.com/mcp"
        }
      }
    }
    ```
  </Tab>
</Tabs>

**Important**: Restart your AI tool after saving the configuration file.

## Test your setup

1. **Restart your AI tool** (Claude Desktop, Cursor, etc.)
2. **Start a new chat** with the AI assistant
3. **Try it** — for the installable server: "Check my Kudosity balance" or "Send a test SMS to my number". For the hosted server: "What APIs does Kudosity offer?" or "Create a curl command to send my first SMS through Kudosity"

## Next steps

* [Agent Skills](https://developers.kudosity.com/docs/agent-skills) — API knowledge for coding agents, pairs with these tools
* [LangChain tools](https://developers.kudosity.com/docs/langchain) — the same capability for LangChain.js agents
* [Send messages from AI agents](https://developers.kudosity.com/docs/send-messages-from-ai-agents) — runnable examples for every channel
