---
title: About MMS
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
The MMS endpoint in our API allows you to send multimedia messages (MMS) to recipients, enabling the inclusion of various media types such as images, audio, and video alongside your text messages. This endpoint is designed to facilitate rich communication by incorporating media content, making it ideal for marketing, notifications, and other multimedia communication needs.

# File Type Support

Our MMS connections support a vast variety of file types. However, be aware that some older handsets models have limited support some images, so results may vary when sending to them.

### Images

| File Type                           | Media Type |
| ----------------------------------- | ---------- |
| JPEG, JPG                           | image/jpeg |
| GIF: GIF87a, GI89a, animated GIF89a | image/gif  |
| BMP (Windows Bitmap)                | image/bmp  |
| PNG                                 | image/png  |

### Audio

| File Type | Media Type |
| --------- | ---------- |
| MP3       | audio/mpeg |
| WAV       | audio/wav  |

### Video

| File Type      | Media Type  |
| -------------- | ----------- |
| MPeG, MPG, MP4 | video/mpeg4 |

# File Size Support

Telcos have limited support for file sizes. Currently, we support:

* 400 KB per file

# File Dimension Support

As you're likely sending on mobile devices, we recommend smaller dimensions for greater visibility on mobile devices:

* 480px by 480px
* 640px by 640px