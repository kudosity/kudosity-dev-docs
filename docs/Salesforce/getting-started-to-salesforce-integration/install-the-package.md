---
title: Install the Package
deprecated: false
hidden: false
metadata:
  robots: index
---
<Cards>
  <Card title="Install in Production" href="https://kudosity.com/integrations/salesforce/installation-guide#install" icon="fa-rocket" target="_blank" />

  <Card title="Test in Sandbox" href="https://kudosity.com/integrations/salesforce/install-sandbox" icon="fa-server" target="_blank" />
</Cards>

1. Log in and navigate to the installation URL.
2. Select Install for All Users.
3. Acknowledge the non-Salesforce AppExchange warning and click Install.

   ![](https://files.readme.io/7dd1df34c1498fa3d26ab3af85f243262a13155680b2950e20e442e25c83ef26-image.png)
4. Approve third-party access to the Kudosity API — tick the checkbox and click Continue.

   ![](https://files.readme.io/55ef13a4ce0c51895477a8f3d978b7984537875a77c195ba00927e9cfb52e128-image.png)
5. Wait approximately 1 minute for installation to complete, then click Done.

<Callout icon="ℹ️" theme="info">
  If installation is taking longer than expected, you will receive an email when it completes.
</Callout>

<Callout icon="⚠️" theme="warn">
  Upgrading from a previous version? Navigate to the same install URL while logged into your existing org. Salesforce will detect the existing package and prompt you to upgrade rather than reinstall. If you encounter a duplicate metadata error, go to Setup → Custom Metadata Types → Credentials and remove the existing record before reinstalling.
</Callout>
