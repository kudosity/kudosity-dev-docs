---
title: Format Number
excerpt: >-
  In the majority of cases, mobile phone number data coming from external
  systems is not ideal for SMS delivery. For highest reliability and to be able
  to route correctly a mobile number should be formatted in international format
  E.164

  Normally however a number is stored in a CRM or contact database in local
  format, with spaces, hyphens and other types of unwanted characters that can
  cause a delivery to fail. The format-number call is used to sanitise a number
  by combining the country and the number. eg.

  > Australia 0438 333 061 will become 61438333061

  > New Zealand 0212172782 will become 64212172782

  > USA (281) 869-1226 will become 12818691226
api:
  file: api_documentation.yml
  operationId: post_format-number-json
hidden: false
---