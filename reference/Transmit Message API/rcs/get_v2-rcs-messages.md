---
title: List RCS messages
excerpt: >
  Retrieves a list of RCS messages for your account with flexible date
  filtering.


  Messages are returned in reverse chronological order (newest first).


  **Date Filtering:**

  - Preset ranges (last_week, last_thirty, last_month): Calculated using your
  account timezone

  - Custom date ranges: Use RFC3339 timestamps (max 90 days)

  - The 'all' option provides up to 365 days of history

  - All timestamps in responses are returned in UTC format


  **Pagination:**

  - Cursor-based pagination for efficient data retrieval

  - Control page size with the limit parameter (1-100 messages per page)
api:
  file: public-openapi.yaml
  operationId: get_v2-rcs-messages
hidden: false
---