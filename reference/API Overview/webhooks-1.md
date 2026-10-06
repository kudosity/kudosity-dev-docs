---
title: Webhooks
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The best way to get immediate notification on the events like SMS delivery or incoming messages, is to set up callback URLs. We will notify this URL immediately after the event occurs and, if the call was unsuccessful, we will retry a few times later. We perform a GET request.

Callbacks can be set in several different places within your account.

* In the SETTINGS section of your account\
  Callbacks set there are the default for the account.
* In the send-sms API call\
  Callbacks for DLR and reply can be set as parameters when you make the send-sms call.
* Forward to URL in Inbound Options for a dedicated virtual number (Reply Callback)
* Forward to URL in Keyword Options (Reply Callback)Forward to URL in List Automation (List Action Callback)

## Callback Priority

Which callback URL is notified is prioritised as follows. Duplicate callbacks will not be sent.

1. send-sms call
2. Forward to URL on feature
3. Default URL in SETTINGS

If you are finding that a callback is not firing from SETTINGS or a Forward to URL. It is likely you have one set on the send-sms call.

## Callback Retries

If we should not be able to reach your configured http callback endpoint, we will retry the attempt a maximum of 4 times with an increasing delay of 1 minute, 5 minute, 1 hour and finally 24 hours.

## Delivery (DLR) Callback

DLR's or Delivery Receipts, are notifications received from the carriers relating to the success or otherwise of an attempted SMS Delivery. Your DLR Callback URL is the default URL we post incoming DLR's to. You can also set the DLR Callback URL separately for individual messages. This is done using a parameter on the send-sms call

Parameters we include in our delivery callback request:

<Table>
  <thead>
    <tr>
      <th>
        PARAMETER
      </th>

      <th>
        DESCRIPTION
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `message_id`
      </td>

      <td>
        Your message ID
      </td>
    </tr>

    <tr>
      <td>
        `mobile`
      </td>

      <td>
        Recipient mobile number
      </td>
    </tr>

    <tr>
      <td>
        `datetime`
      </td>

      <td>
        Date/time of delivery. UTC
      </td>
    </tr>

    <tr>
      <td>
        `status`
      </td>

      <td>
        `delivered` -  Delivered to handset\
        `pending` - No delivery report received from carrier. Allow up to 72hrs or use validity in send-sms call to process.\
        `soft-bounce` - Message was undeliverable due to handset switched off, out of range or other temporary deliverability issue.\
        `hard-bounce` - Handset was disconnected
      </td>
    </tr>

    <tr>
      <td>
        `user_id`
      </td>

      <td>
        Your account ID
      </td>
    </tr>
  </tbody>
</Table>

Example

```http
https://www.myserver.com/processdlr.php?myparameter=myvalue&message_id=331694668&mobile=61400000001&datetime=2020-05-16 12:10:00&status=delivered&user_id=123456
```

## Reply Callback

Incoming SMS are broadly similar, with 2 subtly different forms:

* SMS Replies: are messages received 'in response' to an SMS message sent by you. Responses can come in on both the System Shared Number Pool or any Dedicated Virtual Number, and will be posted to the Callback URL above if there was no URL set as a parameter in the send-sms call.
* Inbound SMS: are messages that are received originating from a users mobile phone and are not in response to a message sent. Inbound SMS can only be received on a Dedicated Virtual Number and will only be posted to the Default URL supplied above.

Parameters we include in our reply callback request:

| PARAMETER       | DESCRIPTION                              |
| --------------- | ---------------------------------------- |
| user\_id        | Your account ID                          |
| message\_id     | Your message ID                          |
| mobile          | Sender’s mobile                          |
| longcode        | The virtual number message was sent to   |
| datetime\_entry | Date/time that message was received. UTC |
| response        | Message text                             |
| response\_id    | ID assigned to incoming message          |
| is\_optout      | Opt-out flag. ‘yes’ or ‘no’              |

Example

```http
https://www.myserver.com/processreply.php?myparameter=myvalue&user_id=54911&message_id=331695745&&response=This is my reply&response_id=35566670&mobile=61400000001&longcode=61429997672&datetime_entry=2020-05-16 12:10:00&is_optout=no
```

## Link Hits Callbacks

The default link hits callback URL is used to notify when a tapth.is/xxxxx URL defined in a message is clicked.

Parameters we include in our link hits callback request:

| PARAMETER   | DESCRIPTION                           |
| ----------- | ------------------------------------- |
| list\_id    | Your list ID                          |
| message\_id | Your message ID                       |
| mobile      | Sender’s mobile                       |
| longcode    | The number message was delivered to   |
| datetime    | Date/time that the click was recorded |
| message     | Message text                          |
| user\_id    | Account ID                            |
| link\_hits  | Link hit count                        |
| firstname   | Firstname of contact that clicked     |
| lastname    | Lastname of contact that clicked      |

Example

```http
https://www.myserver.com/processlinkhit.php?datetime=2020-05-16 13:23:46&firstname=Brad&lastname=Citizen&link_hits=1&list_id=3201749&longcode=TRANSMITSMS&message=This is a link hit callback test
 TapTh.is/clkVMt8z 
UnsubRep.ly/3hHm1u8u&message_id=331696764&mobile=61400000001&rate=10&user_id=54911
```

## List Action Callback

A notification for contacts added too, or removed from lists is available via the Forward to URL option in the AUTOMATION settings for a list.

Parameters we include in our list action callback request:

| PARAMETER         | DESCRIPTION                                      |
| ----------------- | ------------------------------------------------ |
| type              | Add or Delete                                    |
| datetime\_entry   | Date/time of action. UTC.                        |
| list\_id          | ID of list.                                      |
| mobile            | Mobile of number actioned                        |
| firstname         | Firstname of contact                             |
| lastname          | Lastname of contact                              |
| Custom field 1-10 | Custom field names and values will be added also |

Example

```http
https://www.myserver.com/processdlr.php?Carrier=Telstra&Email=jane.citizen@example.com&Handset=iPhone 7&datetime_entry=2020-05-16 13:13:20&firstname=Jane&lastname=Citizen&list_id=4026600&mobile=61400000000&type=delete
```
