---
title: Create partners for business-to-business (B2B) messages - Azure Logic Apps | Microsoft Docs
description: Learn how to add partners to your integration account with the Enterprise Integration Pack and Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: jeconnoc
editor: 

ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc

ms.custom: H1Hack27Feb2017 

---
# Add or update partners in business-to-business agreements in your workflow

Partners are entities that participate in business-to-business (B2B) transactions and exchange messages between each other. Before you can create partners that represent you and another organization in these transactions, you must both share information that identifies and validates messages sent by each other. After you discuss these details and are ready to start your business relationship, you can create partners in your integration account to represent you both.

## What roles do partners play in your integration account?

To define details about the messages exchanged between partners, 
you create agreements between those partners. However, 
before you can create an agreement, you must have added 
at least two partners to your integration account. 
Your organization must be part of the agreement as the **host partner**. 
The other partner, or **guest partner** represents the organization that 
exchanges messages with your organization. The guest partner can be another company, 
or even a department in your own organization.

After you add these partners, you can create an agreement.

Receive and Send settings are oriented from the point of view of the Hosted Partner. 
For example, the Receive settings in an agreement determine how the hosted partner receives messages sent from a guest partner. Likewise, the Send settings on the agreement indicate how the hosted partner sends messages to the guest partner.

## Create partner

1. Sign in to the [Azure portal](https://portal.azure.com).

2. On the main Azure menu, select **All services**. 
In the search box, enter "integration", 
and then select **Integration accounts**.

   ![Find integration account](./media/logic-apps-enterprise-integration-partners/account-1.png)

3. Under **Integration Accounts**, select the integration 
account where you want to add your partners.

   ![Select integration account](./media/logic-apps-enterprise-integration-partners/account-2.png)

4. Choose the **Partners** tile.

   ![Choose "Partners"](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Under **Partners**, choose **Add**.

   ![Choose "Add"](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Enter a name for your partner, then select a **Qualifier**. 
Enter a **Value** to identify documents that your apps receive. 
When you're done, choose **OK**.

   ![Add partner details](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Choose the **Partners** tile again.

   ![Choose "Partners" tile](./media/logic-apps-enterprise-integration-partners/partner-5.png)

   Your new partner now appears. 

   ![View new partner](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## Edit partner

1. In the [Azure portal](https://portal.azure.com), 
find and select your integration account. 
Choose the **Partners** tile.

   ![Choose "Partners" tile](./media/logic-apps-enterprise-integration-partners/edit.png)

2. Under **Partners**, select the partner you want to edit.

   ![Select partner to delete](./media/logic-apps-enterprise-integration-partners/edit-1.png)

3. Under **Update Partner**, make your changes.
After you're done, choose **Save**. 

   ![Make and save your changes](./media/logic-apps-enterprise-integration-partners/edit-2.png)

   To cancel your changes, select **Discard**.

## Delete partner

1. In the [Azure portal](https://portal.azure.com), 
find and select your integration account. 
Choose the **Partners** tile.

   ![Choose "Partners" tile](./media/logic-apps-enterprise-integration-partners/delete.png)

2. Under **Partners**, 
select the partner that you want to delete.
Choose **Delete**.

   ![Delete partner](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## Next steps

* [Learn more about agreements](../logic-apps/logic-apps-enterprise-integration-agreements.md "Learn about enterprise integration agreements")  

