---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 703a8ee989a4655768e09440ed250ade6c000050
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58114990"
---
## <a name="setting-up-your-web-server-or-project"></a>Настройка веб-сервера или проекта

> Хотите скачать этот пример проекта вместо указанного выше проекта?
> - [Скачайте файлы проекта](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) для локального веб-сервера, например Node.
>
> или
> - [Скачайте проект Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)
>
> Затем перейдите к [настройке](#register-your-application), чтобы настроить пример кода перед выполнением.

## <a name="prerequisites"></a>Технические условия
Для работы с этим руководством требуется локальный веб-сервер, например [Node.js](https://nodejs.org/en/download/) или [.NET Core](https://www.microsoft.com/net/core), либо интеграция IIS Express с [Visual Studio 2017](https://www.visualstudio.com/downloads/).

Инструкции в этом руководстве основаны на Node.js и Visual Studio 2017, но вы можете использовать любую другую среду разработки или веб-сервер.

## <a name="create-your-project"></a>Создание проекта

> ### <a name="option-1-node-other-web-servers"></a>Вариант 1. Узел или иное веб-серверы
> Убедитесь, что вы установили [Node.js](https://nodejs.org/en/download/), а затем выполните следующие шаги:
> - Создайте папку для размещения приложения.

<p/><!-- -->

> ### <a name="option-2-visual-studio"></a>Вариант 2. Visual Studio
> Если вы создаете новый проект, используя Visual Studio, то выполните следующие действия, чтобы создать новое решение Visual Studio:
> 1.    В Visual Studio:  **Файл > Создать > проект**
> 2.    В разделе **Visual C#\Web** выберите **Веб-приложение ASP.NET (.NET Framework)**.
> 3.    Введите название приложения и нажмите кнопку **ОК**.
> 4.    В разделе **Создание веб-приложения ASP.NET** выберите **Пусто**


## <a name="create-your-single-page-applications-ui"></a>Создание пользовательского интерфейса одностраничного приложения
1. Создайте файл `index.html` для своего одностраничного приложения JavaScript. Если вы используете Visual Studio, выберите проект (корневую папку проекта), щелкните правой кнопкой мыши и выберите: **Добавить > новый элемент > HTML-страницу** и присвойте странице имя index.html.

2. Добавьте в страницу следующий код:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.js"></script>
       <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```
