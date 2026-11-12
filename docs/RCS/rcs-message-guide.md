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
.rcsg-wrap{width:300px;max-width:100%}
.rcsg-phone{height:450px;padding:6px 6px 0;border-radius:36px 36px 0 0;background:#0f0f12;overflow:hidden;-webkit-mask-image:linear-gradient(#000 80%,transparent);mask-image:linear-gradient(#000 80%,transparent);font-family:"Google Sans",Roboto,"Helvetica Neue",Arial,sans-serif;line-height:1.35;text-align:left;-webkit-font-smoothing:antialiased}
.rcsg-screen{position:relative;height:100%;border-radius:30px 30px 0 0;background:#fff;color:#1f1f1f;overflow:hidden}
.rcsg-status{position:relative;display:flex;align-items:center;justify-content:space-between;height:28px;padding:0 20px;font-size:11px;font-weight:600}
.rcsg-hole{position:absolute;left:50%;top:9px;width:10px;height:10px;margin-left:-5px;border-radius:50%;background:#0f0f12}
.rcsg-status svg{width:11px;height:11px;margin-left:4px;fill:#1f1f1f;vertical-align:middle}
.rcsg-hdr{display:flex;align-items:center;gap:9px;padding:6px 12px 9px;border-bottom:1px solid #eceef3}
.rcsg-hdr svg{width:16px;height:16px;fill:#444746;flex:none}
.rcsg-av{display:flex;align-items:center;justify-content:center;width:28px;height:28px;border-radius:50%;background:#4c23cc;color:#fff;font-size:14px;font-weight:700;flex:none}
.rcsg-name{display:flex;align-items:center;gap:5px;flex:1;font-size:13.5px;font-weight:500}
.rcsg-name svg{width:14px;height:14px}
.rcsg-thread{display:flex;flex-direction:column;align-items:flex-start;gap:7px;padding:16px 12px 0 16px}
.rcsg-bub{position:relative;max-width:226px;padding:9px 12px;border-radius:14px;border-top-left-radius:5px;background:#eef1f8;font-size:13px}
.rcsg-img{position:relative;width:226px}
.rcsg-img svg{display:block;width:100%;height:auto;border-radius:14px;border-top-left-radius:5px}
.rcsg-time{padding-left:4px;font-size:9.5px;color:#5f6368}
.rcsg-chips{display:flex;gap:12px;padding:3px 0 0 1px}
.rcsg-chip{position:relative;display:inline-flex;align-items:center;gap:5px;height:27px;padding:0 11px;border:1px solid #c4c7c5;border-radius:999px;background:#fff;color:#0b57d0;font-size:11.5px;font-weight:500;white-space:nowrap}
.rcsg-chip svg{width:12px;height:12px;fill:#0b57d0}
.rcsg-card{position:relative;width:226px;border:1px solid #dfe3ea;border-radius:14px;border-top-left-radius:5px;background:#fff}
.rcsg-media{position:relative}
.rcsg-media svg{display:block;width:100%;height:auto;border-radius:13px 13px 0 0;border-top-left-radius:4px}
.rcsg-body{padding:9px 12px 3px}
.rcsg-t{position:relative;margin-bottom:3px;font-size:13.5px;font-weight:600}
.rcsg-d{position:relative;font-size:12px;color:#444746}
.rcsg-btns{margin-top:8px;border-top:1px solid #eceef3}
.rcsg-btn{position:relative;display:flex;align-items:center;gap:7px;height:31px;padding:0 12px;border-bottom:1px solid #eceef3;color:#0b57d0;font-size:12px;font-weight:500}
.rcsg-btn:last-child{border-bottom:0}
.rcsg-btn svg{width:13px;height:13px;fill:#0b57d0}
.rcsg-carousel{display:flex;gap:7px}
.rcsg-carousel .rcsg-card{width:168px;flex:none}
.rcsg-n{position:absolute;left:-9px;top:-9px;z-index:1;width:18px;height:18px;border-radius:50%;background:#6f42b9;box-shadow:0 0 0 2px #fff,0 1px 4px rgba(0,0,0,.3);color:#fff;font:700 10px/18px Arial,sans-serif;text-align:center}
.rcsg-t .rcsg-n,.rcsg-d .rcsg-n{left:-22px;top:0}
.rcsg-btn .rcsg-n{top:50%;margin-top:-9px}
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
<div class="rcsg-wrap"><div class="rcsg-phone"><div class="rcsg-screen">
<div class="rcsg-status"><span>9:30</span><span class="rcsg-hole"></span><span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M2 22h20V2z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 21.5 1.5 9.5a16 16 0 0 1 21 0z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><rect x="7.5" y="4" width="9" height="18" rx="1.6"/><rect x="10" y="2" width="4" height="2.5" rx=".6"/></svg></span></div>
<div class="rcsg-hdr"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M20 11H7.8l5.6-5.6L12 4l-8 8 8 8 1.4-1.4L7.8 13H20z"/></svg><span class="rcsg-av">K</span><span class="rcsg-name">Kudosity <svg viewBox="0 -960 960 960" aria-hidden="true"><path fill="#0b57d0" d="m438-452-58-57q-11-11-27.5-11T324-508q-11 11-11 28t11 28l86 86q12 12 28 12t28-12l170-170q12-12 11.5-28T636-592q-12-12-28.5-12.5T579-593L438-452ZM326-90l-58-98-110-24q-15-3-24-15.5t-7-27.5l11-113-75-86q-10-11-10-26t10-26l75-86-11-113q-2-15 7-27.5t24-15.5l110-24 58-98q8-13 22-17.5t28 1.5l104 44 104-44q14-6 28-1.5t22 17.5l58 98 110 24q15 3 24 15.5t7 27.5l-11 113 75 86q10 11 10 26t-10 26l-75 86 11 113q2 15-7 27.5T802-212l-110 24-58 98q-8 13-22 17.5T584-74l-104-44-104 44q-14 6-28 1.5T326-90Z"/></svg></span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M6.6 10.8a15.1 15.1 0 0 0 6.6 6.6l2.2-2.2a1 1 0 0 1 1-.25 11.4 11.4 0 0 0 3.6.6 1 1 0 0 1 1 1V20a1 1 0 0 1-1 1A17 17 0 0 1 3 4a1 1 0 0 1 1-1h3.5a1 1 0 0 1 1 1 11.4 11.4 0 0 0 .6 3.6 1 1 0 0 1-.25 1z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="5" r="2"/><circle cx="12" cy="12" r="2"/><circle cx="12" cy="19" r="2"/></svg></div>
<div class="rcsg-thread">
<div class="rcsg-bub"><span class="rcsg-n">1</span>Can you confirm your appointment on Thursday at 10am?</div>
<div class="rcsg-time">9:41 AM</div>
<div class="rcsg-chips"><span class="rcsg-chip"><span class="rcsg-n">2</span>Yes</span><span class="rcsg-chip"><span class="rcsg-n">3</span>No</span></div>
</div>
</div></div></div>
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

A `200` response confirms that the message was accepted, not that it was delivered. To track delivery, subscribe to the `RCS_STATUS` webhook event. See [RCS Status Events](https://developers.kudosity.com/reference/about-webhooks#rcs-status-events).

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
<div class="rcsg-wrap"><div class="rcsg-phone"><div class="rcsg-screen">
<div class="rcsg-status"><span>9:30</span><span class="rcsg-hole"></span><span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M2 22h20V2z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 21.5 1.5 9.5a16 16 0 0 1 21 0z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><rect x="7.5" y="4" width="9" height="18" rx="1.6"/><rect x="10" y="2" width="4" height="2.5" rx=".6"/></svg></span></div>
<div class="rcsg-hdr"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M20 11H7.8l5.6-5.6L12 4l-8 8 8 8 1.4-1.4L7.8 13H20z"/></svg><span class="rcsg-av">K</span><span class="rcsg-name">Kudosity <svg viewBox="0 -960 960 960" aria-hidden="true"><path fill="#0b57d0" d="m438-452-58-57q-11-11-27.5-11T324-508q-11 11-11 28t11 28l86 86q12 12 28 12t28-12l170-170q12-12 11.5-28T636-592q-12-12-28.5-12.5T579-593L438-452ZM326-90l-58-98-110-24q-15-3-24-15.5t-7-27.5l11-113-75-86q-10-11-10-26t10-26l75-86-11-113q-2-15 7-27.5t24-15.5l110-24 58-98q8-13 22-17.5t28 1.5l104 44 104-44q14-6 28-1.5t22 17.5l58 98 110 24q15 3 24 15.5t7 27.5l-11 113 75 86q10 11 10 26t-10 26l-75 86 11 113q2 15-7 27.5T802-212l-110 24-58 98q-8 13-22 17.5T584-74l-104-44-104 44q-14 6-28 1.5T326-90Z"/></svg></span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M6.6 10.8a15.1 15.1 0 0 0 6.6 6.6l2.2-2.2a1 1 0 0 1 1-.25 11.4 11.4 0 0 0 3.6.6 1 1 0 0 1 1 1V20a1 1 0 0 1-1 1A17 17 0 0 1 3 4a1 1 0 0 1 1-1h3.5a1 1 0 0 1 1 1 11.4 11.4 0 0 0 .6 3.6 1 1 0 0 1-.25 1z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="5" r="2"/><circle cx="12" cy="12" r="2"/><circle cx="12" cy="19" r="2"/></svg></div>
<div class="rcsg-thread">
<div class="rcsg-img"><span class="rcsg-n">1</span><svg viewBox="0 0 452 300" role="img" aria-label="Summer sale banner"><defs><linearGradient id="rcsg-g1" x1="0" y1="0" x2="1" y2="1"><stop offset="0" stop-color="#ffb347"/><stop offset=".55" stop-color="#ff7a59"/><stop offset="1" stop-color="#ff4d8d"/></linearGradient></defs><rect width="452" height="300" fill="url(#rcsg-g1)"/><circle cx="376" cy="72" r="74" fill="#fff3c4" opacity=".28"/><circle cx="376" cy="72" r="46" fill="#fff3c4" opacity=".92"/><path d="M0 236Q56 208 113 236T226 236T339 236T452 236V300H0Z" fill="#fff" opacity=".22"/><path d="M0 262Q56 236 113 262T226 262T339 262T452 262V300H0Z" fill="#fff" opacity=".32"/><text x="34" y="116" fill="#fff" font-family="Arial,Helvetica,sans-serif" font-size="25" font-weight="700" letter-spacing="5">SUMMER SALE</text><text x="31" y="192" fill="#fff" font-family="Arial,Helvetica,sans-serif" font-size="78" font-weight="800">20% OFF</text></svg></div>
<div class="rcsg-time">9:41 AM</div>
<div class="rcsg-chips"><span class="rcsg-chip"><span class="rcsg-n">2</span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 2a10 10 0 1 0 0 20 10 10 0 0 0 0-20zm6.9 6h-3a15.7 15.7 0 0 0-1.4-3.6A8 8 0 0 1 18.9 8zM12 4a14 14 0 0 1 1.9 4h-3.8A14 14 0 0 1 12 4zM4.3 14a8 8 0 0 1 0-4h3.4a16 16 0 0 0 0 4zm.8 2h3a15.7 15.7 0 0 0 1.4 3.6A8 8 0 0 1 5.1 16zm3-8h-3a8 8 0 0 1 4.4-3.6A15.7 15.7 0 0 0 8.1 8zM12 20a14 14 0 0 1-1.9-4h3.8A14 14 0 0 1 12 20zm2.3-6H9.7a14 14 0 0 1 0-4h4.6a14 14 0 0 1 0 4zm.3 5.6a15.7 15.7 0 0 0 1.4-3.6h3a8 8 0 0 1-4.4 3.6zm1.7-5.6a16 16 0 0 0 0-4h3.4a8 8 0 0 1 0 4z"/></svg>Shop now</span></div>
</div>
</div></div></div>
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
<div class="rcsg-wrap"><div class="rcsg-phone"><div class="rcsg-screen">
<div class="rcsg-status"><span>9:30</span><span class="rcsg-hole"></span><span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M2 22h20V2z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 21.5 1.5 9.5a16 16 0 0 1 21 0z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><rect x="7.5" y="4" width="9" height="18" rx="1.6"/><rect x="10" y="2" width="4" height="2.5" rx=".6"/></svg></span></div>
<div class="rcsg-hdr"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M20 11H7.8l5.6-5.6L12 4l-8 8 8 8 1.4-1.4L7.8 13H20z"/></svg><span class="rcsg-av">K</span><span class="rcsg-name">Kudosity <svg viewBox="0 -960 960 960" aria-hidden="true"><path fill="#0b57d0" d="m438-452-58-57q-11-11-27.5-11T324-508q-11 11-11 28t11 28l86 86q12 12 28 12t28-12l170-170q12-12 11.5-28T636-592q-12-12-28.5-12.5T579-593L438-452ZM326-90l-58-98-110-24q-15-3-24-15.5t-7-27.5l11-113-75-86q-10-11-10-26t10-26l75-86-11-113q-2-15 7-27.5t24-15.5l110-24 58-98q8-13 22-17.5t28 1.5l104 44 104-44q14-6 28-1.5t22 17.5l58 98 110 24q15 3 24 15.5t7 27.5l-11 113 75 86q10 11 10 26t-10 26l-75 86 11 113q2 15-7 27.5T802-212l-110 24-58 98q-8 13-22 17.5T584-74l-104-44-104 44q-14 6-28 1.5T326-90Z"/></svg></span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M6.6 10.8a15.1 15.1 0 0 0 6.6 6.6l2.2-2.2a1 1 0 0 1 1-.25 11.4 11.4 0 0 0 3.6.6 1 1 0 0 1 1 1V20a1 1 0 0 1-1 1A17 17 0 0 1 3 4a1 1 0 0 1 1-1h3.5a1 1 0 0 1 1 1 11.4 11.4 0 0 0 .6 3.6 1 1 0 0 1-.25 1z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="5" r="2"/><circle cx="12" cy="12" r="2"/><circle cx="12" cy="19" r="2"/></svg></div>
<div class="rcsg-thread">
<div class="rcsg-card"><div class="rcsg-media"><span class="rcsg-n">1</span><svg viewBox="0 0 452 220" role="img" aria-label="Parcel"><defs><linearGradient id="rcsg-g2" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#e8f0fe"/><stop offset="1" stop-color="#cfdcf7"/></linearGradient></defs><rect width="452" height="220" fill="url(#rcsg-g2)"/><ellipse cx="226" cy="186" rx="118" ry="15" fill="#3c4a6b" opacity=".16"/><path d="M136 80L226 118V188L136 150Z" fill="#d9a66c"/><path d="M316 80L226 118V188L316 150Z" fill="#c48f55"/><path d="M226 42L316 80L226 118L136 80Z" fill="#ecc797"/><path d="M172 65L190 57L280 95L262 103Z" fill="#f7e3c3"/><path d="M262 103L280 95V123L262 131Z" fill="#e3c296"/><path d="M152 106L196 125V147L152 128Z" fill="#fff" opacity=".9"/><path d="M159 116L189 129M159 124L180 133" stroke="#b9c0cc" stroke-width="3" stroke-linecap="round"/></svg></div>
<div class="rcsg-body"><div class="rcsg-t"><span class="rcsg-n">2</span>Your order has shipped</div><div class="rcsg-d"><span class="rcsg-n">3</span>Order #12345 is on its way and should arrive Thursday.</div></div>
<div class="rcsg-btns"><div class="rcsg-btn"><span class="rcsg-n">4</span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 2a10 10 0 1 0 0 20 10 10 0 0 0 0-20zm6.9 6h-3a15.7 15.7 0 0 0-1.4-3.6A8 8 0 0 1 18.9 8zM12 4a14 14 0 0 1 1.9 4h-3.8A14 14 0 0 1 12 4zM4.3 14a8 8 0 0 1 0-4h3.4a16 16 0 0 0 0 4zm.8 2h3a15.7 15.7 0 0 0 1.4 3.6A8 8 0 0 1 5.1 16zm3-8h-3a8 8 0 0 1 4.4-3.6A15.7 15.7 0 0 0 8.1 8zM12 20a14 14 0 0 1-1.9-4h3.8A14 14 0 0 1 12 20zm2.3-6H9.7a14 14 0 0 1 0-4h4.6a14 14 0 0 1 0 4zm.3 5.6a15.7 15.7 0 0 0 1.4-3.6h3a8 8 0 0 1-4.4 3.6zm1.7-5.6a16 16 0 0 0 0-4h3.4a8 8 0 0 1 0 4z"/></svg>Track parcel</div></div></div>
<div class="rcsg-time">9:41 AM</div>
</div>
</div></div></div>
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
<div class="rcsg-wrap"><div class="rcsg-phone"><div class="rcsg-screen">
<div class="rcsg-status"><span>9:30</span><span class="rcsg-hole"></span><span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M2 22h20V2z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 21.5 1.5 9.5a16 16 0 0 1 21 0z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><rect x="7.5" y="4" width="9" height="18" rx="1.6"/><rect x="10" y="2" width="4" height="2.5" rx=".6"/></svg></span></div>
<div class="rcsg-hdr"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M20 11H7.8l5.6-5.6L12 4l-8 8 8 8 1.4-1.4L7.8 13H20z"/></svg><span class="rcsg-av">K</span><span class="rcsg-name">Kudosity <svg viewBox="0 -960 960 960" aria-hidden="true"><path fill="#0b57d0" d="m438-452-58-57q-11-11-27.5-11T324-508q-11 11-11 28t11 28l86 86q12 12 28 12t28-12l170-170q12-12 11.5-28T636-592q-12-12-28.5-12.5T579-593L438-452ZM326-90l-58-98-110-24q-15-3-24-15.5t-7-27.5l11-113-75-86q-10-11-10-26t10-26l75-86-11-113q-2-15 7-27.5t24-15.5l110-24 58-98q8-13 22-17.5t28 1.5l104 44 104-44q14-6 28-1.5t22 17.5l58 98 110 24q15 3 24 15.5t7 27.5l-11 113 75 86q10 11 10 26t-10 26l-75 86 11 113q2 15-7 27.5T802-212l-110 24-58 98q-8 13-22 17.5T584-74l-104-44-104 44q-14 6-28 1.5T326-90Z"/></svg></span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M6.6 10.8a15.1 15.1 0 0 0 6.6 6.6l2.2-2.2a1 1 0 0 1 1-.25 11.4 11.4 0 0 0 3.6.6 1 1 0 0 1 1 1V20a1 1 0 0 1-1 1A17 17 0 0 1 3 4a1 1 0 0 1 1-1h3.5a1 1 0 0 1 1 1 11.4 11.4 0 0 0 .6 3.6 1 1 0 0 1-.25 1z"/></svg><svg viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="5" r="2"/><circle cx="12" cy="12" r="2"/><circle cx="12" cy="19" r="2"/></svg></div>
<div class="rcsg-thread">
<div class="rcsg-carousel">
<div class="rcsg-card"><div class="rcsg-media"><span class="rcsg-n">1</span><svg viewBox="0 0 336 194" role="img" aria-label="Classic tee"><defs><linearGradient id="rcsg-g3" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#eaf2ff"/><stop offset="1" stop-color="#d3e3fd"/></linearGradient></defs><rect width="336" height="194" fill="url(#rcsg-g3)"/><ellipse cx="168" cy="178" rx="74" ry="7" fill="#3c4a6b" opacity=".14"/><path d="M143 30Q168 52 193 30L236 46L262 86L234 100L216 82V170H120V82L102 100L74 86L100 46Z" fill="#2f6fed" stroke="#2f6fed" stroke-width="6" stroke-linejoin="round"/><path d="M120 82L108 54M216 82L228 54" stroke="#1f57c3" stroke-width="2.5" stroke-linecap="round" opacity=".7"/><path d="M143 30Q168 52 193 30" fill="none" stroke="#1f57c3" stroke-width="7" stroke-linecap="round"/><path d="M120 150H216V170H120Z" fill="#1f57c3" opacity=".18"/></svg></div>
<div class="rcsg-body"><div class="rcsg-t">Classic tee</div><div class="rcsg-d">$29</div></div>
<div class="rcsg-btns"><div class="rcsg-btn"><span class="rcsg-n">3</span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 2a10 10 0 1 0 0 20 10 10 0 0 0 0-20zm6.9 6h-3a15.7 15.7 0 0 0-1.4-3.6A8 8 0 0 1 18.9 8zM12 4a14 14 0 0 1 1.9 4h-3.8A14 14 0 0 1 12 4zM4.3 14a8 8 0 0 1 0-4h3.4a16 16 0 0 0 0 4zm.8 2h3a15.7 15.7 0 0 0 1.4 3.6A8 8 0 0 1 5.1 16zm3-8h-3a8 8 0 0 1 4.4-3.6A15.7 15.7 0 0 0 8.1 8zM12 20a14 14 0 0 1-1.9-4h3.8A14 14 0 0 1 12 20zm2.3-6H9.7a14 14 0 0 1 0-4h4.6a14 14 0 0 1 0 4zm.3 5.6a15.7 15.7 0 0 0 1.4-3.6h3a8 8 0 0 1-4.4 3.6zm1.7-5.6a16 16 0 0 0 0-4h3.4a8 8 0 0 1 0 4z"/></svg>Buy tee</div><div class="rcsg-btn">More colours</div></div></div>
<div class="rcsg-card"><div class="rcsg-media"><span class="rcsg-n">2</span><svg viewBox="0 0 336 194" role="img" aria-label="Hoodie"><defs><linearGradient id="rcsg-g4" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#fff4e0"/><stop offset="1" stop-color="#ffe2b8"/></linearGradient></defs><rect width="336" height="194" fill="url(#rcsg-g4)"/><ellipse cx="168" cy="181" rx="78" ry="7" fill="#5b4520" opacity=".16"/><path d="M134 44Q168 -6 202 44Q168 66 134 44Z" fill="#2b2e33" stroke="#2b2e33" stroke-width="6" stroke-linejoin="round"/><path d="M134 44L102 56L72 152L98 160L118 104V172H218V104L238 160L264 152L234 56L202 44Q168 68 134 44Z" fill="#3f444b" stroke="#3f444b" stroke-width="6" stroke-linejoin="round"/><path d="M146 42Q168 14 190 42Q168 56 146 42Z" fill="#1c1e22"/><path d="M138 128H198L210 160H126Z" fill="#565c65"/><path d="M158 56L156 92M178 56L180 92" stroke="#e8eaed" stroke-width="3.5" stroke-linecap="round"/><path d="M118 160H218V172H118Z" fill="#2b2e33" opacity=".55"/></svg></div>
<div class="rcsg-body"><div class="rcsg-t">Hoodie</div><div class="rcsg-d">$59</div></div>
<div class="rcsg-btns"><div class="rcsg-btn"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 2a10 10 0 1 0 0 20 10 10 0 0 0 0-20zm6.9 6h-3a15.7 15.7 0 0 0-1.4-3.6A8 8 0 0 1 18.9 8zM12 4a14 14 0 0 1 1.9 4h-3.8A14 14 0 0 1 12 4zM4.3 14a8 8 0 0 1 0-4h3.4a16 16 0 0 0 0 4zm.8 2h3a15.7 15.7 0 0 0 1.4 3.6A8 8 0 0 1 5.1 16zm3-8h-3a8 8 0 0 1 4.4-3.6A15.7 15.7 0 0 0 8.1 8zM12 20a14 14 0 0 1-1.9-4h3.8A14 14 0 0 1 12 20zm2.3-6H9.7a14 14 0 0 1 0-4h4.6a14 14 0 0 1 0 4zm.3 5.6a15.7 15.7 0 0 0 1.4-3.6h3a8 8 0 0 1-4.4 3.6zm1.7-5.6a16 16 0 0 0 0-4h3.4a8 8 0 0 1 0 4z"/></svg>Buy hoodie</div><div class="rcsg-btn">Size guide</div></div></div>
</div>
<div class="rcsg-time">9:41 AM</div>
<div class="rcsg-chips"><span class="rcsg-chip"><span class="rcsg-n">4</span><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M6.6 10.8a15.1 15.1 0 0 0 6.6 6.6l2.2-2.2a1 1 0 0 1 1-.25 11.4 11.4 0 0 0 3.6.6 1 1 0 0 1 1 1V20a1 1 0 0 1-1 1A17 17 0 0 1 3 4a1 1 0 0 1 1-1h3.5a1 1 0 0 1 1 1 11.4 11.4 0 0 0 .6 3.6 1 1 0 0 1-.25 1z"/></svg>Call us</span></div>
</div>
</div></div></div>
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
* The URL must be publicly reachable and return the file directly, without redirects. Your server must respond within 10 seconds.
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
