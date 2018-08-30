---
title: Создание веб-приложения C# ASP.NET Core в Azure | Документация Майкрософт
description: Узнайте, как запустить веб-приложение в службе приложений Azure, развернув веб-приложение C# ASP.NET по умолчанию.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/29/2018
ms.author: cephalin
ms.custom: mvc, devcenter, vs-azure
ms.openlocfilehash: d7b93c28bf83e468d1470b0962dcf9d87a52adb2
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189582"
---
# <a name="create-an-aspnet-core-web-app-in-azure"></a>Создание веб-приложения ASP.NET Core в Azure

> [!NOTE]
> В этой статье мы развернем приложение в службе приложений на платформе Windows. Чтобы развернуть приложение .NET Core в службе приложений на платформе _Linux_, см. статью [Создание веб-приложения .NET Core в службе приложений на платформе Linux](./containers/quickstart-dotnetcore.md).
>

[Веб-приложения Azure](app-service-web-overview.md) — это служба веб-размещения с самостоятельной установкой исправлений и высоким уровнем масштабируемости.  В этом кратком руководстве рассматривается развертывание первого веб-приложения ASP.NET Core при помощи функции Azure "Веб-приложения". В результате будет создана группа ресурсов, состоящая из плана службы приложений и развернутого веб-приложения Azure.

![](./media/app-service-web-get-started-dotnet/web-app-running-live.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством:

Установите <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017</a> с рабочей нагрузкой **ASP.NET и веб-разработка**.

Если вы уже установили Visual Studio, включите рабочие нагрузки. Для этого последовательно выберите **Инструменты** > **Получить инструменты и функции**.

## <a name="create-an-aspnet-core-web-app"></a>Создание веб-приложения ASP.NET Core

Создайте проект в Visual Studio, последовательно выбрав пункты **Файл > Создать > Проект**. 

В диалоговом окне **Новый проект** последовательно выберите пункты **Visual C# > Веб > Веб-приложение ASP.NET Core**.

Присвойте приложению имя _myFirstAzureWebApp_ и нажмите кнопку **ОК**.
   
![Диалоговое окно "Новый проект"](./media/app-service-web-get-started-dotnet/new-project.png)

Вы можете развернуть в Azure веб-приложения ASP.NET Core любого типа. Для примера в этом руководстве выберите шаблон **Веб-приложение** и задайте для аутентификации значение **Без аутентификации**.
      
Нажмите кнопку **ОК**.

![Диалоговое окно "Новый проект ASP.NET"](./media/app-service-web-get-started-dotnet/razor-pages-aspnet-dialog.png)

В меню последовательно выберите пункты **Отладка > Запуск без отладки**, чтобы запустить веб-приложение локально.

![Локальный запуск приложения](./media/app-service-web-get-started-dotnet/razor-web-app-running-locally.png)

## <a name="publish-to-azure"></a>Публикация в Azure

Щелкните правой кнопкой мыши проект **myFirstAzureWebApp** в **обозревателе решений** и выберите **Опубликовать**.

![Публикация в обозревателе решений](./media/app-service-web-get-started-dotnet/right-click-publish.png)

Выберите **Служба приложений Microsoft Azure** и нажмите кнопку **Опубликовать**.

![Публикация с помощью страницы обзора проекта](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Откроется диалоговое окно **Создать службу приложений**, с помощью которого вы можете создать все ресурсы Azure, необходимые для запуска веб-приложения ASP.NET Core в Azure.

## <a name="sign-in-to-azure"></a>Вход в Azure

В диалоговом окне **Создание службы приложений** щелкните **Добавить учетную запись**, а затем войдите в подписку Azure. Если вы уже вошли в систему, выберите учетную запись, содержащую необходимую подписку, в раскрывающемся меню.

> [!NOTE]
> Если вы уже выполнили вход, пока не нажимайте кнопку **Создать**.
>
   
![Вход в Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a>Создание группы ресурсов

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

Рядом с **группой ресурсов** выберите команду **Создать**.

Присвойте группе ресурсов имя **myResourceGroup** и нажмите кнопку **ОК**.

## <a name="create-an-app-service-plan"></a>Создание плана службы приложений

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Рядом с **планом службы приложений** выберите команду **Создать**. 

В диалоговом окне **Настроить план службы приложений** используйте параметры в таблице, которая находится под снимком экрана.

![Создание плана службы приложений](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| Параметр | Рекомендуемое значение | ОПИСАНИЕ |
|-|-|-|
|План обслуживания приложения| myAppServicePlan | Имя плана службы приложений. |
| Расположение | Западная Европа | Центр обработки данных, где размещается веб-приложение. |
| Размер | Free | [Ценовая категория](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) определяет возможности размещения. |

Нажмите кнопку **ОК**.

## <a name="create-and-publish-the-web-app"></a>Создание и публикация веб-приложения

В поле **Имя веб-приложения** введите уникальное имя (допустимые символы: `a-z`, `0-9` и `-`) или примите уникальное имя, созданное автоматически. URL-адрес веб-приложения: `http://<app_name>.azurewebsites.net`, где `<app_name>` — имя веб-приложения.

Нажмите кнопку **Создать**, чтобы начать создавать ресурсы Azure.

![Настройка имени веб-приложения](./media/app-service-web-get-started-dotnet/web-app-name.png)

Когда мастер завершит работу, веб-приложение ASP.NET Core будет опубликовано в Azure и запущено в браузере по умолчанию.

![Опубликованное веб-приложение ASP.NET в Azure](./media/app-service-web-get-started-dotnet/web-app-running-live.png)

Имя веб-приложения, указанное [на этапе создания и публикации](#create-and-publish-the-web-app), используется как префикс URL-адреса в формате `http://<app_name>.azurewebsites.net`.

Поздравляем, ваше веб-приложение ASP.NET Core работает в службе приложений Azure в режиме реального времени.

## <a name="update-the-app-and-redeploy"></a>Обновление и повторное развертывание приложения

В **обозревателе решений** откройте _Pages/Index.cshtml_.

Найдите тег HTML `<div id="myCarousel" class="carousel slide" data-ride="carousel" data-interval="6000">` в верхней области и замените его следующим кодом:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Чтобы выполнить повторное развертывание в Azure, щелкните правой кнопкой мыши проект **myFirstAzureWebApp** в **обозревателе решений**, а затем выберите **Опубликовать**.

На странице сводных сведений публикации выберите **Опубликовать**.
![Страница сводных сведений публикации в Visual Studio](./media/app-service-web-get-started-dotnet/publish-summary-page.png)

По завершении публикации Visual Studio открывает в браузере страницу с URL-адресом веб-приложения.

![Обновленное веб-приложение ASP.NET в Azure](./media/app-service-web-get-started-dotnet/web-app-running-live-updated.png)

## <a name="manage-the-azure-web-app"></a>Управление веб-приложением Azure

Перейдите на <a href="https://portal.azure.com" target="_blank">портал Azure</a> для управления веб-приложением.

В меню слева выберите **Службы приложений**, а затем щелкните имя своего веб-приложения Azure.

![Переход к веб-приложению Azure на портале](./media/app-service-web-get-started-dotnet/access-portal.png)

Отобразится страница обзора вашего веб-приложения. Вы можете выполнять базовые задачи управления: обзор, завершение, запуск, перезагрузку и удаление. 

![Колонка службы приложений на портале Azure](./media/app-service-web-get-started-dotnet/web-app-blade.png)

В меню слева доступно несколько страниц для настройки приложения. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Использование ASP.NET Core с базой данных SQL](app-service-web-tutorial-dotnetcore-sqldb.md)
