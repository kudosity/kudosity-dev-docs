---
title: 'Kudosity for GitHub: Copilot Extension and SMS Action'
author: Tony Becker
hidden: false
published_at: '2026-05-22T05:32:56.645Z'
type: added
---
Kudosity is now available in two new places on GitHub: as a Copilot extension for natural-language messaging from VS Code, and as a GitHub Action for sending an SMS from any CI workflow.

### GitHub Copilot Extension

Send SMS, MMS, manage contact lists, and configure webhooks from GitHub Copilot in VS Code using natural language. The extension uses the Kudosity MCP server for API documentation and your own credentials to execute requests locally — no message data or credentials leave your machine. Requires Copilot Agent Mode.

* Send SMS and MMS from Copilot Chat (Agent Mode)
* Create and manage contact lists for campaign sends
* Configure delivery, inbound, and link-tracking webhooks
* Authenticates via shell environment variables — no keychain or cloud storage

Read the [Copilot Extension docs](https://developers.kudosity.com/docs/copilot-extension) or browse the [GitHub repository](https://github.com/kudosity/kudosity-copilot-extension).

### SMS GitHub Action

Drop into any GitHub Actions workflow to send an SMS as a single step. Now published on the [GitHub Marketplace](https://github.com/marketplace/actions/kudosity-sms). Best for deploy notifications, on-call alerts when builds fail, and scheduled reminders.

* Single-purpose send-SMS via the Kudosity V2 API
* Composite YAML action — no Docker image, no JavaScript runtime
* Repository-secret-based authentication with `::add-mask::` log scrubbing
* Step outputs (`message-id`, `status`) for downstream verification
* Pin to the moving `@v1` tag for patch updates, or to a specific tag or SHA for reproducibility

Read the [SMS Action docs](https://developers.kudosity.com/docs/sms-action) or browse the [GitHub repository](https://github.com/kudosity/kudosity-sms-action).