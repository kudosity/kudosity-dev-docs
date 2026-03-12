---
title: Create a sender registration
excerpt: |
  Creates a sender registration and sets `status` to `PENDING_APPROVAL`.

    - If the account does not use parent/child accounts, the API ignores `child_account_id` and registers the sender for the authenticated account.
    - If the account is a parent account and you omit `child_account_id`, the API registers the sender for the parent account.
    - If you provide `child_account_id`, the API registers the sender for that child account.
api:
  file: public-openapi.yaml
  operationId: post_v2-senders-registrations
hidden: false
---