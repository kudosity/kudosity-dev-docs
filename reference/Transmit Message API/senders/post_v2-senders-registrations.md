---
title: Create a sender registration
excerpt: |
  Creates a sender registration and sets `status` to `PENDING_APPROVAL`.

    - If the account does not use parent/child accounts, the API ignores `child_account_id` and registers the sender for the authenticated account.
    - If the account is a parent account and you omit `child_account_id`, the API registers the sender for the parent account.
    - If you provide `child_account_id`, the API registers the sender for that child account.
    - If a registration for the same sender was already created within the last 30 minutes (for the same account or child account), the API returns `429 Too Many Requests`. This prevents abuse of the verification retry limit.
    - Any existing `PENDING_APPROVAL` registration for the same sender (for the same account or child account) is automatically cancelled before the new one is created.
api:
  file: public-openapi.yaml
  operationId: post_v2-senders-registrations
hidden: false
---