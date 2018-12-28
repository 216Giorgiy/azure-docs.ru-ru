---
title: Хранение данных удостоверений клиентов из ЕС — Azure Active Directory | Документация Майкрософт
description: Узнайте, где Azure Active Directory хранит данные, связанные с идентификаторами клиентов из ЕС.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/17/2018
ms.custom: it-pro, seodec18
ms.openlocfilehash: 371c7b2eb2f2e0e34dc80cf8fe6771620fa76765
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2018
ms.locfileid: "53099944"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Хранение данных удостоверений клиентов из ЕС — Azure Active Directory
Azure Active Directory (Azure AD) помогает управлять удостоверениями пользователей и создавать политики доступа на основе аналитики для защиты ресурсов организации. Данные удостоверений хранятся в расположении, которое основывается на адресе организации, указанном при оформлении подписки на службу. Например, при оформлении подписки на Office 365 или Azure. Определенные сведения о том, где хранятся данные удостоверений, можно найти в разделе [Где находятся ваши данные?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) Центра управления безопасностью Майкрософт.

Хотя большинство связанных с Azure AD данных удостоверений европейских клиентов, остается в европейских центрах обработки данных, существуют пять связанных с пользователями атрибутов, которые обычно хранятся в центрах обработки данных в США. Этими атрибутами являются GivenName, Surname, userPrincipalName, Domain и PasswordHash. Атрибут PasswordHash может быть исключением и не хранится в США, если кто-то использует локально метод федеративной проверки подлинности, который блокирует синхронизацию значения PasswordHash с Azure AD. Кроме того, есть операционные данные определенных служб, которые требуются для нормальной работы Azure AD, хранятся в США и не содержат персональных данных.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Данные европейских клиентов, хранящиеся за пределами европейских центров обработки данных

Большинство связанных с Azure AD данных удостоверений европейских клиентов из организаций с европейскими адресами остается в европейских центрах обработки данных. Данные Azure AD, которые хранятся в европейских центрах обработки данных, а также реплицируются в центрах обработки данных США, включают в себя:

- **Атрибуты, связанные с идентификаторами**

    Следующие атрибуты, связанные с идентификаторами, будут реплицированы в США:

    - GivenName
    - Surname
    - userPrincipalName
    - Домен
    - PasswordHash
    - sourceAnchor
    - AccountEnabled
    - passwordPolicies
    - StrongAuthenticationRequirement
    - ApplicationPassword
    - PUID

- **Данные Многофакторной идентификации Microsoft Azure и самостоятельного сброса пароля Azure AD (SSPR)**
    
    MFA хранит все неактивные данные пользователей в европейских центрах обработки данных. Тем не менее, некоторые данные определенных служб MFA хранятся в США, а именно:
    
    - Данные двухфакторной проверки подлинности и связанные персональные данные могут храниться в США при использовании MFA или SSPR.
        - Все двухфакторные проверки подлинности с помощью телефонных звонков или SMS могут выполняться операторами из США.
        - Push-уведомления, использующие приложение Microsoft Authenticator, требуют уведомлений от службы уведомлений разработчика (Apple или Google), которая может быть расположена за пределами Европы.
        - OATH-коды всегда проверяются в США. 
    - Некоторые журналы MFA и SSPR хранятся в США 30 дней, независимо от типа проверки подлинности.

- **Microsoft Azure Active Directory B2C (Active Directory B2C)**

    Azure AD B2C хранит все неактивные данные пользователей в европейских центрах обработки данных. Тем не менее, операционные журналы (с удаленными персональными данными) остаются в расположении, откуда пользователь получает доступ к службам. Например, если пользователь B2C обращается к службе в США, то операционные журналы остаются в США. Кроме того, все данные конфигурации политики, не содержащие персональные данные, хранятся только в США. Дополнительные сведения о конфигурациях политики см. в статье [Azure Active Directory B2C: встроенные политики](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies).

- **Microsoft Azure Active Directory B2B (Active Directory B2B)** 
    
    Active Directory B2B хранит все неактивные данные пользователей в европейских центрах обработки данных. Однако B2B хранит неперсональные метаданные в таблицах в центрах обработки данных в США. Эти таблицы содержат такие поля, как redeemUrl, invitationTicket, идентификатор ресурса клиента, InviteRedirectUrl и InviterAppId.

- **Доменные службы Microsoft Azure Active Directory (Azure AD DS)**

    Azure AD DS хранит данные пользователей в том же расположении, где размещена виртуальная сеть Azure, выбранная клиентом. Таким образом, если сеть находится за пределами Европы, то данные реплицируются и хранятся за пределами Европы.

- **Службы и приложения, интегрированные с Azure AD**

    Любые службы и приложения, которые интегрируются с Azure AD, имеют доступ к данным удостоверений. Оцените каждую службу и приложение, чтобы определить, как данные удостоверений обрабатываются этой конкретной службой или приложением и соответствуют ли они требованиям компании к хранению данных.

    Дополнительные сведения о местонахождении данных служб Майкрософт см. в разделе [Где находятся ваши данные?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) Центра управления безопасностью Майкрософт.

## <a name="next-steps"></a>Дополнительная информация
Дополнительные сведения об этих функциях и возможностях, описанных выше, смотрите в следующих статьях.
- [Что такое Многофакторная идентификация Azure?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
- [Самостоятельный сброс пароля в Azure AD для ИТ-специалистов](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)
- [Что такое Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Что такое служба совместной работы Azure AD B2B?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Доменные службы Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)
