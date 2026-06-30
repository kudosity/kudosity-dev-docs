---
title: RBM Agents
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: RCS/RBM Agents
  description: >-
    In RCS Business Messaging, an agent is the branded, verified identity that
    customers see when they receive messages, while the sender or number is the
    mechanism through which messages are delivered; agents serve as digital
    storefronts for brands, offering various functionalities like customer
    support, marketing, and transactional interactions.
  keywords:
    - RCS
    - RBM
    - Agents
  robots: index
next:
  description: ''
---
An RCS Agent, also known as an RCS Sender Agent, is the digital identity that represents a brand in a customer’s RCS (Rich Communication Services) messaging experience. Similar to a Sender ID (like a short code or long code) in SMS, an RCS Agent provides an enhanced, branded interface where customers and brands can interact within the RCS messaging platform.

## 🆔 Agent vs Sender vs Number: What's the Difference?

In RCS Business Messaging, it’s easy to get mixed up between agents, senders, and numbers — especially since each plays a different role in how messages are delivered and displayed.

Here’s how it works in Kudosity’s RCS platform:

### 🤖 Agent – What Your Customers See

An Agent is the branded identity your customers interact with. It’s the verified business profile that appears inside the Messages app.

Agents include:

* Your brand name
* Your logo and color scheme
* Verified Google badge
* Optional contact info (call, email, web)

🧠 Customers see your agent name, not your phone number. Messages appear to come from your business — not a random shortcode or longcode.

### 🔢 Sender / Number – How the Message Gets Delivered

With SMS, the sender is the number (short code or long code) that a message is sent from. RCS is different. In RCS, messages are sent through an agent, not a number.

Some important details about senders and agents:

* Agents can be linked to a sender number behind the scenes -- If the recipient doesn’t support RCS, we can **automatically fall back to SMS** using the linked sender number.
* Agents can also be linked to a callable number that customers can call from the agent. See the image below for an example.

## What Agents Look Like

Agents are branded business profiles that appear in a user’s messaging app. There are quite a few details to an agent, so we've identified them below.

<Image align="center" src="https://files.readme.io/0e7eadbf14b3ab20b6c99c82084fc8d229a924c1f3411315abfe7048664a9ab1-Screenshot_2025-06-05_at_1.27.43_pm.png" />

| #  | Field Name                | Description                                                                                                        |
| -- | ------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| 1  | **Hero Image**            | Top banner image (1440x448px, JPG, ≤360KB)                                                                         |
| 2  | **Logo Image**            | Circular icon used across the RCS UI (224x224px, JPG, ≤90KB)                                                       |
| 3  | **Agent Name**            | The public name users see. This is the brand name your customers will identify your business as. (e.g. "Kudosity") |
| 4  | **Agent Description**     | A short summary of what the agent does — shown under the name                                                      |
| 5  | **Action Icons**          | Call, website, and email icons matching your brand color                                                           |
| 6  | **Action Icons**          | Call, website, and email icons matching your brand color                                                           |
| 7  | **Phone Number**          | Customer-facing number used when calling.                                                                          |
| 8  | **Label for Phone**       | What’s shown under the phone number (e.g. “Phone” or “Support”)                                                    |
| 9  | **Website URL**           | Clickable link visible in the agent info panel                                                                     |
| 10 | **Label for Website**     | Descriptive text shown under the link (e.g. “Kudosity Homepage”)                                                   |
| 11 | **Email Address**         | Customer-facing email visible in the agent                                                                         |
| 12 | **Label for Email**       | Shown under the email address (e.g. “Contact” or “Support”)                                                        |
| 13 | **Privacy Policy Link**   | Required, open-access link to your privacy policy                                                                  |
| 14 | **Terms of Service Link** | Required, open-access link to your terms                                                                           |

<br />

## Common Agent Use Cases

You can think of agents like micro-apps — each tailored to a use case. Some examples:

* Customer Support Agent\
  Branded support line with quick replies, links to help docs, and escalation options.
* Marketing Agent\
  Sends promos, product updates, carousels, or interactive campaigns (e.g. surveys or flash sales).
* Transactional Agent\
  Sends receipts, delivery updates, appointment confirmations, etc., all with smart buttons like “Track,” “Reschedule,” or “Get Help.”
* Multi-brand Platform?\
  You can register separate agents per brand, business unit, or region — all from a single platform.