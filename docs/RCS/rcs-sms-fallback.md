---
title: Track an RCS message that falls back to SMS
excerpt: Follow a message from send to delivery, and tell whether it arrived as RCS or as SMS
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    Learn how SMS fallback works for RCS messages, and how to use webhooks to
    tell whether each message was delivered as RCS or as SMS.
  robots: index
next:
  description: ''
---
SMS fallback lets you provide a text alternative when an RCS message can't be delivered. This guide shows you how to include fallback text, track delivery across both channels, and identify how your message arrived.

The examples use placeholder values. Replace `DemoSender` with your RCS agent ID, and replace the recipient, fallback sender and webhook URL with your own.

## How fallback works

An RCS message can fail for reasons outside your control. Kudosity sends your fallback SMS when the RCS message:

* Is rejected, for example because the recipient's device doesn't support RCS.
* Can't be delivered.
* Expires before it's delivered.

Three things to know before you start:

* **The recipient gets a plain SMS.** Images, cards and buttons aren't carried over. Write `sms_fallback.message` so that it makes sense on its own.
* **Fallback is opt-in for each message.** Without an `sms_fallback` object, a failed RCS message isn't sent by SMS.
* **One message, one ID.** The fallback SMS has the same `id` and `message_ref` as the RCS message it replaces. You use these to follow the message across both channels.

## Step 1: Subscribe to status events

A message that falls back reports on two channels, so subscribe to both `RCS_STATUS` and `SMS_STATUS`. One webhook can receive both.

```bash
curl --request POST \
  --url https://api.transmitmessage.com/v2/webhook \
  --header "x-api-key: $KUDOSITY_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "name": "RCS and SMS status",
    "url": "https://example.com/webhooks/kudosity",
    "filter": {
      "event_type": ["RCS_STATUS", "SMS_STATUS"]
    }
  }'
```

A successful request returns `201` with the webhook. If a webhook with the same `url` and `filter` already exists, the request returns `200` with the existing webhook.

```json
{
  "id": "67e3e4af-62e8-4515-8b67-6f20d0c51ffb",
  "name": "RCS and SMS status",
  "url": "https://example.com/webhooks/kudosity",
  "rate_limit": 10,
  "is_sandbox": false,
  "filter": {
    "event_type": ["RCS_STATUS", "SMS_STATUS"]
  },
  "created_at": "2026-11-17T01:00:00Z",
  "updated_at": "2026-11-17T01:00:00Z"
}
```

## Step 2: Send an RCS message with a fallback

Set `sms_fallback.message` to the text the recipient should receive by SMS. Also set `sms_fallback.sender` to a number or sender ID that your account can send SMS from. Set a `message_ref` so that you can tie each event back to your own record, such as an order.

```bash
curl --request POST \
  --url https://api.transmitmessage.com/v2/rcs/messages \
  --header "x-api-key: $KUDOSITY_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "sender": "DemoSender",
    "recipient": "61438333061",
    "content_type": "text",
    "content": {
      "text": {
        "message": "Your order 12345 is out for delivery."
      }
    },
    "sms_fallback": {
      "message": "Your order 12345 is out for delivery.",
      "sender": "61481074185"
    },
    "message_ref": "order-12345"
  }'
```

A successful request returns `200` with the message as it was accepted. Save `data.id`. You match it against every event for this message.

```json
{
  "data": {
    "id": "0b8f3c2e-6d4a-4f1e-9a7b-2c5d8e1f4a63",
    "message_ref": "order-12345",
    "sender": "DemoSender",
    "recipient": "61438333061",
    "recipient_country": "AU",
    "content_type": "text",
    "content": {
      "text": {
        "message": "Your order 12345 is out for delivery."
      }
    },
    "sms_fallback": {
      "message": "Your order 12345 is out for delivery.",
      "sender": "61481074185"
    },
    "created_at": "2026-11-17T01:05:00Z"
  }
}
```

A `200` response confirms that the message was accepted, not that it was delivered.

## Step 3: Follow the message through its events

The examples below show a message delivered as RCS and a message that falls back to SMS. Events can arrive in any order.

### The message is delivered as RCS

You receive `RCS_STATUS` events only: `SENT`, `DELIVERED`, and `READ` if the recipient opens the message and their device reports it.

```json
{
  "event_type": "RCS_STATUS",
  "timestamp": "2026-11-17T01:05:04Z",
  "webhook_id": "67e3e4af-62e8-4515-8b67-6f20d0c51ffb",
  "webhook_name": "RCS and SMS status",
  "status": {
    "type": "RCS",
    "id": "0b8f3c2e-6d4a-4f1e-9a7b-2c5d8e1f4a63",
    "message_ref": "order-12345",
    "sender": "DemoSender",
    "recipient": "61438333061",
    "status": "DELIVERED",
    "reason_code": "DELIVERED_TO_HANDSET"
  }
}
```

