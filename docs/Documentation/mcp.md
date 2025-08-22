---
title: MCP
deprecated: false
hidden: true
metadata:
  robots: index
---
The Kudosity Model Context Protocol (MCP) server enables AI-powered code editors like Cursor and Windsurf, plus general-purpose tools like Claude Desktop, to interact directly with your Kudosity API and documentation.

## What is MCP?

Model Context Protocol (MCP) is an open standard that allows AI applications to securely access external data sources and tools. The Kudosity MCP server provides AI agents with:

* **Direct API access** to Kudosity functionality
* **Documentation search** capabilities  
* **Real-time data** from your Kudosity account
* **Code generation** assistance for Kudosity integrations
* **Live API execution** - not just answers from memory
* **Always 100% accurate and up to date** - powered by Swagger API source code

## MCP Server Capabilities

The Kudosity MCP server offers comprehensive tools for API discovery, documentation, code generation, and live execution:

### 🔍 API Discovery & Exploration
- **list-specs**: Lists all available OpenAPI specifications (SMS, RCS, WhatsApp, etc.)
- **list-endpoints**: Shows all API paths and HTTP methods for a specific service
- **get-endpoint**: Gets detailed information about a specific API endpoint
- **search-specs**: Searches across all specs for specific patterns or keywords

### 📋 API Schema & Documentation
- **get-request-body**: Retrieves the request body schema for any endpoint
- **get-response-schema**: Gets response schema for specific endpoints and status codes
- **list-security-schemes**: Shows authentication methods and requirements
- **search-documentation**: Searches through API documentation content

### ⚡ Code Generation
- **get-code-snippet**: Generates code examples in various programming languages for any endpoint
- Supports multiple languages (curl, JavaScript, Python, etc.)
- Creates ready-to-use code samples with proper authentication

### 🚀 Live API Execution
- **execute-request**: Actually executes real API calls using HAR format requests
- Send real SMS messages
- Test any Kudosity API endpoint with real credentials
- Returns actual API responses and errors

## Available APIs

The MCP server provides access to these Kudosity APIs:

- **Transmit Message API** - Modern v2 messaging service supporting SMS, RCS, MMS, WhatsApp, and webhooks
- **Transmit SMS API** - Full-featured SMS with advanced capabilities including contact lists, keywords, and reporting
- **Transmit SMS Fast API** - Optimized for quick SMS delivery

## Kudosity MCP Server Setup

Kudosity hosts a remote MCP server at `https://developers.kudosity.com/mcp`. Configure your AI development tools to connect to this server.

### Authentication Configuration

For APIs that require authentication, you'll need to configure your API credentials. The MCP server supports different authentication methods depending on which API endpoints you're using:

- **api.transmitsms.com (v1 endpoints)**: Use **Basic Authentication** with Base64-encoded API Key and API Secret
- **api.transmitmessage.com (v2 endpoints)**: Use **API Key Authentication** with your API Key in the `x-api-key` header

#### For v1 endpoints (api.transmitsms.com) - Basic Authentication:

1. **Get your credentials** from Kudosity dashboard → Developers → API Settings
2. **Combine** as: `API_KEY:API_SECRET`
3. **Base64 encode** using one of these methods:
   - Terminal: `echo -n "API_KEY:API_SECRET" | base64`
   - Browser console: `btoa("API_KEY:API_SECRET")`
   - Online tool: base64encode.org

#### For v2 endpoints (api.transmitmessage.com) - API Key Authentication:

Simply use your API Key directly in the `x-api-key` header (no Base64 encoding required).

<Tabs>
  <Tab title="Claude Desktop (v1 Basic Auth)">
    **For v1 endpoints (api.transmitsms.com) - Add to `claude_desktop_config.json`:**

    ```json
    {
      "mcpServers": {
        "kudosity": {
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

    **Location:**
    - **Mac**: `~/Library/Application Support/Claude/claude_desktop_config.json`
    - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
  </Tab>

  <Tab title="Claude Desktop (v2 API Key)">
    **For v2 endpoints (api.transmitmessage.com) - Add to `claude_desktop_config.json`:**

    ```json
    {
      "mcpServers": {
        "kudosity": {
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

    **Location:**
    - **Mac**: `~/Library/Application Support/Claude/claude_desktop_config.json`
    - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
  </Tab>

  <Tab title="Cursor">
    **Add to `~/.cursor/mcp.json`:**

    ```json
    {
      "mcpServers": {
        "kudosity": {
          "url": "https://developers.kudosity.com/mcp"
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
        "kudosity": {
          "url": "https://developers.kudosity.com/mcp"
        }
      }
    }
    ```
  </Tab>
</Tabs>

**Important**: Restart your AI tool after saving the configuration file.


## Testing Your MCP Setup

Once configured, you can test your MCP server connection:

1. **Restart your AI tool** (Claude Desktop, Cursor, etc.)
2. **Start a new chat** with the AI assistant
3. **Ask about Kudosity** - try questions like:
   * "What APIs does Kudosity offer?"
   * "Show me an example of sending an SMS"
   * "Create a curl command to send my first SMS through Kudosity"

The AI should now have access to your Kudosity account data and documentation through the MCP server.
