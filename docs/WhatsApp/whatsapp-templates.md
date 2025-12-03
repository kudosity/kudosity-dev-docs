---
title: WhatsApp Content Types
deprecated: false
hidden: true
metadata:
  title: WhatsApp Templates Overview - Kudosity API
  description: >-
    Learn how to create and send WhatsApp templates using Kudosity API. Covers
    text, template, and custom content types, plus Marketing, Utility,
    Authentication, and Service message categories.
  robots: index
next:
  description: ''
---
Kudosity's WhatsApp service supports three primary types of message `content_type`s for business messages:

* **text** - Free-form messages sent within the 24-hour customer service window. These are conversational messages, or back and forth replies, to an original business message.
* **templates** - Cornerstones of WhatsApp business communications. Templates allow you to send structured business messages to customers. They need to be pre-approved by Meta, which takes seconds, and adhere to Meta message requirements. There's 4 types of templates, see [WhatsApp Message Types](https://developers.kudosity.com/docs/introduction-to-whatsapp#/whatsapp-message-types).
* **custom** - Custom content types support rich media, carousels, images, interactive buttons, and [WhatsApp Flows](https://developers.facebook.com/docs/whatsapp/flows/).

***

## Content Types

Kudosity's WhatsApp API supports three distinct content types for sending messages. Each content type serves different messaging needs and has specific use cases and requirements.

### 1. Text type

`content_type: "text"`

**Free-form text messages** sent within the 24-hour customer service window. These are conversational messages that can only be sent in response to a user-initiated conversation.

**Key Characteristics:**

* ✅ No pre-approval required
* ✅ Free-form message content
* ✅ Can only be sent within 24 hours of last user message
* ❌ Cannot initiate conversations
* ❌ Not suitable for proactive messaging

**When to Use:**

* Responding to customer inquiries
* Customer service conversations
* Follow-up messages within active conversations
* Real-time support interactions

**Important Limitations:**

* **24-Hour Window**: Text messages can only be sent within 24 hours of the last message received from the user
* **User-Initiated**: Cannot be used to start new conversations
* **No Templates**: Does not use pre-approved templates

**Example Payload:**

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "text",
  "content": {
    "text": {
      "message": "Thank you for your inquiry! Your order #12345 has been shipped and will arrive in 2-3 business days."
    }
  },
  "message_ref": "ref-customer-response-123"
}
```

**Example Use Case:**

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "text",
  "content": {
    "text": {
      "message": "Hi! I've checked your account and your refund of $49.99 has been processed. You should see it in your account within 3-5 business days. Is there anything else I can help you with?"
    }
  },
  "message_ref": "ref-support-response"
}
```

***

### 2. Template type

`content_type: "template"`

**Text-based templates with dynamic parameters** that must be pre-approved by WhatsApp. These templates allow you to send structured messages with variable content (like names, order numbers, dates) outside the 24-hour window.

**Key Characteristics:**

* ✅ Can initiate conversations
* ✅ No 24-hour window restriction
* ✅ Supports dynamic text parameters
* ✅ Pre-approved by WhatsApp
* ❌ Text-only (no media in header)
* ❌ Requires template creation and approval

**When to Use:**

* Order confirmations and updates
* Appointment reminders
* Delivery notifications
* Account alerts
* Transactional messages
* Proactive customer notifications

**Template Structure:**
Templates use placeholders like `{{1}}`, `{{2}}`, etc., which are replaced with the values you provide in the `parameters` array.

**Example Template in WhatsApp Manager:**

```
Hi {{1}}, your order {{2}} was delivered successfully.

You can manage your order below.

[Button: Manage Order]
```

**Example Payload:**

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "template",
  "content": {
    "template": {
      "name": "delivery_confirmation_1",
      "parameters": ["John Smith", "ORD-2024-12345"],
      "locale": "en_US"
    }
  },
  "message_ref": "ref-delivery-confirmation"
}
```

**Result Sent to User:**

```
Hi John Smith, your order ORD-2024-12345 was delivered successfully.

You can manage your order below.

[Manage Order]
```

**Additional Examples:**

**Appointment Reminder:**

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "template",
  "content": {
    "template": {
      "name": "appointment_reminder",
      "parameters": ["Dr. Sarah Johnson", "January 25, 2025", "2:30 PM"],
      "locale": "en_US"
    }
  },
  "message_ref": "ref-appointment-reminder"
}
```

