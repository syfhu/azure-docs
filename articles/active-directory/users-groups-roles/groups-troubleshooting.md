---
title: Troubleshooting dynamic membership for groups | Microsoft Docs
description: Troubleshooting tips for dynamic membership for groups in Azure AD.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''

ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro

---
# Troubleshooting dynamic memberships for groups
**I configured a rule on a group but no memberships get updated in the group**<br/>Verify the values for user attributes on the rule: are there users that satisfy the rule? If everything looks good, please allow some time for the group to populate. Depending on the size of your tenant, the group may take up to 24 hours for populating for the first time or after a rule change.

**I configured a rule, but now the existing members of the rule are removed**<br/>This is expected behavior. Existing members of the group are removed when a rule is enabled or changed. The users returned from evaluation of the rule are added as members to the group.     

**I don’t see membership changes instantly when I add or change a rule, why not?**<br/>Dedicated membership evaluation is done periodically in an asynchronous background process. How long the process takes is determined by the number of users in your directory and the size of the group created as a result of the rule. Typically, directories with small numbers of users will see the group membership changes in less than a few minutes. Directories with a large number of users can take 30 minutes or longer to populate.

### Next steps
These articles provide additional information on Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md)
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md)
* [What is Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integrating your on-premises identities with Azure Active Directory](../connect/active-directory-aadconnect.md)
