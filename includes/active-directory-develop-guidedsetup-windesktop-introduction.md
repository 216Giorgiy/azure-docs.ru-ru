---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: e99d4f6676795a846b48ccd2a08856f4d32f269f
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2018
ms.locfileid: "36205643"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Вызов API Microsoft Graph из классического приложения для Windows

В этом руководстве описывается, как классическое приложение для Windows .NET (XAML) может получить маркер доступа и вызвать API Microsoft Graph или другие API, требующие маркеры доступа от конечной точки Azure Active Directory версии 2.

Когда вы завершите работу с руководством, ваше приложение сможет вызывать защищенный API, использующий личные учетные записи (в том числе outlook.com, live.com и другие). Приложение будет также использовать рабочие и учебные учетные записи из любой компании или организации, использующей Azure Active Directory.  

> [!NOTE] 
> Для работы с руководством требуется Visual Studio 2015 с обновлением 3 или Visual Studio 2017.  У вас нет ни одной из этих версий? [Скачайте Visual Studio 2017 бесплатно.](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Как работает пример приложения, созданный в этом руководстве

![Принцип работы с руководством](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

Пример приложения, созданный в этом руководстве, позволяет классическому приложению для Windows выполнять запрос к API Microsoft Graph или веб-API, принимающему маркеры от конечной точки Azure Active Directory версии 2. В этом сценарии маркер добавляется в запросы HTTP с помощью заголовка авторизации. Получение маркера и его обновление выполняет библиотека проверки подлинности Майкрософт (MSAL).

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Обработка получения маркера для доступа к защищенным веб-интерфейсам API

После того как пользователь пройдет проверку подлинности, пример приложения получит маркер, который может использоваться для запроса API Microsoft Graph или веб-API, защищенного с помощью Azure Active Directory версии 2.

Программным интерфейсам, таким как Microsoft Graph, для доступа к определенным ресурсам требуется маркер. Например, маркер необходим для чтения профиля пользователя, доступа к календарю пользователя и отправки электронной почты. Приложение может запросить маркер доступа с помощью MSAL, чтобы получить доступ к этим ресурсам, указав определенные области API. Затем этот маркер доступа добавляется в заголовок авторизации HTTP для каждого вызова к защищенному ресурсу. 

MSAL управляет кэшированием и обновлением маркеров доступа, поэтому вашему приложению не нужно этого делать.

## <a name="nuget-packages"></a>Пакеты NuGet

В этом руководстве используются следующие пакеты NuGet:

|Библиотека|ОПИСАНИЕ|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Библиотека проверки подлинности Майкрософт (MSAL)|

