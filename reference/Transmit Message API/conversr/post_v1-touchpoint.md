---
title: Create Touchpoint
excerpt: >-
  Creates a new touchpoint in `draft` status.


  | **Key** | **Type** | **Description** | **Required** | **Example Values** |

  | --- | --- | --- | --- | --- |

  | mode | String | Controls whether the touchpoint should be run in Sandbox or
  Live mode.  <br>  <br>Touchpoints in sandbox mode do not send out SMS. | Yes |
  live  <br>sandbox |

  | type | String | The name of the Touchpoint Type that will define template
  message copy and automation rules for inbound replies and other events that
  occur during a conversation. | Yes | nps_survey  <br>email_cleanse 
  <br>lead_call_scheduling |

  | name | String | The name of the touchpoint. Used to differentiate instances
  of the same Touchpoint Type from each other. | Yes | NPS Survey - Jan 23 |

  | operator_name | String | The name of the operator that will receive the
  operator transition emails.  <br>  <br>If the `template_text` within the
  Touchpoint Type includes the _“\[operator_name\]“_ variable, then the value
  provided for operator_name will be substituted here in the outbound SMS
  message. | Yes | Sally |

  | operator_email | String | The email address of the operator that will
  receive the operator transition emails. | Yes |
  [emily@transmitsms.com](https://mailto:emily@transmitsms.com) |

  | company_name | String | The name of the Company / Brand that is sending the
  touchpoint.  <br>  <br>If the `template_text` within the Touchpoint Type
  includes the _“\[company_name\]“_ variable, then the value provided for
  `company_name` will be substituted here in the outbound SMS message. | Yes |
  ACME Co |

  | sender_address | String | The mobile phone number that will be used to send
  the touchpoint to contacts on the contact list. | Yes | 61430000102 |

  | contact_import_id | String | The Contact List ID of the Transmit SMS contact
  list that contains contacts for the touchpoint.  <br>  <br>If no value for
  contact_import_id is provided, a new contact list will be created and returned
  as part of the successful Create Touchpoint response. | No | 1234567 |

  | contact_import_source | String | The source system that is providing the
  contact list.  <br>  <br>For now this is always "transmit". | Yes | transmit |

  | secure | Boolean | Controls whether single-click Quick Responses are enabled
  within operator transition emails.  <br>  <br>If secure is `true`, then
  buttons in the email will behave the same way as the ‘edit’ links and take the
  operator to the Operator Transition web app to continue.  <br>  <br>Defaults
  to `false` if no value provided. | No | true  <br>false |

  | compelling_event | String | Additional text that modifies the outbound SMS
  message.  <br>  <br>If the `template_text` within the Touchpoint Type includes
  _“\[compelling_event\]“_ variable, then this value will be substituted here in
  the outbound SMS message. | No | The 2023 Tax season is fast approaching. |

  | webhook_urls | Object containing key value pairs | A list of webhook
  variables and endpoints that will recieve a webhook when certain conditions
  are reached (as defined in the Touchpoint Type of this touchpoint). | No |
  `"hard_bounces" : "webhook.site/xxxx", "soft_bounces" : "webhook.site/yyyy"` |

  | op_hours | Array of objects | A list of operating days and hours that
  control node transitions with conditions `in_hours` and `out_of_hours` for
  this touchpoint.  <br>  <br>Each object in the array must contain 
  <br>`day_of_week` and `hours`.  <br>  <br>day_of_week is an enum and one of
  "mon", "tue", "wed", "thu", "fri", "sat", "sun".  <br>  <br>Hours is an array
  of objects, and each object must contain `start` and `end` properties which
  define periods of the day considered `in_hours`. | No | `{ "day_of_week":
  "mon", "hours": [{"start": "9:00", "end": "12:00"}, {"start": "13:00", "end":
  "17:00"}] }` |

  | time_zone | String | Sets the time zone for `op_hours`.  <br>Time zone
  string values taken from [IANA Time Zone
  database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones). 
  <br>  <br>If no time zone is provided then the touchpoint will default to UTC.
  | No | Australia/Sydney  <br>Europe/London |
api:
  file: public-openapi.yaml
  operationId: post_v1-touchpoint
hidden: false
---