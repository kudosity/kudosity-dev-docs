---
title: Set Up Live Responses / Reporting (ECA Setup)
excerpt: >-
  This step enables live delivery reporting, inbound replies, and link hit
  notifications back into Salesforce. This uses Salesforce's External Client App
  (ECA) framework.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Deploy the External Client App 

1. In the Kudosity setup screen, locate the **Live Responses / Reporting** section.
2. If you see a **Setup External Client App** button, click it.
3. Wait 30–60 seconds for the spinner to complete — the ECA is being deployed in the background.
4. Once complete, the screen will show **"Your ECA is ready"** with a Consumer Key input field.

<Callout icon="ℹ️">
  If Live Responses already shows **CONNECTED** and no button appears, the ECA is already set up — no action needed.
</Callout>

![](https://files.readme.io/a95795c79fbcde460bbd8e67303f0c75c15d4db2b34b2c8ae8d51291c6cf762e-image.png)

<br />

# Copy Your Consumer Key 

1. Click **Open External Client App Manager** — this opens Salesforce Setup in a new tab.
2. Find **Kudosity** in the list.
3. Click **View → View Consumer Details**.
4. Authenticate if prompted — Salesforce may send a verification code to your email.
5. Copy the **Consumer Key**.    

   ![](https://files.readme.io/e481810b47672770e1c03aa1f0e18813c1253c4866d3c15ce6e7c442f0aa1851-image.png)

# Save Key and Connect 

1. Return to the Kudosity setup screen.
2. Paste the **Consumer Key** into the field.
3. Click **Save Key & Connect**.
4. You will be redirected to a Salesforce OAuth consent page — click **Allow**.
5. You will be redirected back and **Live Responses / Reporting** will show **CONNECTED**.  

   ![](https://files.readme.io/197a4fc635d39a6b9ef2509cad86128327b6e2d119612edee1dd94d7d36369ca-image.png)

![](https://files.readme.io/ace3001f7654a0926fc6bcdbb3b43846331d2648ed63f3a6940040e8d28afc10-image.png)

![](https://files.readme.io/2c054e5224045096f5c10c97c192a65d9f9c052671d08a18f138f3eaec74e559-image.png)

<br />

# Troubleshooting ECA Setup 

| Issue                                                         | Resolution                                                                                                                                |
| :------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------- |
| Spinner times out / VF session bridge error                   | Click Setup External Client App again to retry                                                                                            |
| Already shows CONNECTED with no button                        | ECA is already set up — no action needed                                                                                                  |
| Consumer Key field disappears after clicking Open App Manager | The system detected an existing Connected App session — refresh the Kudosity setup screen and check if Live Responses now shows CONNECTED |
| Error email received about ECA failing                        | Go to the Kudosity Config page and click Setup External Client App to retry                                                               |

<br />

<br />
