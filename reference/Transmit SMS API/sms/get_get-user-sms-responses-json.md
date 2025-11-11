---
title: Get Responses by  Time Frame
excerpt: >-
  Gets the messsages from a defined timeframe.


  ## Pagination


  This endpoint supports pagination using the page/max pattern:


  **Parameters:**

  - `page`: Page number starting from 1 (default: 1)

  - `max`: Maximum results per page (default: 10, recommended: 10-100)

  - `mobile`: Required mobile number filter (E.164 format)

  - `start`/`end`: Optional date range filters


  **Response Structure:**

  The response includes pagination metadata:

  - `page.count`: Total number of pages available

  - `page.number`: Current page number

  - `total`: Total count of SMS responses matching the filters


  **Navigation Examples:**

  ```

  # First page of responses for a mobile number (default)

  GET /get-user-sms-responses.json?mobile=61478038915


  # Second page with 25 results per page

  GET /get-user-sms-responses.json?mobile=61478038915&page=2&max=25


  # Responses within date range with pagination

  GET
  /get-user-sms-responses.json?mobile=61478038915&start=2020-04-01T00:00:00Z&end=2020-04-30T23:59:59Z&max=50


  # Navigate through all pages

  GET /get-user-sms-responses.json?mobile=61478038915&page=1&max=20

  GET /get-user-sms-responses.json?mobile=61478038915&page=2&max=20

  # Continue until page.number >= page.count

  ```


  **Best Practices:**

  - Use max=10-25 for UI display of conversation history

  - Use max=50-100 for data export or analysis tasks

  - Always specify date ranges for large datasets to improve performance

  - Check page.count to determine if more pages exist

  - Use sorting parameters (sort_field, order) to organize results effectively

  - Consider the mobile number is required - this endpoint is designed for
  per-contact message history
api:
  file: api_documentation.yml
  operationId: get_get-user-sms-responses-json
hidden: false
---