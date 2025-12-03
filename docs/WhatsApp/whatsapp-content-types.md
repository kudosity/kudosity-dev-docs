---
title: WhatsApp Content Types
deprecated: false
hidden: true
metadata:
  robots: index
---
Kudosity's WhatsApp API supports three content types for sending messages. Each type serves different business needs, has different capabilities, and follows specific WhatsApp requirements.

The available content types are:

* **`text`** – Free-form conversational messages within the 24-hour support window
* **`template`** – Pre-approved WhatsApp templates with dynamic placeholders
* **`custom`** – Rich-media and interactive template messages (images, video, carousels, buttons)

***

## Content Type Comparison

| Feature                    | `text`                 | `template` | `custom`           |
| -------------------------- | ---------------------- | ---------- | ------------------ |
| Pre-approval required      | No                     | Yes        | Yes                |
| Can initiate conversations | No                     | Yes        | Yes                |
| 24-hour window required    | Yes                    | No         | No                 |
| Dynamic parameters         | No                     | Yes (text) | Yes (text + media) |
| Media support              | Basic attachments only | No         | Yes                |
| Carousel support           | No                     | No         | Yes                |
| Interactive buttons        | No                     | Limited    | Full               |
| Complexity                 | Simple                 | Medium     | Advanced           |

***

# 1. `content_type: "text"`

Free-form text messages used for real-time support and customer service conversations.  
These can only be sent **within 24 hours** of the user's last message.

### When to Use

* Responding to customer questions
* Customer support and troubleshooting
* Follow-ups inside an active conversation
* Any user-initiated chat session

### Key Characteristics

* No approval required
* Cannot start new conversations
* User must send the first message
* Supports simple text and basic media attachments

### Example

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "text",
  "content": {
    "text": {
      "message": "Thanks for your message! Your refund has been processed."
    }
  },
  "message_ref": "support-reply-001"
}
```

***

# 2. `content_type: "template"`

Template messages use **pre-approved** WhatsApp templates.  
Templates allow structured text with dynamic placeholders such as `{{1}}` and `{{2}}`.

Template messages **can initiate conversations** and are not restricted by the 24-hour rule.

### When to Use

* Order updates
* Appointment reminders
* Delivery notifications
* Account or payment alerts
* Any proactive messaging

### Key Characteristics

* Requires WhatsApp approval
* Supports dynamic text parameters
* Cannot include media content (use `custom` for media)
* Ideal for transactional and informational notifications

### Example

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "template",
  "content": {
    "template": {
      "name": "order_status_update",
      "parameters": ["John", "ORD-98765", "Shipped"],
      "locale": "en_US"
    }
  },
  "message_ref": "order-update-123"
}
```

***

# 3. `content_type: "custom"`

Custom messages support **rich media**, **buttons**, and **interactive components**, following Meta's Cloud API component schema.  
Use this type when sending advanced templates such as product showcases, carousels, or media-driven promotions.

### When to Use

* Product catalogs or multi-product carousels
* Media-rich promotional campaigns
* Interactive customer journeys
* Templates with images, videos, or documents
* Buttons, call-to-action, or quick replies

### Key Characteristics

* Requires WhatsApp approval
* Supports images, video, documents, and GIFs
* Supports advanced interactive components
* Most flexible and expressive format

### Example

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "custom",
  "content": {
    "custom": {
      "type": "template",
      "template": {
        "name": "product_showcase_image",
        "language": {
          "code": "en_US",
          "policy": "deterministic"
        },
        "components": [
          {
            "type": "HEADER",
            "parameters": [
              {
                "type": "image",
                "image": { "link": "https://example.com/product.jpg" }
              }
            ]
          },
          {
            "type": "BODY",
            "parameters": [
              { "type": "text", "text": "Premium Wireless Headphones" },
              { "type": "text", "text": "$299.99" }
            ]
          }
        ]
      }
    }
  },
  "message_ref": "product-showcase-001"
}
```

***

# Required Fields for All WhatsApp Messages

All messages—regardless of content type—must include the following fields:

| Field          | Description                                      |
| -------------- | ------------------------------------------------ |
| `sender`       | Your registered WhatsApp Business number (E.164) |
| `recipient`    | The user's WhatsApp number (E.164)               |
| `content_type` | One of: `text`, `template`, `custom`             |
| `content`      | Message content object for the selected type     |
| `message_ref`  | Unique reference ID for tracking (max 500 chars) |

### Optional Fields

* `sms_fallback`: SMS text to send if WhatsApp delivery fails
