---
title: Confirm a verification code
excerpt: >
  Confirms a sender registration by submitting the verification code.

  On success, the verification `status` becomes `CONFIRMED` and the sender
  registration `status` becomes `VERIFIED`.

  If you exceed the allowed attempts, the verification becomes `FAILED`. Request
  a new verification code to try again.
api:
  file: public-openapi.yaml
  operationId: post_v2-senders-registrations-registration-id-verifications-confirmation
hidden: false
---