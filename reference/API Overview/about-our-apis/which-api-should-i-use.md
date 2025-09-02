---
title: Which API Should I Use?
deprecated: false
hidden: true
metadata:
  robots: index
---
Meet the two ways to send messages on Kudosity. Both run on the same platform and share your account, API keys, senders, reporting, and billing — so you can pick what fits your build today and evolve later.

<Image align="center" src="https://files.readme.io/2e548b11a0ecaa5dec79b3a7994a3696abddddc7d252958a709d5f2caea81e25-kudosity-apis-compare-cards-brand-template-classic.png" />

# Which API should I use?

* Use TransmitMessage (V2) (api.transmitmessage.com) for new builds.
* Choose TransmitSMS (V1) (api.transmitsms.com) when you need a classic-specific feature.

## API Picker

| If you need…                                            | Use                      | Why                                                                            |
| ------------------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------ |
| **MMS via API** (send & receive)                        | **TransmitMessage (V2)** | V2 is the only API with MMS support, including an `inbound_mms` webhook.       |
| **Simple, programmatic sends** with modern webhooks     | **TransmitMessage (V2)** | Webhooks are created/managed by API; JSON responses; streamlined API-key auth. |
| **WhatsApp** (send & receive)                           | **TransmitMessage (V2)** |                                                                                |
| **RCS** (send & receive)                                | **TransmitMessage (V2)** |                                                                                |
| **Track multiple links** in one SMS                     | **TransmitMessage (V2)** | Auto-detect & track multiple links in a message.                               |
| **Your own message reference** alongside our message ID | **TransmitMessage (V2)** | Add a `message_ref` for reconciliation/workflows.                              |
| **Classic integration staying put**                     | **TransmitSMS (V1)**     | Fully supported; shares your senders, reporting, and UI.                       |
| **Single-request multi-recipient sends**                | **TransmitSMS (V1)**     | V1 supports multi-recipient (one call, many numbers).                          |
| **Custom tracked link format/domain**                   | **TransmitSMS (V1)**     | Custom tracked links are available on V1. (V2 roadmap)                         |
| **XML response format**                                 | **TransmitSMS (V1)**     | V1 supports XML or JSON; V2 is JSON-only.                                      |

> Recommendation: Start on V2. Keep V1 if you rely on multi-recipient requests or custom tracked links. You can use both under the same account and senders.

## Feature Highlights (at a glance)

* **Unified account & UI**: Same login, senders, reporting, and billing across both APIs.
* **Auth**: V2 uses API-key auth; V1 uses Basic Auth (key + secret).
* **Webhooks**: V2 webhooks are managed via API (create/list/update/delete). V1 webhooks are configured in the UI.
* **Delivery reports**: V2 via webhook; V1 via webhook or email.
* **Retries**: Both retry failed webhook deliveries; V2 has a more granular schedule.

## Migration guidance

* **New customers**: Build on TransmitMessage (V2) to access MMS, multi-link tracking, message_ref, and API-managed webhooks.
* **Existing V1 customers**: No rush. Continue on TransmitSMS (V1) and migrate when you want MMS, API-managed webhooks, or multiple tracked links.
