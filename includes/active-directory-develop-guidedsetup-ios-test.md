---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: e48aaa03463bb968ec366b27308ca908aab30d99
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805003"
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Отправка тестового запроса к API Microsoft Graph из приложения iOS

Чтобы запустить код в имитаторе, нажмите клавиши **Command** + **R**. (Команда + R).

![Тестирование приложения в симуляторе](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Когда будете готовы выполнить тестирование, выберите **Call Microsoft Graph API** (вызвать API Graph Microsoft). При появлении запроса введите имя пользователя и пароль.

### <a name="provide-consent-for-application-access"></a>Предоставление разрешения на доступ к приложению

После первого входа в приложение предоставьте ему разрешение на использование данных вашего профиля для входа:

![Предоставление разрешения на доступ к приложению](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Просмотр результатов приложения

После входа вы увидите сведения о профиле пользователя, полученные в результате вызова API Microsoft Graph, в разделе **Logging** (Ведение журнала).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Дополнительные сведения об областях и делегированных разрешениях

Для чтения профиля пользователя API Microsoft Graph требуется область **user.read**. По умолчанию эта область автоматически добавляется в каждое приложение, зарегистрированное на портале регистрации. Для других API Microsoft Graph, а также для пользовательских API вашего внутреннего сервера, могут потребоваться дополнительные области. Для отображения списка календарей пользователя API Microsoft Graph требуется область **Calendars.Read**.

Чтобы из контекста приложения получить доступ к календарям пользователя, добавьте делегированное разрешение **Calendars.Read** в сведения о регистрации приложения. Затем добавьте область **Calendars.Read** в вызов **acquireTokenSilent**.

>[!NOTE]
>При увеличении количества областей от пользователя могут потребоваться дополнительные согласия.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
