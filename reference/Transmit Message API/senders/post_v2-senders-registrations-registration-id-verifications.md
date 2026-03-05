---
title: Request a verification code.
excerpt: >
  Requests a verification code for a sender registration. This endpoint supports
  registrations with `type` = `PERSONAL_MOBILE_NUMBER` only.

    - The API delivers the code to the registered sender number using the selected `method`, such as `SMS`.
    - Codes expire after 30 minutes.
    - You have a maximum of 5 attempts to confirm the code.
api:
  file: public-openapi.yaml
  operationId: post_v2-senders-registrations-registration-id-verifications
hidden: false
---