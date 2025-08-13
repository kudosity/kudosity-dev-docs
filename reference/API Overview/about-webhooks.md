---
title: About Webhooks
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Kudosity’s Webhooks API enables you to subscribe to key messaging events in real time. To receive events, you must configure a publicly accessible HTTPS endpoint that can accept POST requests. Each event includes an event\_type field to indicate the kind of event and also include a status object with more detailed delivery information.

## Supported `event_type` Values

| Event Type    | Description                                                                 |
| ------------- | --------------------------------------------------------------------------- |
| `LINK_HIT`    | Triggered when a tracked link in a message is clicked                       |
| `OPT_OUT`     | Triggered when a recipient opts out via link or STOP reply                  |
| `MMS_INBOUND` | Triggered when an inbound MMS is received                                   |
| `MMS_STATUS`  | Delivery status updates for MMS messages                                    |
| `SMS_INBOUND` | Triggered when an inbound SMS is received                                   |
| `SMS_STATUS`  | Delivery status updates for SMS messages                                    |
| `RCS_STATUS`  | Delivery status updates for RCS messages (includes extra RCS-specific info) |

### ⚠️ Important: `event_type` Field Deprecation

The `event_type` field is **deprecated** and will be removed in a future version. All new webhook implementations should use the `filter.event_type` array instead.

**Timeline:** Existing webhooks using `event_type` will continue to work, but we recommend migrating to the new filter-based approach as soon as possible.

#### Migration Example

```json
// ❌ Old approach (deprecated)
{
  "event_type": "SMS_STATUS",
  "name": "SMS Status Webhook",
  "url": "https://example.com/webhook"
}

// ✅ New approach (recommended)
{
  "filter": {
    "event_type": ["SMS_STATUS"]
  },
  "name": "SMS Status Webhook",
  "url": "https://example.com/webhook"
}
```

## Status Events

For all status-related events (MMS\_STATUS, SMS\_STATUS, RCS\_STATUS), the payload includes a status object that contains metadata about the message and its delivery state.

### Common Status Values

These values apply to both SMS and MMS messages:

* `SENT`: Message has been submitted to the carrier.
* `DELIVERED`: Message has been delivered to the recipient's handset.
* `FAILED`: Message failed due to an error from the carrier or device.
* `ACCEPTED`: Message was accepted by the carrier; delivery may have been attempted.
* `SOFT_BOUNCE`: Message was undeliverable due to a temporary issue (e.g., handset off or out of range).
* `HARD_BOUNCE`: Message failed due to a permanent issue (e.g., number disconnected).
* `REJECTED`: Message was rejected by Kudosity (e.g., due to compliance validation).
* `OTHER`: A status that does not match any known enum; provided as-is from the carrier.

### RCS Status Events

The RCS\_STATUS event is triggered when the status of an RCS message changes. These updates are sent as a webhook with a status object that includes delivery metadata and a status field indicating the message’s current state.

**Supported RCS Status Values**

| Status      | Description                                                               |
| ----------- | ------------------------------------------------------------------------- |
| `SENT`      | The message has been submitted to the carrier for delivery.               |
| `DELIVERED` | The message has been delivered to the recipient’s device.                 |
| `FAILED`    | Delivery failed due to a carrier or handset error.                        |
| `READ`      | The message was read by the recipient (when supported by device/carrier). |

## Enhanced Filtering Capabilities

The new `filter` object provides powerful filtering beyond the legacy `event_type` field:

### Multiple Event Types

Subscribe to multiple event types in one webhook:

```json
{
  "filter": {
    "event_type": ["SMS_STATUS", "SMS_INBOUND", "OPT_OUT"]
  },
  "name": "Multi-Event Webhook",
  "url": "https://example.com/webhook"
}
```

### Advanced Filtering

Filter by sender, status, message reference, and campaign:

```json
{
  "filter": {
    "event_type": ["SMS_STATUS"],
    "sender": ["+61412345678", "+61487654321"],
    "status": ["DELIVERED", "FAILED"],
    "campaign_id": ["summer-sale"]
  },
  "name": "Filtered SMS Webhook",
  "url": "https://example.com/webhook"
}
```

