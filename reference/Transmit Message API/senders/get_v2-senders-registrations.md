---
title: List sender registrations
excerpt: >
  Returns a paginated list of sender registrations for the authenticated
  account.

    - If the account does not use parent/child accounts, the API returns registrations for the authenticated account.
    - If the account is a parent account, the API returns registrations for the parent account and all child accounts.
api:
  file: public-openapi.yaml
  operationId: get_v2-senders-registrations
hidden: false
---