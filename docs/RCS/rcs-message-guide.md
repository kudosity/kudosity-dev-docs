---
title: RCS Message Guide
excerpt: Build RCS messages with text, images, video, rich cards and buttons
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    Build RCS messages with text, images, video, rich cards, carousels and
    suggested replies and actions. Each example pairs what the recipient sees
    with the JSON request body that creates it.
  robots: index
next:
  description: ''
---
This guide shows you how to build each RCS message format with the [Send RCS message](https://developers.kudosity.com/reference/post_v2-rcs-messages) endpoint. Each example shows a preview of the message, the JSON field behind each numbered part, and the full request body.

## Before you begin

You need:

* A Kudosity account and your API key.
* A registered RCS agent. Until your agent is launched, it can only message its approved test numbers. See [RCS Agent Registration Process](https://developers.kudosity.com/docs/rcs-onboarding).
* A recipient whose device supports RCS. To check a number first, use [Check RCS capability](https://developers.kudosity.com/reference/post_v2-rcs-capabilities).

The examples use placeholder values. Replace `DemoSender` with your RCS agent ID, and replace the recipient, links and media URLs with your own. The `cdn.example.com` media URLs don't exist, so a request that uses them fails with a `400` error.

## Choose a message format

| Format               | `content_type` | Object inside `content` | Use it for                                     |
| :------------------- | :------------- | :---------------------- | :--------------------------------------------- |
| Text                 | `text`         | `text`                  | Reminders, alerts and conversations            |
| Image or video       | `media`        | `media`                 | A single media file, without a caption         |
| Standalone rich card | `rich_card`    | `rich_card.standalone`  | One item with text, optional media and buttons |
| Carousel             | `rich_card`    | `rich_card.carousel`    | 2 to 10 cards that the recipient swipes through |

The `content_type` value must match the object you send inside `content`. Standalone cards and carousels both use `rich_card`.

Every format can also carry `suggestions`: reply chips, links and dial buttons shown under the message. See [Suggested replies and actions](#suggested-replies-and-actions).

<HTMLBlock>{`
<style>
.rcsg-phone{width:300px;max-width:100%;border:1px solid #dedbe4;border-radius:24px;background:#fff;color:#242235;overflow:hidden;box-shadow:0 12px 35px rgba(48,32,80,.06);font:15px/1.5 -apple-system,BlinkMacSystemFont,"Segoe UI",Arial,sans-serif}
.rcsg-bar{display:flex;align-items:center;gap:10px;padding:14px 18px;border-bottom:1px solid #eee9f4;font-weight:600}
.rcsg-avatar{display:inline-grid;place-items:center;width:32px;height:32px;border-radius:50%;background:#683bb5;color:#fff}
.rcsg-screen{background:#fbfafc;padding:20px 14px 28px;min-height:260px}
.rcsg-bubble{background:#eeedf3;padding:14px;border-radius:4px 18px 18px 18px}
.rcsg-card{background:#fff;border:1px solid #e0dce8;border-radius:14px;overflow:hidden}
.rcsg-title{font-weight:600;font-size:16px;margin:12px 14px 4px}
.rcsg-desc{font-size:14px;margin:0 14px 14px}
.rcsg-art{position:relative;display:flex;align-items:center;justify-content:center;height:140px;background:#ede6f7;font-size:64px;line-height:1}
.rcsg-sale{flex-direction:column;gap:8px;border-radius:14px;background:#7447ae;color:#fff;font-size:15px;letter-spacing:1px}
.rcsg-sale b{font-size:28px;letter-spacing:0}
.rcsg-button{padding:10px 8px;border-top:1px solid #e9e5ef;color:#5932a1;text-align:center;font-size:14px}
.rcsg-chips{display:flex;flex-wrap:wrap;gap:6px;margin-top:14px}
.rcsg-chip{border:1px solid #cfc3e0;border-radius:22px;padding:6px 10px;background:#fff;color:#5932a1;font-size:13px}
.rcsg-badge{display:inline-block;width:22px;height:22px;margin-right:5px;border-radius:50%;background:#6f42b9;color:#fff;font:700 11px/22px Arial,sans-serif;letter-spacing:0;text-align:center;vertical-align:middle}
.rcsg-art .rcsg-badge{position:absolute;left:10px;top:10px}
.rcsg-carousel{display:flex;gap:10px;overflow:hidden}
.rcsg-carousel .rcsg-card{flex:0 0 205px}
.rcsg-carousel .rcsg-art{height:120px}
.rcsg-hint{margin-top:10px;font-size:12px;color:#787183}
.rcsg-caption{width:300px;max-width:100%;margin-top:10px;font-size:12px;line-height:1.5;color:#787183}
</style>
`}</HTMLBlock>

## Text with suggested replies

Use text for a simple message of up to 3,072 characters. Add suggested replies so the recipient can respond with a tap.

| # | What the recipient sees | JSON field                          |
| :- | :---------------------- | :---------------------------------- |
| 1 | Message text            | `content.text.message`              |
| 2 | **Yes** reply chip      | `content.suggestions[0].reply.text` |
| 3 | **No** reply chip       | `content.suggestions[1].reply.text` |

The agent name and logo above the conversation come from your RCS agent, not from the request.

Send the request with your API key in the `x-api-key` header:

<Columns layout="auto">
  <Column>
    <HTMLBlock>{`
<div class="rcsg-phone">
  <div class="rcsg-bar"><span>‹</span><span class="rcsg-avatar">D</span>Demo store</div>
  <div class="rcsg-screen">
    <div class="rcsg-bubble"><span class="rcsg-badge">1</span>Can you confirm your appointment on Thursday at 10am?</div>
    <div class="rcsg-chips"><span class="rcsg-chip"><span class="rcsg-badge">2</span>Yes</span><span class="rcsg-chip"><span class="rcsg-badge">3</span>No</span></div>
  </div>
</div>
<div class="rcsg-caption">Illustration only. The layout varies between messaging apps.</div>
`}</HTMLBlock>
  </Column>

  <Column>

```bash
curl --request POST \
  --url https://api.transmitmessage.com/v2/rcs/messages \
  --header "x-api-key: $KUDOSITY_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "sender": "DemoSender",
    "recipient": "61438333061",
    "content_type": "text",
    "content": {
      "text": {
        "message": "Can you confirm your appointment on Thursday at 10am?"
      },
      "suggestions": [
        {
          "reply": {
            "text": "Yes",
            "postback_data": "appt-789-yes"
          }
        },
        {
          "reply": {
            "text": "No",
            "postback_data": "appt-789-no"
          }
        }
      ]
    },
    "sms_fallback": {
      "message": "Can you confirm your appointment on Thursday at 10am? Reply YES or NO."
    },
    "message_ref": "appt-789"
  }'
```

  </Column>
</Columns>

A successful request returns `200` with the message as it was accepted. Keep the `id` to look up the message later with [Get RCS message](https://developers.kudosity.com/reference/get_v2-rcs-messages-id).

```json
{
  "data": {
    "id": "0b8f3c2e-6d4a-4f1e-9a7b-2c5d8e1f4a63",
    "message_ref": "appt-789",
    "sender": "DemoSender",
    "recipient": "61438333061",
    "recipient_country": "AU",
    "content_type": "text",
    "content": {
      "text": {
        "message": "Can you confirm your appointment on Thursday at 10am?"
      },
      "suggestions": [
        {
          "reply": {
            "text": "Yes",
            "postback_data": "appt-789-yes"
          }
        },
        {
          "reply": {
            "text": "No",
            "postback_data": "appt-789-no"
          }
        }
      ]
    },
    "sms_fallback": {
      "message": "Can you confirm your appointment on Thursday at 10am? Reply YES or NO."
    },
    "created_at": "2026-11-12T01:30:00Z"
  }
}
```

When the recipient taps a reply chip, the chip's label is sent back to your agent as their reply. The `postback_data` value isn't shown to the recipient. Your application uses it to tell which chip was tapped. See [Handle a tap in your application](#handle-a-tap-in-your-application).

To send plain text, leave out `content.suggestions`.

The remaining examples show the request body only. Send each one to the same endpoint with the same headers.

## Image or video with a link

Use `media` to send one image or video. This example adds a **Shop now** link under the image.

Media messages don't support a caption. When the image needs accompanying text, use a [standalone rich card](#standalone-rich-card).

| # | What the recipient sees | JSON field                                          |
| :- | :---------------------- | :-------------------------------------------------- |
| 1 | Image                   | `content.media.file_url`                            |
| 2 | **Shop now** label      | `content.suggestions[0].action.text`                |
|   | Page the link opens     | `content.suggestions[0].action.open_url_action.url` |

<Columns layout="auto">
  <Column>
    <HTMLBlock>{`
<div class="rcsg-phone">
  <div class="rcsg-bar"><span>‹</span><span class="rcsg-avatar">D</span>Demo store</div>
  <div class="rcsg-screen">
    <div class="rcsg-art rcsg-sale"><span class="rcsg-badge">1</span><span>SUMMER SALE</span><b>20% OFF</b></div>
    <div class="rcsg-chips"><span class="rcsg-chip"><span class="rcsg-badge">2</span>Shop now</span></div>
  </div>
</div>
<div class="rcsg-caption">Illustration only. The layout varies between messaging apps.</div>
`}</HTMLBlock>
  </Column>

  <Column>

```json
{
  "sender": "DemoSender",
  "recipient": "61438333061",
  "content_type": "media",
  "content": {
    "media": {
      "file_url": "https://cdn.example.com/promo/summer-sale.jpg"
    },
    "suggestions": [
      {
        "action": {
          "text": "Shop now",
          "open_url_action": {
            "url": "https://example.com/sale"
          }
        }
      }
    ]
  },
  "message_ref": "sale-2027"
}
```

  </Column>
</Columns>

To send a video, use the same request and point `file_url` at a video file. There's no separate content type for video. See [Media file requirements](#media-file-requirements) for the supported file types.

## Standalone rich card

A standalone card combines an image or video, a title, a description and up to 4 buttons in one message. This order update puts a **Track parcel** button on the card.

The fields in this table sit inside `content.rich_card.standalone.card_content`.

| # | What the recipient sees | JSON field                                  |
| :- | :---------------------- | :------------------------------------------ |
| 1 | Image                   | `media.file_url`                            |
| 2 | Title                   | `title`                                     |
| 3 | Description             | `description`                               |
| 4 | **Track parcel** label  | `suggestions[0].action.text`                |
|   | Page the button opens   | `suggestions[0].action.open_url_action.url` |

<Columns layout="auto">
  <Column>
    <HTMLBlock>{`
<div class="rcsg-phone">
  <div class="rcsg-bar"><span>‹</span><span class="rcsg-avatar">D</span>Demo store</div>
  <div class="rcsg-screen">
    <div class="rcsg-card">
      <div class="rcsg-art"><span class="rcsg-badge">1</span>📦</div>
      <div class="rcsg-title"><span class="rcsg-badge">2</span>Your order has shipped</div>
      <div class="rcsg-desc"><span class="rcsg-badge">3</span>Order #12345 is on its way and should arrive Thursday.</div>
      <div class="rcsg-button"><span class="rcsg-badge">4</span>Track parcel</div>
    </div>
  </div>
</div>
<div class="rcsg-caption">Illustration only. The layout varies between messaging apps.</div>
`}</HTMLBlock>
  </Column>

  <Column>

```json
{
  "sender": "DemoSender",
  "recipient": "61438333061",
  "content_type": "rich_card",
  "content": {
    "rich_card": {
      "standalone": {
        "card_content": {
          "title": "Your order has shipped",
          "description": "Order #12345 is on its way and should arrive Thursday.",
          "media": {
            "file_url": "https://cdn.example.com/orders/parcel.jpg"
          },
          "suggestions": [
            {
              "action": {
                "text": "Track parcel",
                "open_url_action": {
                  "url": "https://example.com/track/12345"
                }
              }
            }
          ]
        }
      }
    }
  },
  "message_ref": "order-12345"
}
```

  </Column>
</Columns>

A card needs at least one of `title`, `description` or `media`. It doesn't have to contain all three. A standalone card is shown vertically, with the media above the text at medium height.

## Carousel of rich cards

Use a carousel to present several products or options in one message. Each entry in `card_contents` creates one card, and the recipient swipes sideways to see the rest.

| # | What the recipient sees               | JSON field                                              |
| :- | :------------------------------------ | :------------------------------------------------------ |
| 1 | First card                            | `content.rich_card.carousel.card_contents[0]`           |
| 2 | Second card                           | `content.rich_card.carousel.card_contents[1]`           |
| 3 | Buttons on a card                     | `content.rich_card.carousel.card_contents[n].suggestions` |
| 4 | **Call us** chip under the carousel   | `content.suggestions[0]`                                |

<Columns layout="auto">
  <Column>
    <HTMLBlock>{`
<div class="rcsg-phone">
  <div class="rcsg-bar"><span>‹</span><span class="rcsg-avatar">D</span>Demo store</div>
  <div class="rcsg-screen">
    <div class="rcsg-carousel">
      <div class="rcsg-card">
        <div class="rcsg-art"><span class="rcsg-badge">1</span>👕</div>
        <div class="rcsg-title">Classic tee</div>
        <div class="rcsg-desc">$29</div>
        <div class="rcsg-button"><span class="rcsg-badge">3</span>Buy tee</div>
        <div class="rcsg-button">More colours</div>
      </div>
      <div class="rcsg-card">
        <div class="rcsg-art"><span class="rcsg-badge">2</span>🧥</div>
        <div class="rcsg-title">Hoodie</div>
        <div class="rcsg-desc">$59</div>
        <div class="rcsg-button">Buy hoodie</div>
        <div class="rcsg-button">Size guide</div>
      </div>
    </div>
    <div class="rcsg-hint">Swipe to see the next card</div>
    <div class="rcsg-chips"><span class="rcsg-chip"><span class="rcsg-badge">4</span>Call us</span></div>
  </div>
</div>
<div class="rcsg-caption">Illustration only. The layout varies between messaging apps.</div>
`}</HTMLBlock>
  </Column>

  <Column>

```json
{
  "sender": "DemoSender",
  "recipient": "61438333061",
  "content_type": "rich_card",
  "content": {
    "rich_card": {
      "carousel": {
        "card_contents": [
          {
            "title": "Classic tee",
            "description": "$29",
            "media": {
              "file_url": "https://cdn.example.com/shop/tee.jpg"
            },
            "suggestions": [
              {
                "action": {
                  "text": "Buy tee",
                  "open_url_action": {
                    "url": "https://example.com/shop/tee"
                  }
                }
              },
              {
                "reply": {
                  "text": "More colours"
                }
              }
            ]
          },
          {
            "title": "Hoodie",
            "description": "$59",
            "media": {
              "file_url": "https://cdn.example.com/shop/hoodie.jpg"
            },
            "suggestions": [
              {
                "action": {
                  "text": "Buy hoodie",
                  "open_url_action": {
                    "url": "https://example.com/shop/hoodie"
                  }
                }
              },
              {
                "reply": {
                  "text": "Size guide"
                }
              }
            ]
          }
        ]
      }
    },
    "suggestions": [
      {
        "action": {
          "text": "Call us",
          "dial_action": {
            "phone_number": "+61290000000"
          }
        }
      }
    ]
  },
  "sms_fallback": {
    "message": "New arrivals: the classic tee ($29) and hoodie ($59). Shop at example.com"
  }
}
```

  </Column>
</Columns>

A carousel needs 2 to 10 cards. Each card follows the same rules as a standalone card. Carousel cards are medium width, with the media at medium height.

Set either `standalone` or `carousel` inside `rich_card`, never both.

## Suggested replies and actions

A suggestion is either a `reply` or an `action`. An action is either an `open_url_action` or a `dial_action`. Set exactly one at each level.

| Type     | What a tap does                                   | Example                                                                                |
| :------- | :------------------------------------------------ | :------------------------------------------------------------------------------------- |
| Reply    | Sends the label back to your agent as a reply     | `{"reply": {"text": "Yes", "postback_data": "appt-789-yes"}}`                          |
| Open URL | Opens a web page                                  | `{"action": {"text": "Shop now", "open_url_action": {"url": "https://example.com/sale"}}}` |
| Dial     | Opens the phone dialler with the number filled in | `{"action": {"text": "Call us", "dial_action": {"phone_number": "+61290000000"}}}`     |

Rules for every suggestion:

* `text` is the label the recipient sees. It must be 1 to 25 characters.
* `open_url_action.url` must be an absolute `https` URL of up to 2,048 characters.
* `dial_action.phone_number` must be in E.164 international format. The leading `+` is optional. Local formats, such as `02 9000 0000`, are rejected.
* `postback_data` is optional on replies and actions, and can be up to 2,048 characters.

### Where to put suggestions

| Placement         | JSON field                                                                                         | Limit        | Behaviour                                                                 |
| :---------------- | :------------------------------------------------------------------------------------------------- | :----------- | :------------------------------------------------------------------------ |
| Under the message | `content.suggestions`                                                                              | 11 chips     | The chips disappear once the recipient replies or another message arrives. |
| On a card         | `card_content.suggestions` for a standalone card, or `card_contents[n].suggestions` for a carousel | 4 per card   | The buttons stay on the card after the recipient replies.                  |

Use card buttons for actions that should stay available, such as tracking an order. Use chips under the message for a quick response to the latest message.

### Handle a tap in your application

Set `postback_data` to a value your application understands, such as `appt-789-yes`. If you leave it out, Kudosity sets a value for you. Set your own so that the value maps to something in your application, such as an order, a booking or a step in a conversation.

When the recipient taps a suggestion, your `RCS_INBOUND` webhook receives an event with `mo.content_type` set to `SUGGESTION`. The label is in `mo.content.suggestion.text`, and your value is in `mo.content.suggestion.postback_data`.

```json
{
  "event_type": "RCS_INBOUND",
  "timestamp": "2026-11-12T01:31:20.246802Z",
  "webhook_id": "fd0e6485-b905-44c1-bd55-fee1d0d6d864",
  "webhook_name": "RCS Inbound Webhook",
  "mo": {
    "type": "RCS",
    "id": "bc2e0a4a-7e21-49de-9912-20cb4148c21f",
    "sender": "61438333061",
    "recipient": "DemoSender",
    "content_type": "SUGGESTION",
    "content": {
      "suggestion": {
        "text": "Yes",
        "postback_data": "appt-789-yes"
      }
    }
  }
}
```

For the appointment example, route `appt-789-yes` to your confirmation workflow and `appt-789-no` to your rescheduling workflow. To receive these events, create a webhook that subscribes to `RCS_INBOUND`. See [About Webhooks](https://developers.kudosity.com/reference/about-webhooks).

## Add an SMS fallback

An RCS message can fail, for example when the recipient's device doesn't support RCS. To reach the recipient anyway, add a top-level `sms_fallback` object with the text to send by SMS.

```json
{
  "sender": "DemoSender",
  "recipient": "61438333061",
  "content_type": "text",
  "content": {
    "text": {
      "message": "Can you confirm your appointment on Thursday at 10am?"
    }
  },
  "sms_fallback": {
    "message": "Can you confirm your appointment on Thursday at 10am? Reply YES or NO."
  }
}
```

* `sms_fallback.message` is required when you include the object. Write it as a standalone SMS. Images, cards and buttons aren't carried over, so include any link or reply instruction in the text.
* `sms_fallback.sender` is optional. Use it to set the number or alphanumeric sender ID that the SMS comes from.
* Without `sms_fallback`, a failed RCS message isn't sent by SMS.

## Media file requirements

These rules apply to `content.media.file_url` and to the `media.file_url` of every card. Kudosity checks each file when you send the request, and the request fails with a `400` error if a file doesn't meet them.

* Use an absolute `https` URL of up to 2,048 characters, on the default port, without credentials. Percent-encode spaces and non-ASCII characters.
* The URL must be publicly reachable and return the file directly within 10 seconds. Redirects aren't followed.
* Your server must return a `Content-Type` header for a supported type, and the file content must match it:
  * Images: `image/jpeg`, `image/png`, `image/gif`
  * Videos: `video/mp4`, `video/mpeg`, `video/mpeg4`, `video/m4v`, `video/webm`, `video/h263`
* The maximum file size is 100 MB. Return a `Content-Length` or `Content-Range` header so that the size can be checked.
* Files are cached by URL for up to 60 days. To change a file, host it at a new URL.

## Limits at a glance

| Item                                | Limit                 |
| :---------------------------------- | :-------------------- |
| Text message                        | 3,072 characters      |
| Cards in a carousel                 | 2 to 10               |
| Card title                          | 200 characters        |
| Card description                    | 2,000 characters      |
| Buttons on a card                   | 4                     |
| Suggestions under a message         | 11                    |
| Suggestion label                    | 1 to 25 characters    |
| `postback_data`                     | 2,048 characters      |
| Link and media URLs                 | 2,048 characters      |
| Media file size                     | 100 MB                |
| `message_ref`                       | 500 characters        |

How a message looks varies between messaging apps and devices. Before you send to customers, test your button placement, media cropping and SMS fallback with your agent's test numbers.

## Next steps

* [Send RCS message](https://developers.kudosity.com/reference/post_v2-rcs-messages): the full request, response and validation reference.
* [Get RCS message](https://developers.kudosity.com/reference/get_v2-rcs-messages-id): check the status and events of a message you sent.
* [About Webhooks](https://developers.kudosity.com/reference/about-webhooks): receive delivery status and inbound events.
