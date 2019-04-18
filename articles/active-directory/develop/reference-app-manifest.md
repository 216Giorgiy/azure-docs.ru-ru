---
title: Основные сведения о манифесте приложения Azure Active Directory | Документация Майкрософт
description: Подробное описание манифеста приложения Azure Active Directory, который представляет конфигурацию удостоверений приложения в клиенте Azure AD и используется для упрощения авторизации OAuth, предоставления согласия и многого другого.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/18/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e07e371afaa239ca423f4266557cd2f55aa3a55
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59495264"
---
# <a name="azure-active-directory-app-manifest"></a>Манифест приложения Azure Active Directory

Манифест приложения содержит определения всех атрибутов для объекта приложения на платформе удостоверений Майкрософт. Также он служит механизмом для обновления объекта приложения. Дополнительные сведения см. в разделе, посвященном сущности "Приложение" и ее схеме (в [документации по сущности "Приложение API Graph"](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)).

Вы можете настроить атрибуты приложения через портал Azure или программным способом через Microsoft Graph. Но в некоторых сценариях манифест приложения придется редактировать напрямую, чтобы настроить атрибут приложения. Ниже приведены соответствующие сценарии.

* Если вы зарегистрировали приложение c мультитенантной учетной записью AAD и личной учетной записью Майкрософт, вы не сможете изменить в пользовательском интерфейсе поддерживаемые учетные записи Майкрософт. Вместо этого для изменения типа поддерживаемых учетных записей придется использовать редактор манифеста приложения.
* Если вам нужно определить разрешения и роли, которые поддерживает приложение, необходимо изменить манифест приложения.

## <a name="configure-the-app-manifest"></a>Настройка манифеста приложения

Чтобы настроить манифест приложения, выполните следующее.

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите службу **Azure Active Directory**, а затем **Регистрация приложений** или **Регистрация приложений (предварительная версия)**.
1. Выберите приложение, которое вам нужно настроить.
1. На странице **Обзор** выберите раздел **Манифест**. Откроется веб-редактор манифеста, который позволяет изменить манифест, не покидая портал. Также здесь можно выбрать команду **Скачать**, чтобы изменить манифест локально и применить к приложению обновленную версию с помощью команды **Отправить**.

## <a name="manifest-reference"></a>Справка по манифесту

> [!NOTE]
> Если не отображается столбец **Пример значения** после **Описания**, увеличьте окно браузера и прокрутите или проведите пальцем, пока не увидите столбец **Пример значения**.