**Order Status Update:**

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "template",
  "content": {
    "template": {
      "name": "order_status_update",
      "parameters": ["Sarah", "ORD-98765", "Out for Delivery"],
      "locale": "en_US"
    }
  },
  "message_ref": "ref-order-update"
}
```

***

### 3. Custom type

`content_type: "custom"`

**Advanced templates with rich media and interactive components** following Meta's Cloud API format. This content type is used for templates that include images, videos, documents, carousels, or complex button configurations.

**Key Characteristics:**

* ✅ Supports rich media (images, videos, documents)
* ✅ Carousel templates (multiple cards)
* ✅ Advanced button configurations
* ✅ Complex interactive elements
* ✅ Can initiate conversations
* ✅ No 24-hour window restriction
* ❌ Requires template pre-approval
* ❌ More complex payload structure

**When to Use:**

* Product catalogs and showcases
* Marketing campaigns with visuals
* Multi-product carousels
* Templates with dynamic media
* Interactive surveys or forms
* Rich promotional content

**Media Types Supported:**

* **Images**: JPG, PNG (max 5 MB)
* **Videos**: MP4 (max 16 MB)
* **Documents**: PDF (max 100 MB)
* **GIFs**: Animated GIFs (max 8 MB)

**Example: Template with Image Header**

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
                "image": {
                  "link": "https://storage.googleapis.com/products/premium-headphones.jpg"
                }
              }
            ]
          },
          {
            "type": "BODY",
            "parameters": [
              {
                "type": "text",
                "text": "Premium Wireless Headphones"
              },
              {
                "type": "text",
                "text": "$299.99"
              }
            ]
          }
        ]
      }
    }
  },
  "message_ref": "ref-product-showcase"
}
```

**Example: Carousel Template (4 Cards)**

```json
{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "custom",
  "content": {
    "custom": {
      "type": "template",
      "template": {
        "name": "kudosity_whatsapp_partnership_carousel",
        "language": {
          "code": "en_US",
          "policy": "deterministic"
        },
        "components": [
          {
            "type": "body",
            "parameters": []
          },
          {
            "type": "carousel",
            "cards": [
              {
                "card_index": "0",
                "components": [
                  {
                    "type": "header",
                    "parameters": [
                      {
                        "type": "video",
                        "video": {
                          "link": "https://storage.googleapis.com/waba-images/CAROUSEL_1.mp4"
                        }
                      }
                    ]
                  },
                  {
                    "type": "body",
                    "parameters": []
                  },
                  {
                    "type": "button",
                    "index": "0",
                    "sub_type": "quick_reply"
                  },
                  {
                    "type": "button",
                    "index": "1",
                    "sub_type": "URL",
                    "parameters": [
                      {
                        "type": "text",
                        "text": "https://kudosity.com/products/messaging/whatsapp"
                      }
                    ]
                  }
                ]
              },
              {
                "card_index": "1",
                "components": [
                  {
                    "type": "header",
                    "parameters": [
                      {
                        "type": "video",
                        "video": {
                          "link": "https://storage.googleapis.com/waba-images/CAROUSEL_2.mp4"
                        }
                      }
                    ]
                  },
                  {
                    "type": "body",
                    "parameters": []
                  },
                  {
                    "type": "button",
                    "index": "0",
                    "sub_type": "quick_reply"
                  },
                  {
                    "type": "button",
                    "index": "1",
                    "sub_type": "URL",
                    "parameters": [
                      {
                        "type": "text",
                        "text": "https://kudosity.com/products/messaging/whatsapp"
                      }
                    ]
                  }
                ]
              },
              {
                "card_index": "2",
                "components": [
                  {
                    "type": "header",
                    "parameters": [
                      {
                        "type": "video",
                        "video": {
                          "link": "https://storage.googleapis.com/waba-images/CAROUSEL_3.mp4"
                        }
                      }
                    ]
                  },
                  {
                    "type": "body",
                    "parameters": []
                  },
                  {
                    "type": "button",
                    "index": "0",
                    "sub_type": "quick_reply"
                  },
                  {
                    "type": "button",
                    "index": "1",
                    "sub_type": "URL",
                    "parameters": [
                      {
                        "type": "text",
                        "text": "https://kudosity.com/products/messaging/whatsapp"
                      }
                    ]
                  }
                ]
              },
              {
                "card_index": "3",
                "components": [
                  {
                    "type": "header",
                    "parameters": [
                      {
                        "type": "video",
                        "video": {
                          "link": "https://storage.googleapis.com/waba-images/CAROUSEL_4.mp4"
                        }
                      }
                    ]
                  },
                  {
                    "type": "body",
                    "parameters": []
                  },
                  {
                    "type": "button",
                    "index": "0",
                    "sub_type": "quick_reply"
                  },
                  {
                    "type": "button",
                    "index": "1",
                    "sub_type": "URL",
                    "parameters": [
                      {
                        "type": "text",
                        "text": "https://kudosity.com/products/messaging/whatsapp"
                      }
                    ]
                  }
                ]
              }
            ]
          }
        ]
      }
    }
  },
  "message_ref": "ref-carousel-message"
}
```

