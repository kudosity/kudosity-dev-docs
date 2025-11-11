---
title: Get Numbers
excerpt: >-
  Edit inbound options for a dedicated virtual number.


  **Get a list of numbers leased by you or available to be leased.**


  Dedicated virtual numbers are used to receive MO (Mobile Originated) messages.
  They also make sure that all of your messages are sent from a number that is
  always the same. Dedicated Virtual Number availability is limited to certain
  countries. Check [Global SMS delivery
  list](https://support.transmitsms.com/support/solutions/articles/44001940675-global-sms-delivery-list)
  for availability for your destination country.


  ## Pagination


  This endpoint supports pagination using the page/max pattern:


  **Parameters:**

  - `page`: Page number starting from 1 (default: 1)

  - `max`: Maximum results per page (default: varies, recommended: 10-100)

  - `filter`: Choose 'owned' or 'available' (default: owned)


  **Response Structure:**

  The response includes pagination metadata:

  - `page.count`: Total number of pages available

  - `page.number`: Current page number

  - `numbers_total`: Total count of numbers matching the filter


  **Navigation Examples:**

  ```

  # First page of owned numbers (default)

  GET /get-numbers.json


  # Second page with 20 results per page

  GET /get-numbers.json?page=2&max=20


  # Available numbers for leasing

  GET /get-numbers.json?filter=available&max=50


  # Navigate through all pages

  GET /get-numbers.json?page=1&max=100

  GET /get-numbers.json?page=2&max=100

  # Continue until page.number >= page.count

  ```


  **Best Practices:**

  - Use max=10-20 for UI display purposes

  - Use max=50-100 for administrative tasks

  - Check page.count to determine if more pages exist

  - Filter by 'owned' vs 'available' to reduce dataset size

  - For large accounts, consider filtering by country or status
api:
  file: api_documentation.yml
  operationId: get_get-numbers-json
hidden: false
---