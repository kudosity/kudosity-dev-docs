---
title: Kudosity MCP Server
deprecated: false
hidden: false
metadata:
  robots: index
---
The Kudosity Model Context Protocol (MCP) server gives AI assistants and agents native tools for messaging — send SMS, MMS, and WhatsApp, manage contacts and webhooks, and check your balance — plus live discovery of the Kudosity API specs. RCS has no send tool of its own; see [Send RCS](#send-rcs) for how an agent sends it.

There are two ways to use it:

|  | Best for | Install |
| --- | --- | --- |
| **Installable server** (`kudosity-mcp` on npm) | Day-to-day use: 19 native tools your agent calls directly, with your API key | `npx -y kudosity-mcp` |
| **Hosted server** (`developers.kudosity.com/mcp`) | Zero-install API exploration: search the specs and guides, look up any endpoint, run ad-hoc requests | Point your client at the URL |

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

The discovery tools read the live Kudosity OpenAPI specs at runtime, so the tools never drift from the documentation. That includes the RCS endpoints: `get_endpoint` returns the current request schema for text, media, rich cards, carousels and suggested replies.

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

* **API discovery** — `list-specs`, `list-endpoints`, `search-endpoints`, `get-endpoint`
* **Documentation** — `search` finds guides and reference pages, and `fetch` returns a page's content
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

## Send RCS

Neither server has a dedicated RCS send tool. Both read the live API spec, so your agent can look up the RCS endpoints and get the current request schema for text, images and video, rich cards, carousels and suggested replies.

| Step | Installable server | Hosted server |
| --- | --- | --- |
| Find the RCS endpoints | `search_endpoints` with the query `rcs` | `search-endpoints` with the pattern `rcs` |
| Get the request schema | `get_endpoint` for `POST /v2/rcs/messages` | `get-endpoint` for `POST /v2/rcs/messages` |
| Send the message | No tool. A coding agent, such as Claude Code or Cursor, can make the request itself | `execute-request`, with your API key in the `x-api-key` header |
| Get delivery events and replies | `create_webhook` with `RCS_STATUS` and `RCS_INBOUND` | `execute-request` on `POST /v2/webhook` |

If your client can't make HTTP requests itself, as with Claude Desktop, add the hosted server to send RCS.

RCS has rules that the other channels don't. Give your agent these, or install the `kudosity-rcs` [Agent Skill](https://developers.kudosity.com/docs/agent-skills), which covers them:

* **The sender is your RCS agent ID, not a phone number.** Until your agent is launched, it can message its test devices only. See [RCS onboarding](https://developers.kudosity.com/docs/rcs-onboarding).
* **The recipient must be in international format**, such as `61438333061`. Local formats, such as `0438 333 061`, are rejected.
* **An SMS fallback needs a message and a sender.** Without `sms_fallback.sender`, the fallback SMS isn't sent.
* **A `200` response means the message was accepted, not delivered.** Subscribe to `RCS_STATUS`, and to `SMS_STATUS` if you set a fallback.
* **A tapped suggestion arrives as an `RCS_INBOUND` event** with `mo.content_type` set to `SUGGESTION`.

The [RCS Message Guide](https://developers.kudosity.com/docs/rcs-message-guide) has an example of every message format. [Track an RCS message that falls back to SMS](https://developers.kudosity.com/docs/rcs-sms-fallback) shows how to tell which channel delivered a message.

## Test your setup

1. **Restart your AI tool** (Claude Desktop, Cursor, etc.)
2. **Start a new chat** with the AI assistant
3. **Try it** — for the installable server: "Check my Kudosity balance" or "Send a test SMS to my number". For the hosted server: "What APIs does Kudosity offer?", "Create a curl command to send my first SMS through Kudosity" or "Show me the request to send an RCS rich card with an SMS fallback"

## Next steps

* [Agent Skills](https://developers.kudosity.com/docs/agent-skills) — API knowledge for coding agents, pairs with these tools
* [LangChain tools](https://developers.kudosity.com/docs/langchain) — the same capability for LangChain.js agents
* [Send messages from AI agents](https://developers.kudosity.com/docs/send-messages-from-ai-agents) — runnable examples for every channel