***

## Content Type Comparison

| Feature                        | Text                       | Template                    | Custom                 |
| ------------------------------ | -------------------------- | --------------------------- | ---------------------- |
| **Pre-approval Required**      | ❌ No                       | ✅ Yes                       | ✅ Yes                  |
| **24-Hour Window**             | ✅ Required                 | ❌ Not required              | ❌ Not required         |
| **Can Initiate Conversations** | ❌ No                       | ✅ Yes                       | ✅ Yes                  |
| **Dynamic Parameters**         | ❌ No                       | ✅ Yes (text only)           | ✅ Yes (text + media)   |
| **Media Support**              | ❌ No                       | ❌ No                        | ✅ Yes                  |
| **Carousels**                  | ❌ No                       | ❌ No                        | ✅ Yes                  |
| **Interactive Buttons**        | ❌ No                       | ✅ Limited                   | ✅ Advanced             |
| **Use Case**                   | Customer service responses | Transactional notifications | Marketing & rich media |
| **Approval Time**              | Instant                    | 24-48 hours                 | 24-48 hours            |
| **Complexity**                 | Simple                     | Medium                      | Advanced               |

***

## WhatsApp Message Categories

WhatsApp classifies template messages into four distinct categories based on their purpose and use case. Understanding these categories is essential for proper template creation, approval, and billing.

### Category Overview

| Category           | Purpose               | Approval Required | 24-Hour Window | Example Use Cases                                    |
| ------------------ | --------------------- | ----------------- | -------------- | ---------------------------------------------------- |
| **Marketing**      | Promotional content   | ✅ Yes             | ❌ Not required | Product launches, special offers, newsletters        |
| **Utility**        | Transactional updates | ✅ Yes             | ❌ Not required | Order updates, payment confirmations, account alerts |
| **Authentication** | Security verification | ✅ Yes             | ❌ Not required | OTP codes, login verification, password reset        |
| **Service**        | Customer support      | ❌ No              | ✅ Required     | Support responses, inquiries, follow-ups             |

***

### 1. Marketing Templates

**Purpose**: Business-initiated promotional communications to users who have opted in.

**When to Use**:

* Product announcements and launches
* Special offers and promotions
* Seasonal campaigns
* Newsletter content
* Abandoned cart reminders
* Customer re-engagement

**Available Template Formats**:

* Text and rich media (images, videos)
* Carousel (up to 10 cards)
* Limited-time offer
* Coupon code
* Flow templates
* Multi-product (API only)
* Catalog (API only)

**Examples**:

```
Thank you for your order! Use code PROMO25 for 25% off your next purchase!

Hello! Welcome to our WhatsApp channel. Stay tuned for exclusive offers.

Here are this month's featured products - happy shopping!

You left items in your cart! Complete your purchase now and get 10% off.
```

**Important Notes**:

* ⚠️ **Any template containing both utility and marketing content is classified as marketing**
* ⚠️ Marketing templates are subject to stricter quality ratings
* ⚠️ Must include opt-out language for promotional content
* ⚠️ Higher cost per conversation compared to utility

***

### 2. Utility Templates

**Purpose**: Facilitate business-initiated conversations related to specific transactions, accounts, or ongoing interactions.

**When to Use**:

* Post-purchase notifications
* Order and shipping updates
* Payment reminders and receipts
* Appointment confirmations
* Account status changes
* Subscription renewals
* Service alerts

**Available Template Formats**:

* Text and rich media
* Carousel
* Flow templates

**Key Requirements**:

* ✅ Must relate to a specific, active transaction or account
* ✅ Must include transaction/account details
* ✅ Should be event-triggered (not promotional)

**Examples**:

```
Hi {{1}}, your order {{2}} was delivered successfully. 
You can manage your order below.

Your payment of ${{1}} is due on {{2}}. Pay now to avoid late fees.

Reminder: Your appointment with {{1}} is scheduled for {{2}} at {{3}}.

Your subscription will renew on {{1}} for ${{2}}.
```

**Important Notes**:

* ⚠️ **Mixed content rule**: If a template contains both utility and marketing elements, it will be classified and charged as a marketing template
* ✅ Lower cost per conversation compared to marketing
* ✅ Generally higher approval rates

***

### 3. Authentication Templates

**Purpose**: Secure user authentication through one-time passcodes at various stages of the login process.

**When to Use**:

