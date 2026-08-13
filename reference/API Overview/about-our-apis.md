---
title: Which API Should I Use?
deprecated: false
hidden: false
link:
  new_tab: false
metadata:
  title: ''
  description: ''
  robots: index
---
# Which API Should I Use?

Meet the two ways to send messages on Kudosity. Both run on the same platform and share your account, API keys, senders, reporting, and billing — so you can pick what fits your build today and evolve later.

<Image align="center" src="https://files.readme.io/1cefe19046a739730f16adef586462cd0e55b57e0397c60d19e6cb2f7ceb0eec-kudosity-apis-compare-cards-updated.png" />

## Quick Recommendation

<Cards columns="2">
  <Card title="TransmitMessage (V2)" href="#transmitmessage-v2" icon="fa-rocket">
    **Recommended for new builds**
    
    Modern API with MMS, WhatsApp, RCS support and API-managed webhooks.
  </Card>
  <Card title="TransmitSMS (V1)" href="#transmitsms-v1" icon="fa-gears">
    **For classic integrations**
    
    Fully supported with multi-recipient sends.
  </Card>
</Cards>

## API Comparison

<Tabs>
  <Tab title="When to Use V2">
    Choose **TransmitMessage (V2)** when you need:
    
    - **MMS via API** (send & receive) - V2 is the only API with MMS support, including an `inbound_mms` webhook
    - **Simple, programmatic sends** with modern webhooks - Webhooks are created/managed by API; JSON responses; streamlined API-key auth
    - **WhatsApp** (send & receive) - Rich WhatsApp messaging, notifications
    - **RCS** (send & receive) - RCS notifications, rich messaging
    - **Track multiple links** in one SMS - Auto-detect & track multiple links in a message
    - **Your own message reference** alongside our message ID - Add a `message_ref` for reconciliation/workflows
  </Tab>
  
  <Tab title="When to Use V1">
    Choose **TransmitSMS (V1)** when you need:
    
    - **Classic integration staying put** - Fully supported; shares your senders, reporting, and UI
    - **Single-request multi-recipient sends** - V1 supports multi-recipient (one call, many numbers)
    - **XML response format** - V1 supports XML or JSON; V2 is JSON-only
  </Tab>
</Tabs>

> **💡 Recommendation:** Start on V2. Keep V1 if you rely on multi-recipient requests. You can use both under the same account and senders.

## Feature Comparison

<Accordion title="Account & Authentication" icon="fa-key">
- **Unified account & UI**: Same login, senders, reporting, and billing across both APIs
- **Auth**: V2 uses API-key auth; V1 uses Basic Auth (key + secret)
</Accordion>

<Accordion title="Webhooks & Delivery" icon="fa-plug">
- **Webhooks**: V2 webhooks are managed via API (create/list/update/delete). V1 webhooks are configured in the UI
- **Delivery reports**: V2 via webhook; V1 via webhook or email
- **Retries**: Both retry failed webhook deliveries; V2 has a more granular schedule
</Accordion>

<Accordion title="Message Features" icon="fa-message">
- **Message Types**: V2 supports SMS, MMS, WhatsApp, RCS; V1 supports SMS only
- **Link Tracking**: V2 auto-detects and tracks multiple links per message; V1 tracks a single link per message
- **Multi-recipient**: V1 supports batch sends; V2 requires individual requests
- **Response Format**: V2 is JSON-only; V1 supports both JSON and XML
</Accordion>

## Migration Guidance

<Columns layout="auto">
  <Column>
    ### New Customers
    
    **Build on TransmitMessage (V2)** to access:
    - MMS capabilities
    - Multi-link tracking
    - Message references
    - API-managed webhooks
    - Modern messaging channels (WhatsApp, RCS)
  </Column>
  
  <Column>
    ### Existing V1 Customers
    
    **No rush to migrate.** Continue on TransmitSMS (V1) and consider migrating when you need:
    - MMS support
    - API-managed webhooks
    - Multiple tracked links per message
    - WhatsApp or RCS messaging
  </Column>
</Columns>