---
title: 12 December 2024 - 🚀 WhatsApp Notifications
author: Tony Becker
hidden: false
published_at: '2025-06-06T03:57:49.654Z'
type: added
---
We’ve just launched a brand new endpoint to support templated notification messages via WhatsApp using registered WhatsApp Business accounts.

# ✅ What’s New

POST **[/v2/whatsapp](https://developers.kudosity.com/reference/post_v2-whatsapp)**\
A robust API endpoint for sending pre-approved templated messages to users on WhatsApp.

Key features:

* Template-Based Messaging:\
  Send messages using WhatsApp-approved templates. Each template must be pre-configured in your WhatsApp Business account and supports text-based dynamic parameters.
* SMS Fallback Support (optional):\
  Include an SMS fallback message that gets delivered if the WhatsApp message fails.
* Message Tracking via message\_ref:\
  Tag each message with a custom message\_ref—this ID will be included in delivery webhooks, making it easy to track message delivery status in your systems.
* Locale-Specific Messaging (optional):\
  Customize the message by specifying a locale (e.g., en\_US, pt\_BR) to reach users in their preferred language.

Clean and Compliant Formatting:

* All phone numbers must be in E.164 international format.
* Templates must adhere to WhatsApp’s strict format and approval process.