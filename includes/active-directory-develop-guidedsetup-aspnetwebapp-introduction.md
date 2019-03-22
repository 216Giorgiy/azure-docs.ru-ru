---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 5a1ff10901d10fb274a6fa1418f04e1f20113cb7
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203638"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Добавление возможности входа в веб-приложение ASP.NET с помощью учетной записи Майкрософт

В этом руководстве описывается, как реализовать вход через учетную запись Майкрософт, используя решение ASP.NET MVC с традиционным браузерным приложением с помощью OpenID Connect.

В конце этого руководства ваше приложение сможет принимать операции входа с помощью личных учетных записей (включая outlook.com, live.com и другие), а также рабочих и учебных учетных записей из любой компании или организации, которая использует Azure Active Directory.

> Для работы с этим руководством требуется Visual Studio 2015 с обновлением 3 или Visual Studio 2017.  У вас его нет?  [Скачайте Visual Studio 2017 бесплатно.](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Как работает пример приложения, созданный в этом руководстве

![Показано, как в примере приложения создаются, это работает, учебники](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral-updated.png)

Пример приложения, создающегося в рамках этого руководства, основан на сценарии, в котором пользователь с помощью браузера получает доступ к веб-сайту ASP.NET. При этом пользователь должен выполнить аутентификацию, воспользовавшись кнопкой входа. В этом сценарии большая часть работы по отображению веб-страницы происходит на стороне сервера.

## <a name="libraries"></a>Библиотеки

В этом руководстве используются следующие библиотеки:

|Библиотека|ОПИСАНИЕ|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Промежуточный слой, который позволяет приложению использовать OpenIDConnect для проверки подлинности.|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Промежуточный слой, который позволяет приложению поддерживать пользовательский сеанс с помощью файлов cookie.|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Позволяет приложениям на основе OWIN работать на платформе IIS с помощью конвейера запросов ASP.NET.|
