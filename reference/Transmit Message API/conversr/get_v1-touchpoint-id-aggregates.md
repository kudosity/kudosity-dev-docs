---
title: Touchpoint Aggregates Report
excerpt: >-
  Returns an an aggregate summary of contacts in the touchpoint including the
  status of message delivery, results, operator transitions and engagement.


  **Filtering by date the contact was added:**


  - If neither `contact-start` or `contact-end` query parameters are provided,
  the response will include all contacts.

  - If only one of `contact-start` or `contact-end` query parameters is
  provided, the response will include contacts that were added to the touchpoint
  after the `contact-start` date or before the `contact-end` date.

  - If both `contact-start` and `contact-end` query parameters are provided, the
  response will only include contacts that were added to the touchpoint between
  those dates.
api:
  file: public-openapi.yaml
  operationId: get_v1-touchpoint-id-aggregates
hidden: false
---