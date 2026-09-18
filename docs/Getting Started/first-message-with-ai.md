---
title: Get your first message working with your AI assistant
deprecated: false
hidden: false
metadata:
  robots: index
---
Send a test SMS to your own phone using your connected AI assistant. You will prepare the message, approve the send and check the result.

## Before you start

Complete [Getting started with AI](https://developers.kudosity.com/docs/getting-started-with-ai) first. This walkthrough uses the recommended Kudosity MCP setup with messaging tools enabled, not a documentation-only connection.

Have your own mobile number and a sender available for your account and destination. See the [Send SMS reference](https://developers.kudosity.com/reference/post_v2-sms) for sender requirements.

> **This sends a real SMS.** Use your own phone for this test and check your account's available credit and pricing before sending.
>
> **Trial accounts:** Use the mobile number registered at sign-up. Trial messages may be replaced with preconfigured demo text. See the [trial SMS guidance](https://developers.kudosity.com/docs/step-3-make-an-api-request).

## 1. Prepare your message

Paste this into your connected assistant. Replace `[YOUR_MOBILE_NUMBER]` with your own mobile number, including the country code.

```text
Prepare one test SMS using my existing Kudosity MCP connection.

Recipient: [YOUR_MOBILE_NUMBER]
Message: "Hello from Kudosity! My AI assistant sent this SMS."

Use the official SMS documentation:
https://developers.kudosity.com/reference/post_v2-sms

Ask me which sender to use if one is not already configured.
Do not guess a sender or recipient, and keep API credentials private.

Show me the sender, recipient and message for review.
Do not send until I explicitly approve.
```

Your assistant should show the proposed message and identify any missing details. Resolve those before continuing. This step prepares the request; it does not send it.

## 2. Approve and send

Check the sender, recipient and message. When they are correct, reply:

```text
Send this SMS once using the details I approved.
Show me the message ID and the status returned by Kudosity.
Do not retry automatically if the outcome is uncertain.
```

Keep the returned message ID so you can look up the same message. A generated code example is not evidence of a send: your assistant should report the actual API result.

If your assistant cannot execute messaging requests, return to the [MCP setup guide](https://developers.kudosity.com/docs/mcp) before continuing.

## 3. Check the result

Check your phone, then ask:

```text
Check the current delivery status of the SMS we just sent,
using its returned message ID. Use a read-only lookup.

Explain the reported status and any returned error.
Do not send another message.
```

The MCP server's `get_message` tool can retrieve a sent message and its status. The underlying [Get SMS endpoint](https://developers.kudosity.com/reference/get_v2-sms-id) uses the ID returned when the message was created.

**A successful send request is not the same as confirmed delivery.** Your assistant should report the status it actually retrieves, rather than assume the SMS has arrived. See the [status definitions](https://developers.kudosity.com/reference/about-webhooks) for the distinction between submission and delivery.

If delivery is still unconfirmed, recheck the original message instead of sending another one. If a request fails, ask your assistant to explain the returned error and what needs correcting before approving another attempt.

## Next: Build it into your application

Once you have received the test SMS, continue to [Send messages from AI agents & workflows](https://developers.kudosity.com/docs/send-messages-from-ai-agents) for runnable examples you can use in your own application.
