---
title: Introduction to RCS
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    RCS Business Messaging is an advanced evolution of SMS, offering
    interactive, branded communication through default messaging apps on Android
    and select iPhones, with features like verified business profiles, smart SMS
    fallback, and a spam-resistant channel, all accessible via a
    developer-friendly API.
  robots: index
next:
  description: ''
---
[RCS Business Messaging](https://developers.google.com/business-communications/rcs-business-messaging/guides/get-started/how-it-works) is the modern evolution of SMS — a richer, interactive channel built into the default messaging apps on Android, and now increasingly available on iPhones in select markets.

With support for branded, app-like experiences and full conversational interactivity, RCS empowers businesses to connect with customers in a far more engaging way — no app installs, no redirects, no clunky mobile web.

Our RCS API is intended for the developers  who plan to integrate their systems with the Kudosity Messaging API to send RCS messages to their users and/or to build an RCS Conversation bot.

<Callout icon="⚙️" theme="default">
  ### This documentation is for developers integrating with our RCS API. At this time, we’re API-first — no UI required (or available) yet
</Callout>

## Why It Matters — For You and Your Customers

If you're a developer building for internal stakeholders or client businesses, RCS unlocks powerful capabilities:

* **Verified by Google** – Messages come from your official, branded business profile, so customers know it’s really you.
* **Smart delivery fallback** – If RCS isn’t supported, we can automatically fall back and send via SMS — ensuring your messages get through.
* **Spam-free by design** – RCS traffic is tightly controlled and verified, avoiding the spam filters that increasingly block legitimate SMS.
* **Boost conversions** - with media-rich, action-oriented messages, customers are seeing high conversion rates. 2-3x higher than SMS!!
* **Simple API Integration**  - Clean, RESTful API to send and receive responses.

## Key RCS Value Propositions

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        ✅ Verified Business Presence on Google
      </th>

      <th>
        🔁 Smart Routing with SMS Fallback
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Messages come from your official brand profile, verified by Google. No random numbers. No confusion.
        Customers can see your logo, brand name, colors, and trust they’re hearing from you — not a scammer.
        ![](https://files.readme.io/a66ce8f4895672803c4f23f0ef8a0a4447c6aed5136daa701c68e2827806b7ef-Screenshot_2025-06-04_at_10.22.40_am.png)
      </td>

      <td>
        If an RCS message can’t be delivered (e.g. unsupported device or network), we automatically route it via SMS — no extra code, no manual failovers.![](https://files.readme.io/2455ad3bbdb494c8be19bd20bd3ee9fd257ccb78828fde545b23851fda3ff830-failover.png)
      </td>
    </tr>
  </tbody>
</Table>

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        :lock:

         Tightly governed, Trusted Channel-low SPAM
      </th>

      <th>
        Built for Developers
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        RCS offers a spam-resistant channel — only verified businesses can send messages, and traffic is tightly governed by carrier and platform rules.\
        In some countries, aggressive SMS spam filtering is catching even valid messages. RCS changes that. Your messages will get through.![](https://files.readme.io/0077389c620d4bb2e3eb277f02c0bb4d52ee32c5cde70c936f3b4aa1f7f9950f-verified-messages.png)
      </td>

      <td>
        Our RCS platform is API-first by design, so you can integrate messaging into your product or workflow without waiting for a frontend.\
        The structure, logic, and endpoints feel familiar — making it simple to adopt, extend, or switch between channels.![](https://files.readme.io/cbb0464109ed157a306e278fa472db0c47f44d9e2c10063f9cf8338c6421d816-ChatGPT_Image_Jun_6_2025_11_01_02_AM.png)
      </td>
    </tr>
  </tbody>
</Table>
