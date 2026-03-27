---
title: What are webhooks?
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
A webhook is an HTTPS callback triggered by a user-defined event at the originating site, such as the WhatsApp Self-Serve Access API in this case.

In its simplest form, a webhook is a web service that receives HTTPS POST requests from a source.

Webhooks manage incoming messages from WhatsApp users, including text, location, and media such as photos and documents, as well as the status of messages you've sent. Webhooks provide both timely notifications and handle out-of-band issues, making it strongly recommended to establish one in the application settings.

Messages sent by customers to your WhatsApp Business Phone Number are routed to your webhook. When a client sends a text message or a media attachment to your WhatsApp Business API, the platform logs the message and sends a notice (HTTP POST request) to the webhook defined in your app's settings.
