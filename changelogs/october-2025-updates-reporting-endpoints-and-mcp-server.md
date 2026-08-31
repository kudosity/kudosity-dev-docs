---
title: 'October 2025 - 🧠📊 Updates: Reporting Endpoints and MCP Server'
author: Tony Becker
hidden: false
published_at: '2025-10-21T04:07:04.206Z'
type: improved
---
# 🧠 Smart API Version Routing (MCP Update)

We’ve rolled out a major improvement to our developer portal’s MCP server — introducing Smart API Version Routing.

When users or AIs ask general questions like “How do I send an SMS?” or “How do I send an SMS and get delivery reports?”, the system now asks a few short clarifying questions before recommending V1 or V2.

This ensures the assistant consistently provides the correct authentication method and endpoint, removing confusion between similar V1 and V2 functionality.

# 📊 New API Endpoints for RCS and WhatsApp

## Message Retrieval and Statistics APIs

We've introduced comprehensive endpoints for programmatic access to RCS and WhatsApp message data. These APIs allow developers to retrieve individual messages and generate aggregated statistics for reporting and analytics.

### RCS APIs

**Retrieve Individual RCS Messages**

* Endpoint: `GET /v2/rcs/{id}`
* Retrieve detailed information about specific RCS messages — including delivery status, timestamps, content, and pricing

**RCS Aggregated Statistics**

* Endpoint: `GET /v2/rcs/stats`
* Query parameters: `start_at`, `end_at` (ISO 8601), optional `sender` filter
* Returns aggregated counts: sent, delivered, failed, read
* Enables flexible reporting and analytics over any date range

### WhatsApp APIs

**Retrieve Individual WhatsApp Messages**

* Endpoint: `GET /v2/whatsapp/{id}`
* Retrieve detailed information about specific WhatsApp messages — including delivery status, events, and history

**WhatsApp Aggregated Statistics**

* Endpoint: `GET /v2/whatsapp/stats`
* Query parameters: `start_at`, `end_at` (ISO 8601), optional `sender` filter
* Returns aggregated message metrics for analytics and reporting