| Ключ  | Тип значения | ОПИСАНИЕ  | Пример значения |
|---------|---------|---------|---------|
| `accessTokenAcceptedVersion` | Int32, допускающий значение NULL | Указывает версию маркера доступа, ожидаемую ресурсом. В результате изменяются версия и формат JWT, созданного независимо от конечной точки или клиента, используемых для запроса маркера доступа.<br/><br/>Используемая конечная точка версии 1.0 или 2.0 выбирается клиентом и влияет только на версию id_tokens. В ресурсах необходимо явным образом настраивать `accesstokenAcceptedVersion` для указания поддерживаемого формата маркера доступа.<br/><br/>Возможные значения для `accesstokenAcceptedVersion`: 1, 2 или null. Если значение равно null, по умолчанию используется значение 1, соответствующее конечной точке версии 1.0. <br/><br/>Если `signInAudience` является `AzureADandPersonalMicrosoftAccount`, оно должно быть равно `2` | `2` |
| `allowPublicClient` | Логическое | Указывает тип возврата приложения. Azure AD по умолчанию задает тип приложения из replyUrlsWithType. Существуют определенные сценарии, в которых Azure AD не может определить тип клиентского приложения (например, поток [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3), где HTTP-запрос получается без перенаправления URL-адреса). В таких случаях Azure AD будет интерпретировать тип приложения на основе значения этого свойства. Если для этого значения установлено значение true, тип резервного приложения устанавливается как общедоступный клиент, например, установленное приложение, работающее на мобильном устройстве. Значение по умолчанию — false. Это значит, что тип возврата приложения является закрытым клиентом, таким как веб-приложение. | `false` |
| `appId` | Строка идентификатора | Указывает уникальный идентификатор приложения, который назначается приложению в Azure AD. | `"601790de-b632-4f57-9523-ee7cb6ceba95"` |
| `appRoles` | Тип массива | Указывает коллекцию ролей, которые может объявить приложение. Эти роли могут назначаться пользователям, группам или субъектам-службам. Ознакомьтесь с дополнительными сведениями и примерами в статье [Добавление ролей приложения в приложение, зарегистрированное в Azure Active Directory, и их получение в токене](howto-add-app-roles-in-azure-ad-apps.md) | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  |
| `groupMembershipClaims` | строка | Настраивает утверждение `groups`, выданное в маркере пользователя или маркере доступа OAuth 2.0, которого ожидает приложение. Чтобы задать этот атрибут, используйте одно из следующих допустимых строковых значений.<br/><br/>- `"None"`<br/>- `"SecurityGroup"` (группы безопасности и роли Azure AD).<br/>- `"All"` (это предоставит все группы безопасности, группы рассылки и роли каталога Azure AD, членом которых является выполнивший вход пользователь). | `"SecurityGroup"` |
| `optionalClaims` | строка | Необязательные утверждения, возвращаемые в маркере службой токенов безопасности для этого конкретного приложения.<br>На данный момент в приложениях, в которых поддерживаются как личные учетные записи, так и Azure AD (зарегистрированный с помощью портала регистрации приложений), невозможно использовать необязательные утверждения. Тем не менее приложения, которые были зарегистрированы, только чтобы использовать Azure AD и конечную точку версии 2.0, могут получить необязательные утверждения, запрашиваемые в манифесте. Дополнительные сведения см. в статье [Необязательные утверждения в Azure AD (предварительная версия)](active-directory-optional-claims.md). | `null` |
| `id` | Строка идентификатора | Уникальный идентификатор для приложения в каталоге. Это не тот идентификатор, который используется для идентификации приложения в транзакциях протоколов. Он используется для ссылки на объект в запросах к каталогу. | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `identifierUris` | Массив строк | Определяемые пользователем URI, которые уникальным образом идентифицируют веб-приложение в клиенте Azure AD или в проверенном личном домене, если приложение является мультитенантным. | <code>[<br>&nbsp;&nbsp;"https://MyRegisteredApp"<br>]</code> |
| `informationalUrls` | строка | Указывает ссылки на условия предоставления услуг и заявление о конфиденциальности приложения. Условия обслуживания и заявление о конфиденциальности отображаются в окне запроса согласия пользователя. Дополнительные сведения см. в разделе [Практическое руководство. Добавление условий обслуживания и заявления о конфиденциальности для зарегистрированных приложений Azure AD](howto-add-terms-of-service-privacy-statement.md). | <code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> |
| `keyCredentials` | Тип массива | Содержит ссылки на учетные данные, назначенные приложению, строковые общие секреты и сертификаты X.509. Эти учетные данные используются при запросе маркеров доступа (когда приложение работает в качестве клиента, а не ресурса). | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> |
| `knownClientApplications` | Тип массива | Используется для объединения согласия, если у вас есть решение, которое содержит две части: клиентское приложение и пользовательское приложение веб-API. Если в это значение ввести appID клиентского приложения, пользователю нужно будет только единожды предоставить согласие на использование клиентского приложения. Azure AD будет знать, что предоставление клиенту согласия подразумевает неявное одобрение веб-API, и автоматически предоставит субъекты-службы для клиентского приложения и для веб-API одновременно. И клиентское приложение, и веб-API должны быть зарегистрированы в одном и том же клиенте. | `[GUID]` |
| `logoUrl` | строка | Значение "Только для чтения", указывает на URL-адрес CDN логотипа, который был загружен на портале. | `https://MyRegisteredAppLogo` |
| `logoutUrl` | строка | URL-адрес для выхода из приложения. | `https://MyRegisteredAppLogout` |
| `name` | строка | Отображаемое имя приложения. | `MyRegisteredApp` |
| `oauth2AllowImplicitFlow` | Логическое | Определяет, может ли это веб-приложение запрашивать маркеры доступа неявного потока OAuth 2.0. Его значение по умолчанию — false. Этот флаг используется для браузерных приложений, таких как одностраничные приложения JavaScript. Чтобы получить дополнительные сведения, введите `OAuth 2.0 implicit grant flow` в поле над содержанием и просмотрите статьи о неявном потоке. | `false` |
| `oauth2AllowIdTokenImplicitFlow` | Логическое | Определяет, может ли это веб-приложение запрашивать маркеры идентификатора неявного потока OAuth 2.0. Его значение по умолчанию — false. Этот флаг используется для браузерных приложений, таких как одностраничные приложения JavaScript. | `false` |
| `oauth2Permissions` | Тип массива | Указывает коллекцию областей разрешений доступа OAuth 2.0, которые веб-API (ресурс) предоставляет клиентским приложениям. Эти области действия разрешений могут быть назначены клиентским приложениям во время предоставления согласия. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>] </code>|
| `oauth2RequiredPostResponse` | Логическое | Указывает, будет ли Azure AD в рамках запросов маркеров OAuth 2.0 разрешать запросы POST в отличие от запросов GET. Значение по умолчанию — false. В таком случае будут разрешены только запросы GET. | `false` |
| `parentalControlSettings` | строка | `countriesBlockedForMinors` указывает страны, в которых приложение заблокировано для несовершеннолетних.<br>`legalAgeGroupRule` определяет правило возрастной группы, применяемое к пользователям приложения. Можно установить `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids` или `BlockMinors`.  | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> |
| `passwordCredentials` | Тип массива | См. описание свойства `keyCredentials`. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> |
| `preAuthorizedApplications` | Тип массива | Список приложений и запрашиваемых разрешений для косвенного согласия. Требуется, чтобы администратор предоставил согласие для приложения. preAuthorizedApplications не требуют согласия пользователя на запрашиваемые разрешения. Разрешения, перечисленные в preAuthorizedApplications, не требуют согласия пользователя. Тем не менее любые дополнительные запрашиваемые разрешения, не указанные в preAuthorizedApplications, требуют согласия пользователя. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `replyUrlsWithType` | Массив строк | Это свойство с несколькими значениями хранит список зарегистрированных значений redirect_uri, которые Azure AD будет принимать в качестве назначений при возвращении маркеров. Каждое значение URI должно содержать значение типа связанного приложения. Поддерживаются такие значения: `Web`, `InstalledClient`. | <code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> |
| `requiredResourceAccess` | Тип массива | При динамическом согласии `requiredResourceAccess` обеспечивает процедуру использования согласия администратора и согласия пользователя для пользователей, которые используют статическое согласие. Однако не предоставляет процедуру использования согласия пользователя для общего случая.<br>`resourceAppId` — уникальный идентификатор для ресурса, доступ к которому нужен приложению. Это значение должно соответствовать appId, который был объявлен в целевом приложении ресурса.<br>`resourceAccess` — массив, в котором перечислены области разрешений OAuth 2.0 и роли приложений, которые требуются приложению из указанного ресурса. Содержит значения `id` и `type` указанных ресурсов. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>] </code> |
| `samlMetadataUrl` | строка | URL-адрес метаданных SAML для приложения. | `https://MyRegisteredAppSAMLMetadata` |
| `signInUrl` | строка | Указывает URL-адрес домашней страницы приложения. | `https://MyRegisteredApp` |
| `signInAudience` | строка | Указывает, какие учетные записи Майкрософт поддерживаются для текущего приложения. Поддерживаемые значения:<ul><li>**AzureADMyOrg**. Пользователи с учетной записью Майкрософт или учебной учетной записью в клиенте организации Azure AD (т. е. единственный клиент);</li><li>**AzureADMultipleOrgs**. Пользователи с рабочей учетной записью Майкрософт или учебной учетной записью в клиенте любой организации Azure AD (т. е. мультитенантный);</li> <li>**AzureADandPersonalMicrosoftAccount**. Пользователи с личной учетной записью Майкрософт, рабочей или учебной учетной записью в клиенте Azure AD любой организации.</li></ul> | `AzureADandPersonalMicrosoftAccount` |
| `tags` | Массив строк | Пользовательские строки, которые могут использоваться для классификации и идентификации приложения. | <code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code> |


