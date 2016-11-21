---
title: Audit Authorization Policy Change
description: "Windows Server Security"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4713e783-4235-4adf-87d4-a5f2ffc12a4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
---
# Audit Authorization Policy Change

>Applies To: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

This topic for the IT professional describes the Advanced Security Audit policy setting, **Audit Authorization Policy Change**, which determines whether the operating system generates audit events when specific changes are made to the authorization policy.

Authorization policy changes that can be audited include:

-   Assigning or removing user rights (privileges) such as **SeCreateTokenPrivilege**, except for the system access rights that are audited by using the [Audit Authentication Policy Change](audit-authentication-policy-change.md) subcategory.

-   Changing the Encrypting File System (EFS) policy.

Event volume: Low

Default: Not configured

If this policy setting is configured, the following events appear on computers running the supported versions of the Windows operating system as designated in the **Applies to** list at the beginning of this topic, in addition to Windows Server 2008 and Windows Vista.

|Event ID|Event message|
|------|---------|
|4704|A user right was assigned.|
|4705|A user right was removed.|
|4706|A new trust was created to a domain.|
|4707|A trust to a domain was removed.|
|4714|Encrypted data recovery policy was changed.|

## Related resource
[Advanced Security Audit Policy Settings](../advanced-security-audit-policy-settings.md)

