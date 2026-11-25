---
title: RCS rich messages, WhatsApp templates, delivery reasons and new AI guides
author: Tony Becker
hidden: true
published_at: '2026-11-25T06:21:31.940Z'
---
A round-up of what's new in the Kudosity API and developer docs since June.

## RCS

* **Rich messages:** [`POST /v2/rcs/messages`](https://developers.kudosity.com/reference/post_v2-rcs-messages) now sends images, video and rich cards as well as text. Set `content_type` to `media` or `rich_card`. A rich card can be a single standalone card or a carousel.
* **Suggested replies and actions:** Add up to 11 suggestions under any message. When a recipient taps one, the `RCS_INBOUND` webhook event arrives with `mo.content_type` set to `SUGGESTION` and the `postback_data` you supplied.
* **New guide:** [RCS Message Guide](https://developers.kudosity.com/docs/rcs-message-guide) shows each message format with a request example, a phone preview, media file requirements and limits.
* **New guide:** [Track an RCS message that falls back to SMS](https://developers.kudosity.com/docs/rcs-sms-fallback) shows how to follow one message across both channels with webhooks.
* **Updated guide:** [RCS Agent Setup Guide](https://developers.kudosity.com/docs/rcs-onboarding) now covers creating, testing and launching an agent in the app.

## Delivery reasons on status webhooks

Status events now tell you why a message ended up where it did, so your code can branch on the outcome:

* `reason_code`: a machine-readable reason, such as `DELIVERED_TO_HANDSET`, `HANDSET_UNREACHABLE`, `NOT_RCS_CAPABLE`, `RECIPIENT_BLOCKED`, `INVALID_RECIPIENT` or `EXPIRED`. More values may be added, so treat a code you don't recognise as unclassified.
* `reason`: a human-readable explanation, where one is available.
* `permanent`: present and `true` when retrying is likely to fail.

See [About Webhooks](https://developers.kudosity.com/reference/about-webhooks).

## WhatsApp

* **New endpoint:** [`GET /v2/whatsapp/templates`](https://developers.kudosity.com/reference/get_v2-whatsapp-templates) lists your account's templates with their content, approval status, category and language.
* **Campaign filter:** [`GET /v2/whatsapp/messages`](https://developers.kudosity.com/reference/get_v2-whatsapp-messages) accepts `campaign_id`, and messages now return the `campaign_id` they belong to.

## Webhooks

* **Safe to repeat:** [`POST /v2/webhook`](https://developers.kudosity.com/reference/post_v2-webhook) with the same `url` and `filter` as an existing webhook returns that webhook with `200 OK` instead of creating a duplicate.
* **Limit:** An account can have up to 25 webhooks. Creating another returns `409 Conflict`.

## Senders

[`GET /v2/senders/registrations`](https://developers.kudosity.com/reference/get_v2-senders-registrations) has new filters and sorting:

* `account_id` accepts one or more accounts and replaces `child_account_id`, which is now deprecated.
* `search` matches on the sender or the child account's company name.
* `registry` and `registry_status` filter alphanumeric registrations.
* `sort` and `order` control the order of results.
* Registrations can now have the status `SUBMITTED_TO_REGISTRY`.

## Errors

The [Error Registry](https://developers.kudosity.com/reference/errors) has been updated to match the API. It now includes entries for `402 Payment Required`, `413 Request Entity Too Large`, `422 Unprocessable Entity`, `501 Not Implemented` and `503 Service Unavailable`.

## AI and agents

* **New:** [Getting started with AI](https://developers.kudosity.com/docs/getting-started-with-ai) and [Get your first message working with your AI assistant](https://developers.kudosity.com/docs/first-message-with-ai).
* **New:** [Agent Skills](https://developers.kudosity.com/docs/agent-skills), [LangChain Tools](https://developers.kudosity.com/docs/langchain) and [Vercel AI SDK Tools](https://developers.kudosity.com/docs/ai-sdk).
* **New:** [Vercel Connect & v0](https://developers.kudosity.com/docs/vercel-connect) and [Bolt](https://developers.kudosity.com/docs/bolt).
* **Updated:** [Kudosity MCP Server](https://developers.kudosity.com/docs/mcp) now leads with the installable server.

## Salesforce

* **New:** [Assign User Permissions](https://developers.kudosity.com/docs/assign-user-permissions).