## <a name="manifest-limits"></a>Манифест ограничения
Манифест приложения имеет несколько атрибутов, которые называются коллекциями, например approles, keycredentials, knownClientApplications, identifierUris, rediretUris, requiredResourceAccess, oauth2Permissions и т.д. В рамках полные манифесты приложений для любого приложения общее число записей во всех коллекциях, которые в сочетании были составляет 1200. Если у вас уже есть 100 redirect URI указан в манифесте приложения, то вы являетесь единственным слева с оставшимися 1100 операции для использования во всех коллекциях объединены, который составляющих манифест.

> [!NOTE]
> В случае, если попытаться добавить более чем 1200 записей в манифесте приложения. Вы можете получить ошибку **«не удалось обновить xxxxxx приложения. Сведения об ошибке: Превышен предел для размера манифеста. Уменьшите количество значений и повторите запрос.** "


## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о связи между объектами приложения и субъекта-службы см. в статье [Объекты приложения и субъекта-службы в Azure Active Directory (Azure AD)](app-objects-and-service-principals.md).
* Определения некоторых основных понятий для разработчиков Azure Active Directory (AD) приведены в статье [Глоссарий по Azure Active Directory для разработчика](developer-glossary.md).

Оставляйте свои замечания и пожелания в разделе ниже. Ваши отзывы помогают нам улучшать содержимое веб-сайта.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
