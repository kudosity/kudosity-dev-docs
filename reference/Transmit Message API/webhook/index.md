---
title: Webhook
excerpt: >-
  Our webhooks API allows you to subscribe to events for SMS, MMS, and RCS
  messages. Using them requires you to have URLs (https) configured on your
  account that can receive POST requests over the internet.


  The `event_type` field is an enum that specifies which event to subscribe to:


  - LINK_HIT

  - OPT_OUT

  - MMS_STATUS

  - MMS_INBOUND

  - SMS_INBOUND

  - SMS_STATUS

  - RCS_STATUS
      

  For status events, the nested `status` field is an enum with these values:


  - FAILED: Failed because of an error from the carrier or handset.
      
  - SENT: Submitted to the carrier.

  - ACCEPTED: Accepted by the carrier and delivery may have been attempted (but
  is not confirmed).

  - DELIVERED: Delivered to handset.

  - SOFT_BOUNCE: Undeliverable due to handset being switched off, out of range
  or other temporary deliverability issue.

  - HARD_BOUNCE: Handset was disconnected.

  - OTHER: Any other status from the carrier.
      

  ## Link Hit


  The `LINK_HIT` event is triggered any time a recipient visits a link that is
  tracked. Track Links is an optional flag on the send message API calls. Along
  with the URL that was being tracked is a hits field indicating how many visits
  this tracked link has in total and a source_message which contains the track
  link sent to the recipient.


  ###### Example Payloads


  ```json Hit link in MMS {
    "event_type": "LINK_HIT",
    "timestamp": "2021-05-06T05:19:42Z",
    "link_hit": {
      "hits": 1,
      "url": "https://www.example.com/abc",
      "source_message": {
        "type": "MMS",
        "id": "b50e4dc1-e57f-459c-a15c-526bee00a4c4",
        "message": "Hey, Check this out! http://clckme.info/KYhSsuIH Opt-out reply STOP",
        "message_ref": "D701",
        "recipient": "61435790000",
        "sender": "61481074191",
        "subject": "Hello",
        "content_urls": [
          "https://res.cloudinary.com/burstsms/image/upload/v1618798563/284KB_qgqtbe.jpg"
        ]
      }
    }
  } ``` ```json Hit link in SMS {
    "event_type": "LINK_HIT",
    "timestamp": "2021-07-20T23:14:04Z",
    "link_hit": {
      "hits": 1,
      "url": "https://www.example.com/abc",
      "source_message": {
        "type": "SMS",
        "id": "faf68308-16cd-4cf9-aef7-47342bd405be",
        "message": "Hey, Check this out! http://clckme.info/KYhSsuIH for Opt-out reply STOP or hit opt out link - http://nsub.me/vqHTcCsh ",
        "message_ref": "D301",
        "recipient": "61435795809",
        "sender": "61481074185"
      }
    }
  } ```

  ## Opt Out


  The `OPT_OUT` event is triggered when a recipient has visited an opt-out link
  in a message they have received or by sending a message with the text "STOP".


  Using parameter \[opt-out-link\] in message body, inserts the opt-out link.


  The source field will be set according to the method a recipient has used to
  opt-out and contain a value of either link or SMS.
    

  ###### Example Payloads


  ```json Opt out via link {
    "event_type": "OPT_OUT",
    "timestamp": "2021-05-06T05:16:20Z",
    "opt_out": {
      "source": "LINK_HIT",
      "source_message": {
        "type": "SMS",
        "id": "a51ebe4e-a412-440e-a8d9-464e68a521cc",
        "message": "Hey, Check this out! http://clckme.info/KYhSsuIH for Opt-out reply STOP or hit opt out link - http://nsub.me/vqHTcCsh",
        "message_ref": "ncc5009d",
        "recipient": "61435790000",
        "sender": "61481074190"  
      }
    }
  } ``` ```json Opt out via message {
    "event_type": "OPT_OUT",
    "timestamp": "2021-05-06T05:16:20Z",
    "opt_out": {
      "source": "SMS_INBOUND",
      "source_message": {
        "type": "SMS",
        "id": "a51ebe4e-a412-440e-a8d9-464e68a521cc",
        "message": "Hey, Check this out! http://clckme.info/KYhSsuIH for Opt-out reply STOP or hit opt out link - http://nsub.me/vqHTcCsh",
        "message_ref": "ncc5009d",
        "recipient": "61435790000",
        "sender": "61481074190"
      }
    }
  } ```

  ## MMS Status


  The `MMS_STATUS` event data is posted to you for changes to an MMS message
  status. These are currently only comprised of internal statuses (SENT,
  FAILED).


  ###### Example Payload


  ```json {
    "event_type": "MMS_STATUS",
    "timestamp": "2021-05-06T05:19:33Z",
    "status": {
      "type": "MMS",
      "id": "b50e4dc1-e57f-459c-a15c-526bee00a4c4",
      "message_ref": "D7001",
      "recipient": "61435790000",
      "sender": "61481074191",
      "status": "SENT"
    }
  } ```

  ## SMS Inbound


  The `SMS_INBOUND` event is posted to you on receipt of an SMS sent from a
  recipient to one of the senders listed on your account. For convenience we
  will try and find a message that you have sent to this recipient from that
  sender and supply it as the `last_message` field. This is useful for
  determining if an inbound message is potentially a reply. The `routed_via`
  field will display when a shared local number has been used to deliver your
  message.


  ###### Example Payload


  ```json {
    "event_type": "SMS_INBOUND",
    "timestamp": "2021-05-06T05:16:33Z",
    "mo": {
      "type":"SMS",
      "id": "alss-2way-605b31c7-d2c49104",
      "message": "Stop",
      "recipient": "61481074190",
      "routed_via": "447507333300",
      "sender": "447507222200",
      "last_message": {
        "type":"SMS",
        "id": "a51ebe4e-a412-440e-a8d9-464e68a521cc",     
        "message": "Hey, check this out!",
        "message_ref": "ncc5009d",
        "recipient": "447507222200",
        "routed_via": "447507333300",
        "sender": "61481074190"
      }
    }
  } ```

  ## SMS Status


  The `SMS_STATUS` event data is posted to you for changes to a SMS message
  status. Multiple status events can be triggered for a single message.


  The `routed_via` field will display when a shared local number has been used
  to deliver your message.


  ###### Example Payload


  ```json {
    "event_type": "SMS_STATUS",
    "timestamp": "2021-05-06T05:16:07Z",
    "status": {
      "type": "SMS",
      "id": "a51ebe4e-a412-440e-a8d9-464e68a521cc",
      "message_ref": "ncc5009d",
      "recipient": "447507222200",
      "routed_via": "447507333300",
      "sender": "61481074190",
      "status": "SENT"
    }
  } ```

  ## RCS Status


  The `RCS_STATUS` event data is posted to you for changes to an RCS message
  status. RCS supports enhanced status tracking including read receipts.


  RCS-specific statuses include:


  - SENT: Submitted to the carrier.

  - DELIVERED: Delivered to handset.

  - FAILED: Failed because of an error from the carrier or handset.

  - READ: Message was read by the recipient (RCS-specific feature).


  ###### Example Payload


  ```json {
    "event_type": "RCS_STATUS",
    "timestamp": "2025-01-07T05:16:07Z",
    "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
    "webhook_name": "RCS Status Webhook",
    "status": {
      "type": "RCS",
      "id": "6fdae71c-dad7-4c36-9734-a69693ecf3b4",
      "message_ref": "order-12345",
      "recipient": "+447903749662",
      "sender": "DemoSender",
      "status": "READ"
    }
  } ```
hidden: false
---