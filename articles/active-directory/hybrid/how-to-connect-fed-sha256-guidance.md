---
title: Изменение хэш-алгоритма подписи для отношения доверия с проверяющей стороной Office 365 | Документация Майкрософт
description: Эта страница содержит рекомендации по изменению алгоритма SHA для доверия федерации с Office 365
keywords: SHA1,SHA256,O365,федерация,aadconnect,adfs,ad fs,изменение sha,доверие федерации,отношение доверия с проверяющей стороной
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9aa597c8b458305946aa298631726df3da317534
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56181377"
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Изменение хэш-алгоритма подписи для отношения доверия с проверяющей стороной Office 365
## <a name="overview"></a>Обзор
Службы федерации Active Directory (AD FS) подписывают маркеры для Microsoft Azure Active Directory, чтобы защитить их от незаконного изменения. Подпись основывается на алгоритме SHA1 или SHA256. Теперь Azure Active Directory поддерживает маркеры, подписанные с помощью алгоритма SHA256. Мы рекомендуем выбирать этот алгоритм подписи маркера для обеспечения максимальной защиты. В этой статье описывается процедура замены алгоритма подписи маркера на алгоритм с более высоким уровнем защиты — SHA256.

>[!NOTE]
>Корпорация Майкрософт рекомендует использовать алгоритм SHA256 для подписания маркеров, так как он защищеннее, чем SHA1, но SHA1 по-прежнему поддерживается.

## <a name="change-the-token-signing-algorithm"></a>Изменение алгоритма подписи маркера
После того как алгоритм подписи будет выбран с помощью одного из указанных ниже способов, службы AD FS будут подписывать маркеры для отношения доверия с проверяющей стороной Office 365 с помощью SHA256. В конфигурацию не требуется вносить никакие дополнительные изменения. Кроме того, это изменение никак не влияет на возможность доступа к Office 365 или другим приложениям Azure AD.

### <a name="ad-fs-management-console"></a>Консоль управления AD FS
1. Откройте консоль управления AD FS на сервере-источнике AD FS.
2. Разверните узел AD FS и выберите **Relying party trusts**(Отношения доверия с проверяющей стороной).
3. Щелкните правой кнопкой мыши нужный объект отношений доверия с проверяющей стороной Office 365 или Azure и выберите пункт **Свойства**.
4. Перейдите на вкладку **Дополнительно** и в поле "Secure hash algorithm" (Алгоритм SHA) выберите значение SHA256.
5. Последовательно выберите **ОК**.

![Алгоритм подписи SHA256 — MMC](./media/how-to-connect-fed-sha256-guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>Командлеты AD FS PowerShell
1. На любом сервере AD FS откройте PowerShell с правами администратора.
2. Задайте алгоритм SHA с помощью командлета **Set-AdfsRelyingPartyTrust** .
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'https://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Также ознакомьтесь
* [Управление службами федерации Active Directory и их настройка с помощью Azure AD Connect](how-to-connect-fed-management.md#repairthetrust)

