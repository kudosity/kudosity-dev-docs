---
title: 2 July 2025 - RCS API Launch
author: Tony Becker
hidden: false
published_at: '2025-07-02T05:55:36.038Z'
type: added
---
We've launched Rich Communication Services (RCS) messaging with intelligent SMS fallback and enhanced delivery tracking.

***

## ✅ What's New

**POST[/v2/rcs](/reference/post_v2-rcs)**\
A powerful new API endpoint for sending rich, interactive messages via RCS with automatic SMS fallback capabilities.

**Key features:**

* **Dual Message Types:**\
  Send Basic RCS messages (160 characters, SMS format) or Simple RCS messages (3072 characters with full UTF-8 and emoji support).

* **RCS Agent-Based Delivery:**\
  Messages are sent through registered RCS Agent IDs rather than phone numbers, providing better brand identity and compliance with RCS standards.

* **Intelligent SMS Fallback (optional):**\
  Include an SMS fallback message that automatically delivers if the recipient's device doesn't support RCS or if carrier delivery fails.

* **Enhanced Webhook Support:**\
  Track message delivery with RCS-specific webhook events including `RCS_STATUS` with statuses: `SENT`, `DELIVERED`, `FAILED`, and `READ` (unique to RCS).

* **Message Tracking via message\_ref:**\
  Tag each message with a custom `message_ref`—this ID will be included in delivery webhooks, making it easy to track message delivery status in your systems.

* **Traffic Type Classification:**\
  Specify message traffic types (`authentication`, `transaction`, `promotion`, `serviceRequest`, `acknowledgement`) for better carrier routing and compliance.

**Clean and Compliant Formatting:**

* All phone numbers must be in E.164 international format.
* RCS Agent IDs must be pre-registered with your account.
* Currently available for Australian recipients.

***

## 🎯 Key Benefits

* **Rich Messaging:** Send longer messages with emoji and UTF-8 support
* **Read Receipts:** Know when recipients actually read your messages (RCS-specific feature)
* **Intelligent Fallback:** Ensure message delivery even to non-RCS devices
* **Brand Identity:** Use registered RCS Agent names instead of phone numbers

***

## 🔧 Technical Details

### Request Format

```json
{
  "sender": "DemoSender",
  "recipient": "+447903749662",
  "content_type": "text",
  "content": {
    "text": {
      "message": "Hi Michael,\nJust a quick reminder. You are amazing!!\nReply STOP to opt out."
    }
  },
  "sms_fallback": {
    "message": "Hi Michael, just a quick reminder. You are amazing!! Reply STOP to opt out."
  },
  "message_ref": "order-12345"
}
```

### Response Format

```json
{
  "data": {
    "id": "6fdae71c-dad7-4c36-9734-a69693ecf3b4",
    "message_ref": "order-12345",
    "sender": "DemoSender",
    "recipient": "+447903749662",
    "message_traffic_type": "default",
    "content_type": "text",
    "content": {
      "text": {
        "message": "Hi Michael,\nJust a quick reminder. You are amazing!!\nReply STOP to opt out."
      }
    },
    "created_at": "2025-01-07T05:16:07Z"
  }
}
```

### Webhook Support

RCS webhooks include the new `RCS_STATUS` event type with enhanced status tracking:

```json
{
  "event_type": "RCS_STATUS",
  "timestamp": "2025-01-07T05:16:07Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "RCS Status Webhook",
  "status": {
    "type": "RCS",
    "id": "6fdae71c-dad7-4c36-9734-a69693ecf3b4",
    "message_ref": "order-12345",
    "recipient": "+447903749662",
    "sender": "DemoSender",
    "status": "READ"
  }
}
```

***

## 🚀 Getting Started

1. **Contact Support:** Reach out to register your RCS Agent ID
2. **API Integration:** Use the new `/v2/rcs` endpoint
3. **Webhook Setup:** Configure webhooks to track `RCS_STATUS` events
4. **Test Messages:** Start with Basic RCS messages, then explore Simple RCS features

***

## 📋 Message Types Comparison

| Feature             | Basic RCS      | Simple RCS       | SMS Fallback   |
| ------------------- | -------------- | ---------------- | -------------- |
| **Character Limit** | 160 characters | 3,072 characters | 160 characters |
| **UTF-8 Support**   | ✅              | ✅                | ✅              |
| **Emoji Support**   | ✅              | ✅                | ✅              |
| **Read Receipts**   | ✅              | ✅                | ❌              |
| **Rich Formatting** | Limited        | ✅                | ❌              |

***

## 💬 Questions or Need Help?

Ready to start sending RCS messages? Contact our support team to get your RCS Agent registered and begin integrating this powerful new messaging channel into your applications.