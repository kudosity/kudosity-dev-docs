---
title: Delete a sender by phone number.
excerpt: |
  Deletes a sender phone number from the account.

    - If the account does not use parent/child accounts, the API ignores `child_account_id` and deletes the sender from the authenticated account.
    - If the account is a parent account and you omit `child_account_id`, the API deletes the sender from the parent account.
    - If you provide `child_account_id`, the API deletes the sender from that child account.
api:
  file: public-openapi.yaml
  operationId: delete_v2-senders-phone-numbers-phone-number
hidden: false
---