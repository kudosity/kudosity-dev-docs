---
title: Which API Should I Use?
deprecated: false
hidden: true
metadata:
  robots: index
---
Meet the two ways to send messages on Kudosity. Both run on the same platform and share your account, API keys, senders, reporting, and billing — so you can pick what fits your build today and evolve later.

# TL;DR — Which API should I use?

Use TransmitMessage (V2) for new builds. Choose TransmitSMS (V1) only when you need a legacy-specific feature.

## API Picker

| If you need…                                            | Use                      | Why                                                                            |
| ------------------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------ |
| **MMS via API** (send & receive)                        | **TransmitMessage (V2)** | V2 is the only API with MMS support, including an `inbound_mms` webhook.       |
| **Simple, programmatic sends** with modern webhooks     | **TransmitMessage (V2)** | Webhooks are created/managed by API; JSON responses; streamlined API-key auth. |
| **Track multiple links** in one SMS                     | **TransmitMessage (V2)** | Auto-detect & track multiple links in a message.                               |
| **Your own message reference** alongside our message ID | **TransmitMessage (V2)** | Add a `message_ref` for reconciliation/workflows.                              |
| **Legacy integration staying put**                      | **TransmitSMS (V1)**     | Fully supported; shares your senders, reporting, and UI.                       |
| **Single-request multi-recipient sends**                | **TransmitSMS (V1)**     | V1 supports multi-recipient (one call, many numbers).                          |
| **Custom tracked link format/domain**                   | **TransmitSMS (V1)**     | Custom tracked links are available on V1. (V2 roadmap)                         |
| **XML response format**                                 | **TransmitSMS (V1)**     | V1 supports XML or JSON; V2 is JSON-only.                                      |

> Recommendation: Start on V2. Keep V1 if you rely on multi-recipient requests or custom tracked links. You can use both under the same account and senders.

<br />