### Filter Logic

* **Within each filter array**: OR logic (matches any value)
* **Between different filters**: AND logic (must match all conditions)

### Available Filter Fields

* **`event_type`**: Array of event types to subscribe to (replaces the deprecated single `event_type` field)
* **`sender`**: Array of sender addresses to filter by
* **`message_ref`**: Array of message references to filter by
* **`status`**: Array of message statuses to filter by (applies to status events only)
* **`campaign_id`**: Array of campaign IDs to filter by

## Link Hit

The LINK\_HIT event is triggered any time a recipient visits a link that is tracked. Track Links is an optional flag on the send message API calls. Along with the URL that was being tracked is a hits field indicating how many visits this tracked link has in total and a source\_message which contains the track link sent to the recipient.

Example Payloads

```json JSON
{
  "event_type": "LINK_HIT",
  "timestamp": "2021-05-06T05:19:42.123456Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "Link Tracking Webhook",
  "link_hit": {
    "hits": 1,
    "url": "https://www.example.com/abc",
    "source_message": {
      "type": "MMS",
      "id": "b50e4dc1-e57f-459c-a15c-526bee00a4c4",
      "message": "Hey, Check this out! http://clckme.info/KYhSsuIH Opt-out reply STOP",
      "message_ref": "D701",
      "recipient": "61435790000",
      "sender": "61481074191",
      "subject": "Hello",
      "content_urls": [
        "https://res.cloudinary.com/burstsms/image/upload/v1618798563/284KB_qgqtbe.jpg"
      ]
    }
  }
}

```

<br />

```json JSON
{
  "event_type": "LINK_HIT",
  "timestamp": "2021-07-20T23:14:04.789012Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "Link Tracking Webhook",
  "link_hit": {
    "hits": 1,
    "url": "https://www.example.com/abc",
    "source_message": {
      "type": "SMS",
      "id": "faf68308-16cd-4cf9-aef7-47342bd405be",
      "message": "Hey, Check this out! http://clckme.info/KYhSsuIH for Opt-out reply STOP or hit opt out link - http://nsub.me/vqHTcCsh",
      "message_ref": "D301",
      "recipient": "61435795809",
      "sender": "61481074185"
    }
  }
}

```

## Opt Out

The OPT\_OUT event is triggered when a recipient has visited an opt-out link in a message they have received or by sending a message with the text "STOP".

Using parameter \[opt-out-link] in message body, inserts the opt-out link.

The source field will be set according to the method a recipient has used to opt-out and contain a value of either link or SMS.

Example Payloads

```json JSON
{
  "event_type": "OPT_OUT",
  "timestamp": "2021-05-06T05:16:20.456789Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "Opt Out Webhook",
  "opt_out": {
    "source": "LINK_HIT",
    "source_message": {
      "type": "SMS",
      "id": "a51ebe4e-a412-440e-a8d9-464e68a521cc",
      "message": "Hey, Check this out! http://clckme.info/KYhSsuIH for Opt-out reply STOP or hit opt out link - http://nsub.me/vqHTcCsh",
      "message_ref": "ncc5009d",
      "recipient": "61435790000",
      "sender": "61481074190"
    }
  }
}
```

```json JSON
{
  "event_type": "OPT_OUT",
  "timestamp": "2021-05-06T05:16:20.654321Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "Opt Out Webhook",
  "opt_out": {
    "source": "SMS_INBOUND",
    "source_message": {
      "type": "SMS",
      "id": "a51ebe4e-a412-440e-a8d9-464e68a521cc",
      "message": "Hey, Check this out! http://clckme.info/KYhSsuIH for Opt-out reply STOP or hit opt out link - http://nsub.me/vqHTcCsh",
      "message_ref": "ncc5009d",
      "recipient": "61435790000",
      "sender": "61481074190"
    }
  }
}
```

## MMS Inbound

The MMS\_INBOUND event is posted to you on receipt of an MMS sent from a recipient to one of the senders listed on your account. For convenience we will try and find a message that you have sent to this recipient from that sender in the past `72 hours` and supply it as the last\_message field. This is useful for determining if an inbound message is potentially a reply.