### The message falls back to SMS

An `RCS_STATUS` event reports that the RCS message failed. The status is `REJECTED`, `UNDELIVERABLE` or `FAILED`. The event can include a `reason_code`, such as `NOT_RCS_CAPABLE` or `EXPIRED`, or a `description` that says why the message failed. The `description` is text for people to read, so don't match on it.

```json
{
  "event_type": "RCS_STATUS",
  "timestamp": "2026-11-17T01:05:02Z",
  "webhook_id": "67e3e4af-62e8-4515-8b67-6f20d0c51ffb",
  "webhook_name": "RCS and SMS status",
  "status": {
    "type": "RCS",
    "id": "0b8f3c2e-6d4a-4f1e-9a7b-2c5d8e1f4a63",
    "message_ref": "order-12345",
    "sender": "DemoSender",
    "recipient": "61438333061",
    "status": "FAILED",
    "description": "Requested entity was not found. : sms fallback requested"
  }
}
```

`SMS_STATUS` events report the fallback SMS, ending in `DELIVERED` when it reaches the recipient. The `id` and `message_ref` are the same as the RCS message.

```json
{
  "event_type": "SMS_STATUS",
  "timestamp": "2026-11-17T01:05:06Z",
  "webhook_id": "67e3e4af-62e8-4515-8b67-6f20d0c51ffb",
  "webhook_name": "RCS and SMS status",
  "status": {
    "type": "SMS",
    "id": "0b8f3c2e-6d4a-4f1e-9a7b-2c5d8e1f4a63",
    "message_ref": "order-12345",
    "sender": "61481074185",
    "recipient": "61438333061",
    "status": "DELIVERED"
  }
}
```

## Step 4: Work out which channel delivered the message

For a message sent with `POST /v2/rcs/messages`, match events on `status.id`, then read `status.type` and `status.status` together. Use `status.message_ref` to tie the event back to your own record.

| You receive                                                               | What it means                                                                                           |
| :------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------ |
| `RCS_STATUS` with `DELIVERED` or `READ`                                   | Delivered as RCS                                                                                        |
| `SMS_STATUS` with `DELIVERED`                                             | Delivered as SMS                                                                                        |
| A failed `RCS_STATUS` and a failed `SMS_STATUS`                           | Not delivered on either channel                                                                         |
| A failed `RCS_STATUS`, `sms_fallback` set, and no final `SMS_STATUS` yet  | The fallback SMS is still on its way, or it couldn't be sent. See [Things to watch](#things-to-watch)   |
| A failed `RCS_STATUS` and no `sms_fallback`                               | Not delivered                                                                                           |

A failed RCS status is `REJECTED`, `UNDELIVERABLE` or `FAILED`. A failed SMS status is `HARD_BOUNCE`, `SOFT_BOUNCE`, `FAILED` or `REJECTED`. `SENT` and `ACCEPTED` aren't final. See [RCS Status Events](https://developers.kudosity.com/reference/about-webhooks#rcs-status-events) for what each RCS status means.

## If you use the No Code RCS Upgrade

With the [No Code RCS Upgrade](https://kudosity.com/resources/articles/no-code-rcs-upgrade), also known as SMS via RCS, Kudosity delivers your SMS as RCS when the recipient's device supports it, and as SMS when it doesn't. You keep sending SMS as you do today.

An upgraded SMS keeps its `id` and `message_ref` on the RCS side, but the table in Step 4 doesn't apply. `SMS_STATUS` reports `DELIVERED` whichever channel delivered the message, so use `RCS_STATUS` to tell the channels apart:

* **To see which SMS were delivered as RCS**, subscribe to `RCS_STATUS`. An event with `DELIVERED` or `READ` for an SMS `id` means that message arrived as RCS.
* **To get read receipts**, use `RCS_STATUS`. SMS has no read status, so `SMS_STATUS` stops at `DELIVERED`.
* **Your SMS reporting doesn't change.** `SMS_STATUS` still reports `DELIVERED` for every message that arrives, on either channel.

## Things to watch

* **No `sms_fallback`, no SMS.** A failed RCS message without one isn't sent on any other channel.
* **The fallback sender must be yours.** If `sms_fallback.sender` isn't a number or sender ID that your account can send SMS from, the request fails with `404`.
* **The fallback SMS follows the same rules as any SMS you send.** It isn't sent if the recipient has opted out of your SMS, or if your account can't send it, for example because it has run out of credit. You then receive the failed `RCS_STATUS` event and no `SMS_STATUS` events.
* **Links in the fallback SMS aren't shortened or tracked.**
* **Test your fallback text.** Send to a phone that doesn't support RCS and check that the SMS reads well on its own.
