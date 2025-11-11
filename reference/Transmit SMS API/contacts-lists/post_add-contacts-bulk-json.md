---
title: Bulk Add Contacts from CSV File
excerpt: >-
  Add bulk contacts to a list from a file


  The add-contacts-bulk request can be used to add a CSV file of contacts to an
  existing list. It can also be used to create a new list and upload a CSV file
  of contacts to it. If a contact is added that already exists in a list, any
  existing data will be updated.


  **The return code 200 is only used to indicate that the API call has
  successfully hit our server. It in no way indicates if the contacts have been
  added\updated successfully.

  To find the status of an upload, including error messages, you must use the
  add-contacts-bulk-progress endpoint.**


  ```

  CSV File Format and Example:
          Firstname,Lastname,Mobile,"Custom Field 1"
          Jane,Doe,61412345678,10.44

  The minimum required for successful import is Mobile.


  The order above must be followed.

  ```
api:
  file: api_documentation.yml
  operationId: post_add-contacts-bulk-json
hidden: false
---