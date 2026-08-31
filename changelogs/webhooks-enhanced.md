---
title: 10 June 2025 - 🔔 Enhanced Webhooks with Filtering
author: Michael Mallett
hidden: false
published_at: '2025-06-10T07:13:00.000Z'
type: improved
---
We’re introducing enhanced webhook subscriptions that allow for more granular filtering, reducing unnecessary noise and improving third-party integrations.

***

## 🎯 Key Features

When creating a new webhook, you can now provide filters on the following fields:

**Event Type** (e.g. `LINK_HIT`, `SMS_INBOUND`)

**Sender** (e.g. `61400000000`)

**Message Reference** (e.g. `msg_abc`)

**Status** (e.g. `DELIVERED`)

***

## 🧠 Filter Logic

Within each individual filter field, multiple values are combined with **OR** logic.\
**Example:**\
`event_type == LINK_HIT OR event_type == SMS_INBOUND`

Across different filter fields, conditions are combined with **AND** logic.\
**Example:**\
`(event_type == LINK_HIT OR SMS_INBOUND) AND sender == 61400000000`

### 🔍 Context-Sensitive Filtering

The filters are applied differently based on the event type:

* For **status events** (`SMS_STATUS`, `MMS_STATUS`, etc.):\
  `sender`, `status`, and `message_ref` filters are applied to the **status** payload.

* For **LINK\_HIT** events:\
  `sender` and `message_ref` filters are applied to the **source\_message** in the link hit payload.

* For **OPT\_OUT** events:\
  `sender` and `message_ref` filters are applied to the **source\_message** in the opt-out payload.

* For **inbound events** (`SMS_INBOUND`, `MMS_INBOUND`, etc.):\
  `sender` and `message_ref` filters are applied to the **last\_message** in the inbound payload.

***

## 🚨 Legacy Support

You can still use the existing webhook creation format for now, but it will be deprecated in a future release.

***

## 🆕 New Webhook Request Format

```json
{
  "name": "Webhook test name",
  "url": "https://webhook.site/test",
  "rate_limit": 100,
  "filter": {
    "event_type": ["LINK_HIT"],
    "sender": ["61400000000"],
    "message_ref": ["msg_abc", "message_cdf"],
    "status": ["DELIVERED"]
  }
}
```

***

## ✅ New Webhook Response Format

```json
{
  "id": "23f4692f-34e0-4931-90b9-50b5a527495a",
  "name": "My Webhook",
  "url": "https://sms-kudosity-webpage-example.glitch.me/message",
  "rate_limit": 0,
  "filter": {
    "event_type": ["LINK_HIT"],
    "sender": ["61412345678"],
    "status": ["DELIVERED"],
    "message_ref": ["msg_xyz789"]
  },
  "created_at": "2024-11-19T05:55:06.810258960Z",
  "updated_at": "2024-11-19T05:55:06.810258960Z"
}
```

***

## 🛠 Minor Changes

### 📅 Timestamp Format Update

We are slightly modifying the datetime format used in the webhook API.

| Old Format                       | New Format                    |
| -------------------------------- | ----------------------------- |
| `2025-05-14T01:05:15.728779000Z` | `2025-05-14T01:05:15.728779Z` |

**Why this matters:**\
Both formats conform to RFC 3339, a subset of ISO 8601.\
This change removes nanosecond precision (9 digits) in favor of microsecond precision (6 digits).\
Most datetime parsers will handle this gracefully unless you are doing exact string matching or have schema constraints.

***

## 🆕 `webhook_id` and `webhook_name` in Payloads

We’ve added two new fields to webhook payloads for better traceability:

```json
{
  "event_type": "WHATSAPP_STATUS",
  "timestamp": "2025-05-02T01:57:54Z",
  "webhook_id": "21153c94-4e75-4c79-bed4-1398f8c00e8b",
  "webhook_name": "PJ WA Status",
  "status": {
    "id": "f22625c4-647f-4e32-86af-9eed4bc064e7",
    "type": "WHATSAPP",
    "message_ref": "optional-ref-12",
    "sender": "61423411111",
    "recipient": "61457577729",
    "status": "SUBMITTED"
  }
}
```

These are non-breaking additions and should not affect existing integrations unless strict schema validation is enforced.

***

## 💬 Questions or Concerns?

If you anticipate issues (e.g., due to schema validation), please contact us — we're happy to work with you on a solution.