* Account registration
* Login verification
* Password reset
* Two-factor authentication (2FA)
* Security checks
* Account recovery

**Template Structure** (Predefined by Meta):

Authentication templates follow a strict format:

1. **Verification code** (required): `{{1}} is your verification code.`
2. **Security disclaimer** (optional): `For your security, do not share this code.`
3. **Expiration warning** (optional): `This code expires in {{2}} minutes.`
4. **Button** (optional): Copy code or one-tap autofill

**Example**:

```
123456 is your verification code.

For your security, do not share this code.

This code expires in 10 minutes.

[Copy Code Button]
```

**Strict Restrictions**:

* ❌ No URLs allowed
* ❌ No media (images, videos, documents)
* ❌ No emojis
* ✅ Verification codes: Maximum 15 characters
* ✅ Expiration time: 1-10 minutes (configurable)

**Validity Period**:

* Configurable delivery window: 1-10 minutes
* If delivery fails within this period (user offline, device off), message is dropped
* No charges apply for undelivered authentication messages
* Default: 24-hour delivery window (set value to `-1`)

**Important Notes**:

* ✅ Lowest cost per conversation
* ✅ Fastest approval process
* ⚠️ One-tap autofill only available on Android devices

***

### 4. Service (Free-Form Messages)

**Purpose**: Real-time customer service conversations within an active messaging session.

**When to Use**:

* Responding to customer inquiries
* Live chat support
* Follow-up questions within 24-hour window
* Personalized assistance
* Problem resolution

**Key Characteristics**:

* ❌ No pre-approval required
* ✅ Can only be sent within 24 hours of user's last message
* ✅ Supports all media types
* ✅ Free-form content (no template restrictions)

**Examples**:

```
Thank you for contacting us! How can I help you today?

I've checked your account and your refund of $49.99 has been processed. 
You should see it in 3-5 business days.

I understand your concern. Let me look into this for you right away.
```

**Important Notes**:

* ⚠️ **24-hour window is strict**: Messages sent outside this window will be rejected
* ⚠️ Cannot initiate conversations - user must message first
* ✅ No template approval needed
* ✅ Instant delivery (no approval delays)

***

## Conversation Windows Explained

### Standard 24-Hour Window

* Opens when a user sends a message to your business
* Allows free-form messages during this period
* Outside this window, only pre-approved templates can be sent
* Applies to: Marketing, Utility, Authentication templates

### Free-Entry Point (72-Hour Window)

* Extended window for specific entry points
* Allows 72 hours instead of 24 hours
* Applies to: Click-to-WhatsApp ads, QR codes, certain CTAs
* Provides more flexibility for initial engagement

***

## Template Structure

<Image align="center" border={false} src="https://files.readme.io/5f664a00f49a8c883033d91015b1601d5e9499be94b5468c9bda6705b2a4d83e-whatsapp-message-template.png" />

### Required Fields

All WhatsApp template messages require:

* **sender**: Registered WhatsApp Business number (E.164 format)
* **recipient**: Recipient's WhatsApp number (E.164 format)
* **content_type**: Type of content (`text`, `template`, or `custom`)
* **content**: The template content object

### Optional Fields

* **message_ref**: Your unique reference ID (max 500 characters)
* **sms_fallback**: SMS fallback message if WhatsApp delivery fails

### Template Components

Templates can include:

1. **Header**: Text, image, video, or document
2. **Body**: Main message text with dynamic parameters
3. **Footer**: Optional footer text
4. **Buttons**: Call-to-action, quick reply, or URL buttons
5. **Carousel**: Multiple cards with media and buttons

***

## Sending Templates via API

### Basic Template Send

```bash
curl --location 'https://api.transmitmessage.com/v2/whatsapp/messages' \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_API_KEY' \
--data '{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "template",
  "content": {
    "template": {
      "name": "order_status_update",
      "parameters": ["John", "ORD-12345", "Shipped"],
      "locale": "en_US"
    }
  },
  "message_ref": "order-update-123"
}'
```

### Template with Dynamic Media

```bash
curl --location 'https://api.transmitmessage.com/v2/whatsapp/messages' \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_API_KEY' \
--data '{
  "sender": "1234567890",
  "recipient": "+1234567890",
  "content_type": "custom",
  "content": {
    "custom": {
      "type": "template",
      "template": {
        "name": "product_image_template",
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
                "image": {
                  "link": "https://example.com/product.jpg"
                }
              }
            ]
          },
          {
            "type": "BODY",
            "parameters": [
              {
                "type": "text",
                "text": "Premium Product"
              }
            ]
          }
        ]
      }
    }
  }
}'
```