```json JSON
{
  "event_type": "MMS_INBOUND",
  "timestamp": "2021-05-06T05:16:33.987654Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "MMS Inbound Webhook",
  "mo": {
    "type": "MMS",
    "id": "alss-2way-605b31c7-d2c49104",
    "sender": "61435790000",
    "recipient": "61481074190",
    "message": "Here's a cute picture",
    "media": [
      {
        "name": "image-0.png",
        "content": "BASE64-encoded-string------"
      }
    ],
    "last_message": {
      "type": "SMS",
      "id": "a51ebe4e-a412-440e-a8d9-464e68a521cc",
      "message": "Hey, check this out!",
      "message_ref": "ncc5009d",
      "recipient": "61435790000",
      "sender": "61481074190"
    }
  }
}
```

## MMS Status

The MMS\_STATUS event data is posted to you for changes to an MMS message status. These are currently only comprised of internal statuses (SENT, FAILED).

```json JSON
{
  "event_type": "MMS_STATUS",
  "timestamp": "2021-05-06T05:19:33.234567Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "MMS Status Webhook",
  "status": {
    "type": "MMS",
    "id": "b50e4dc1-e57f-459c-a15c-526bee00a4c4",
    "message_ref": "D7001",
    "recipient": "61435790000",
    "sender": "61481074191",
    "status": "SENT"
  }
}
```

## SMS Inbound

The SMS\_INBOUND event is posted to you on receipt of an SMS sent from a recipient to one of the senders listed on your account. For convenience we will try and find a message that you have sent to this recipient from that sender and supply it as the last\_message field. This is useful for determining if an inbound message is potentially a reply. The routed\_via field will display when a shared local number has been used to deliver your message.

```json JSON
{
  "event_type": "SMS_INBOUND",
  "timestamp": "2021-05-06T05:16:33.345678Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "SMS Inbound Webhook",
  "mo": {
    "type": "SMS",
    "id": "alss-2way-605b31c7-d2c49104",
    "message": "Stop",
    "recipient": "61481074190",
    "routed_via": "447507333300",
    "sender": "447507222200",
    "last_message": {
      "type": "SMS",
      "id": "a51ebe4e-a412-440e-a8d9-464e68a521cc",
      "message": "Hey, check this out!",
      "message_ref": "ncc5009d",
      "recipient": "447507222200",
      "routed_via": "447507333300",
      "sender": "61481074190"
    }
  }
}

```

## SMS Status

The SMS\_STATUS event data is posted to you for changes to a SMS message status. Multiple status events can be triggered for a single message. The routed\_via field will display when a shared local number has been used to deliver your message.

**SMS\_Status: SENT**

```json JSON
{
  "event_type": "SMS_STATUS",
  "timestamp": "2025-04-23T00:05:18.123456Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "SMS Status Webhook",
  "status": {
    "id": "e48d3e71-264e-4afb-9c06-edec1f0bb9d4",
    "type": "SMS",
    "message_ref": "test V2 QA SMS Send",
    "sender": "447507222200",
    "routed_via": "6140000000",
    "recipient": "6140000000",
    "status": "SENT"
  }
}
```

**SMS\_Status: DELIVERED**

```json JSON
{
  "event_type": "SMS_STATUS",
  "timestamp": "2025-04-23T00:05:18.567890Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "SMS Status Webhook",
  "status": {
    "id": "e48d3e71-264e-4afb-9c06-edec1f0bb9d4",
    "type": "SMS",
    "message_ref": "test V2 QA SMS Send",
    "sender": "447507222200",
    "routed_via": "6140000000",
    "recipient": "6140000000",
    "status": "DELIVERED"
  }
}
```

## RCS Status

Example Payloads

```json
{
  "event_type": "RCS_STATUS",
  "timestamp": "2025-07-01T09:32:10.876543Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "RCS Status Webhook",
  "status": {
    "type": "RCS",
    "id": "bc2e0a4a-7e21-49de-9912-20cb4148c21f",
    "message_ref": "promo-campaign-001",
    "sender": "DemoAgent",
    "recipient": "61400000000",
    "status": "READ"
  }
}
```