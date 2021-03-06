---
title: Создавайте бессерверные приложения с помощью Azure Logic Apps и функции Azure в Visual Studio
description: Сведения о том, как в Visual Studio создать, развернуть и администрировать свое первое простое бессерверное приложение на основе Azure Logic Apps и Функций Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 04/02/2019
ms.openlocfilehash: 39b44668a89ce0c77c09a7fa20dc4d95b2164bf4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "58863004"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Создание первого бессерверного приложения на основе Azure Logic Apps и Функций Azure (Visual Studio)

Вы можете быстро разработать и развернуть облачное приложение с помощью бессерверных средств и возможностей в Azure, таких как [Azure Logic Apps](../logic-apps/logic-apps-overview.md) и [Функции Azure](../azure-functions/functions-overview.md). В этой статье показано, как в Visual Studio приступить к созданию бессерверного приложения на основе приложения логики, которое вызывает функцию Azure. Дополнительные сведения о бессерверных решениях в Azure см. в статье [Обзор бессерверных компонентов Azure с функциями и Logic Apps](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Технические условия

Для создания бессерверного приложения в Visual Studio вам потребуется следующее:

* Подписка Azure. Если у вас еще нет подписки Azure, <a href="https://azure.microsoft.com/free/" target="_blank">зарегистрируйтесь для получения бесплатной учетной записи Azure</a>.

* Скачайте и установите эти средства, если вы еще этого не сделали:

  * <a href="https://aka.ms/download-visual-studio" target="_blank">Visual Studio 2019, 2017 или 2015 — выпуски Community или выше</a>. 
  В этом кратком руководстве используется бесплатная версия Visual Studio Community 2017.

    > [!IMPORTANT]
    > При установке Visual Studio 2019 или 2017 обязательно выберите рабочую нагрузку **разработки Azure**.
    > Если используется Visual Studio 2019, Cloud Explorer может открыть конструктор приложений логики на портале Azure. Открытие встроенного конструктора приложений логики пока не поддерживается.

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">Пакет Microsoft Azure SDK для .NET (2.9.1 или более поздней версии)</a>. Дополнительные сведения о <a href="https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet">пакете Azure SDK для .NET</a>.

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Средства Azure Logic Apps для необходимой версии Visual Studio:

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019" target="_blank">Visual Studio 2019 г.</a>

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017" target="_blank">Visual Studio 2017</a>

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015" target="_blank">Visual Studio 2015</a>
  
    Вы можете скачать и установить средства Azure Logic Apps напрямую из Visual Studio Marketplace или узнать, <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">как установить это расширение из Visual Studio</a>. 
    После завершения установки перезагрузите Visual Studio.

  * <a href="https://www.npmjs.com/package/azure-functions-core-tools" target="_blank">Основные инструменты службы "Функции Azure"</a> для локальной отладки функций.

* Доступ к Интернету при использовании встроенного конструктора приложений логики

  Конструктору требуется подключение к Интернету, чтобы создать ресурсы в Azure и считать свойства и данные из соединителей в приложении логики. 
  Например, если вы используете соединитель Dynamics CRM Online, конструктор проверяет экземпляр CRM и получает информацию для отображения доступных свойств по умолчанию и пользовательских свойств.

## <a name="create-resource-group-project"></a>Создание проекта группы ресурсов

Чтобы начать работу, создайте для бессерверного приложения [проект группы ресурсов Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Ресурсы в Azure создаются в группе ресурсов, которая представляет собой логическую коллекцию для организации, развертывания ресурсов приложения и управления ими как единым ресурсом. Группа ресурсов для бессерверного приложения в Azure содержит ресурсы для Azure Logic Apps и службы "Функции Azure". Узнайте больше о [группах ресурсов Azure и ресурсах](../azure-resource-manager/resource-group-overview.md).

1. Запустите Visual Studio и войдите в учетную запись Azure.

1. В меню **Файл** выберите **Создать** > **Проект**.

   ![Создание проекта в Visual Studio](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. В разделе **Установленные** выберите **Visual C#** или **Visual Basic**. Выберите **Облако** > **Группа ресурсов Azure**.

   > [!NOTE]
   > Если не существует категория **Cloud** или проект **Группа ресурсов Azure**, убедитесь, что у вас установлен пакет Azure SDK для Visual Studio.

   Если вы используете Visual Studio 2019, сделайте следующее:

   1. В поле **Создать проект** выберите шаблон проекта **Группа ресурсов Azure** для языка C# или Visual Basic, а затем нажмите кнопку **Далее**.

   1. Укажите имя группы ресурсов Azure, которую вы хотите использовать, и другие сведения о проекте. Когда все будет готово, выберите **Создать**.

1. Укажите имя и расположение проекта, а затем щелкните **ОК**.

   Visual Studio предложит выбрать шаблон из списка шаблонов. 
   В этом примере используется шаблон быстрого запуска Azure, поэтому вы можете создавать бессерверные приложение, которое включает в себя приложение логики и вызова функции Azure.

   > [!TIP]
   > В сценариях, где вы не хотите заранее развертывать решения в группе ресурсов Azure, можно использовать пустую **приложение логики** шаблона, который просто создает пустое приложение логики.

1. Из **Показать шаблоны из этого расположения** выберите **быстрого запуска Azure (github.com/Azure/azure-quickstart-templates)**.

1. В поле поиска введите «приложение логики» в качестве фильтра. В результатах выберите этот шаблон: **101-logic-app-and-function-app**.

   ![Выбор шаблона быстрого запуска Azure](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio создаст и откроет решение для проекта группы ресурсов. 
   Шаблон быстрого запуска Azure, вы выбрали создает шаблон развертывания с именем `azuredeploy.json` внутри проекта группы ресурсов. Этот шаблон развертывания включает определение простого приложения логики, которое запускает HTTP-запрос, вызывает функцию Azure и возвращает результат в виде HTTP-ответа.

   ![Новое бессерверное решение](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Теперь это решение следует развернуть в Azure, и тогда вы сможете открыть шаблон развертывания и просмотреть ресурсы для бессерверного приложения.

## <a name="deploy-your-solution"></a>Развертывание решения

Чтобы открыть приложение логики в соответствующем конструкторе в Visual Studio, нужно развернуть в Azure группу ресурсов. Это позволит конструктору создавать подключения к ресурсам и службам из приложения логики. Для этого разверните решение с помощью Visual Studio на портал Azure.

1. В обозревателе решений щелкните проект ресурсов контекстном меню выберите **развернуть** > **New**.

   ![Создание развертывания для группы ресурсов](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Выберите подписку Azure, если она еще не выбрана, и группу ресурсов для предстоящего развертывания. Выберите **Развернуть**.

   ![Параметры развертывания](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Если появится диалоговое окно **Изменение параметров**, укажите имя ресурса для приложения логики и приложение-функцию Azure для этого развертывания, а затем сохраните введенные параметры. Используйте для приложения-функции глобально уникальное имя.

   ![Ввод имен для приложения логики и приложения-функции](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Когда Visual Studio запускает развертывание решения в указанной группе ресурсов, состояние этого развертывания отображается в **окне вывода** Visual Studio. 
   После завершения развертывания приложение логики активируется на портале Azure.

## <a name="edit-logic-app-in-visual-studio"></a>Изменение приложения логики в Visual Studio

Теперь, когда решение развертывается в группе ресурсов, откройте приложение логики с помощью конструктора приложений логики можно редактировать и изменить приложение логики.

1. В обозревателе решений из `azuredeploy.json` контекстное меню файла выберите **открыть с помощью конструктора приложений логики**.

   ![Открытие файла azuredeploy.json в конструкторе приложений логики](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Когда появится окно **свойств приложения логики**, выберите нужную подписку Azure в разделе **Подписки**, если она еще не выбрана. В разделе **Группа ресурсов** выберите группу ресурсов и расположение, в котором развернуто решение, и щелкните **ОК**.

   ![Свойства приложения логики](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   В открывшемся окне конструктора приложений логики вы можете добавить новые действия или изменить рабочий процесс, а затем сохранить изменения.

   ![Открытое приложение логики в конструкторе приложений логики](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Создание проекта Функций Azure

Чтобы создать проект Функций Azure и функцию с помощью JavaScript, Python, F#, PowerShell, пакетной службы или Bash, выполните действия, описанные в статье [Запуск основных инструментов службы "Функции Azure"](../azure-functions/functions-run-local.md). Чтобы создать в этом решении функцию Azure на языке C#, можно применить библиотеку классов C# с помощью процедуры, описанной в записи блога [о публикации библиотеки классов .NET в качестве приложения-функции](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/).

## <a name="deploy-functions-from-visual-studio"></a>Развертывание функций из Visual Studio

Шаблон развертывания позволяет развернуть все включенные в решение функции Azure из репозитория Git, который определяется переменными в файле `azuredeploy.json`. Если вы проектируете и создаете в вашем решении проект Функций Azure, его можно передать в систему управления версиями Git, например GitHub или Azure DevOps, а затем указать значение переменной `repo` таким образом, чтобы с этим шаблоном развертывалась нужная функция Azure.

## <a name="manage-logic-apps-and-view-run-history"></a>Управление приложениями логики и просмотр журнала выполнения

Если приложение логики уже развернуто в Azure, вы по-прежнему можете изменять его, управлять им, просматривать журнал выполнения и отключать приложение с помощью Visual Studio.

1. Откройте меню **Представление** в Visual Studio и выберите **Cloud Explorer**.

1. В разделе **Все подписки** выберите подписку Azure, связанную с нужным приложением логики, и щелкните **Применить**.

1. В разделе **Приложения логики** выберите это приложение логики. В контекстном меню для приложения логики выберите **Open with Logic App Editor** (Открыть в редакторе приложений логики).

Теперь опубликованное приложение логики можно скачать в проект группы ресурсов. Таким образом, даже созданное на портале Azure приложение логики можно импортировать, чтобы управлять им с помощью Visual Studio. Дополнительные сведения см. в статье [Управление приложениями логики в Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Управление приложениями логики в Visual Studio](manage-logic-apps-with-visual-studio.md)
