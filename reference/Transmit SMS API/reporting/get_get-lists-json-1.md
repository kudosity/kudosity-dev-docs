---
title: Get Information About All Lists
excerpt: >-
  Get information about all lists in your account

  This will return metadata on all your lists


  ## Pagination


  This endpoint supports pagination using the page/max pattern:


  **Parameters:**

  - `page`: Page number starting from 1 (default: 1)

  - `max`: Maximum results per page (default: varies, recommended: 10-50)


  **Response Structure:**

  The response includes pagination metadata:

  - `page.count`: Total number of pages available

  - `page.number`: Current page number

  - `lists_total`: Total count of lists in your account


  **Navigation Examples:**

  ```

  # First page of lists (default)

  GET /get-lists.json


  # Second page with 20 results per page

  GET /get-lists.json?page=2&max=20


  # Get all lists with larger page size

  GET /get-lists.json?max=50


  # Navigate through all pages

  GET /get-lists.json?page=1&max=25

  GET /get-lists.json?page=2&max=25

  # Continue until page.number >= page.count

  ```


  **Best Practices:**

  - Use max=10-25 for UI display purposes

  - Use max=50 for administrative tasks or bulk operations

  - Check page.count to determine if more pages exist

  - For accounts with many lists, use appropriate page sizes to balance
  performance and usability

  - Consider filtering or searching if you need specific lists rather than
  paginating through all
api:
  file: api_documentation.yml
  operationId: get_get-lists-json
hidden: false
---