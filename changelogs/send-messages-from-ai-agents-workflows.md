---
title: Send messages from AI agents & workflows
author: Tony Becker
hidden: false
published_at: '2026-06-03T01:06:00.768Z'
type: added
---
We've published a new guide and an open-source examples repo for using Kudosity inside AI agents and AI workflows.

* **New guide:** [Send messages from AI agents & workflows](https://developers.kudosity.com/docs/send-messages-from-ai-agents) — the pattern for turning an AI agent decision into a real SMS, MMS, WhatsApp, or RCS message.
* **New examples repo:** [github.com/kudosity/ai-agent-examples](https://github.com/kudosity/ai-agent-examples) — small, copy-paste-ready Node.js examples (no dependencies) for every channel:
  * Send SMS from an AI agent / workflow
  * Lead-scoring SMS follow-up, appointment reminders
  * AI-generated MMS campaigns
  * WhatsApp templates (order updates, payment reminders)
  * RCS with automatic SMS fallback (delivery updates, payment reminders)
  * Using Kudosity as an MCP tool inside Claude, Cursor, and Windsurf

Each example keeps the API call visible so you can drop it straight into your own agent or workflow. Base URL `https://api.transmitmessage.com`, auth via the `x-api-key` header.