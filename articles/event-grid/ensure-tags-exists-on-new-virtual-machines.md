---
title: Интеграция службы автоматизации Azure со службой "Сетка событий Azure" | Документация Майкрософт
description: Узнайте, как автоматически добавить тег при создании новой виртуальной машины и как отправить уведомление в Microsoft Teams.
keywords: automation, runbook, teams, event grid, virtual machine, VM
services: automation
author: eamonoreilly
manager: ''
ms.service: automation
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: eamono
ms.openlocfilehash: d7113842aff4d6d9ec66439d1d43e5e24333d3f1
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467576"
---
# <a name="tutorial-integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Руководство. Интеграция службы автоматизации Azure со службой "Сетка событий Azure" и Microsoft Teams

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * импорт примера runbook для Сетки событий;
> * создание перехватчика для Microsoft Teams (необязательно);
> * создание веб-перехватчика для runbook;
> * создание подписки в службе "Сетка событий";
> * создание виртуальной машины, которая запускает runbook.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить инструкции из этого руководства, вам понадобится [учетная запись службы автоматизации Azure](../automation/automation-offering-get-started.md), в которой будет храниться runbook для запуска из подписки служба "Сетка событий".

* Модуль `AzureRM.Tags` нужно загрузить в учетной записи службы автоматизации. См. инструкции по [импорту модулей в службе автоматизации Azure](../automation/automation-update-azure-modules.md).

## <a name="import-an-event-grid-sample-runbook"></a>Импорт примера runbook для службы "Сетка событий"

1. Выберите учетную запись службы автоматизации и перейдите на страницу **Runbooks**.

   ![Выбор модулей runbook](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. Нажмите кнопку **Просмотреть коллекцию**.

3. Выполните поиск по запросу **Сетка событий** и выберите элемент **Integrating Azure Automation with Event grid** (Интеграция службы автоматизации Azure с Сеткой событий).

    ![Импорт runbook из коллекции](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Выберите действие **Импорт** и присвойте ему имя **Watch-VMWrite**.

5. Когда импорт успешно завершится, выберите **Изменить**, чтобы просмотреть источник runbook. Нажмите кнопку **Опубликовать**.

> [!NOTE]
> Строку 74 в скрипте нужно заменить строкой `Update-AzureRmVM -ResourceGroupName $VMResourceGroup -VM $VM -Tag $Tag | Write-Verbose`. Параметр `-Tags` теперь имеет значение `-Tag`.

## <a name="create-an-optional-microsoft-teams-webhook"></a>Создание перехватчика для Microsoft Teams (необязательно)

1. В Microsoft Teams выберите элемент **Дополнительные параметры** рядом с именем канала, а затем выберите **Соединители**.

    ![Подключения Microsoft Teams](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Прокрутите список соединителей до элемента **Входящий веб-перехватчик** и выберите для него действие **Добавить**.

3. Введите имя **AzureAutomationIntegration** и нажмите кнопку **Создать**.

4. Скопируйте веб-перехватчик в буфер обмена и сохраните его. URL-адрес веб-перехватчика используется для отправки данных в Microsoft Teams.

5. Выберите **Готово**, чтобы сохранить веб-перехватчик.

## <a name="create-a-webhook-for-the-runbook"></a>Создание веб-перехватчика для runbook

1. Откройте runbook с именем Watch-VMWrite.

2. Выберите **Веб-перехватчики** и нажмите кнопку **Добавить веб-перехватчик**.

3. Введите имя **WatchVMEventGrid**. Скопируйте URL-адрес в буфер обмена и сохраните его.

    ![Настройка имени веб-перехватчика](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. Выберите действие **Настройка параметров и настроек запуска** и введите URL-адрес веб-перехватчика Microsoft Teams в поле **CHANNELURL**. Поле **WEBHOOKDATA** оставьте пустым.

    ![Настройка параметров веб-перехватчика](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Щелкните **Создать**, чтобы создать веб-перехватчик для модуля runbook службы автоматизации.

## <a name="create-an-event-grid-subscription"></a>Создание подписки Сетки событий

1. На странице **Учетная запись службы автоматизации** выберите элемент **Сетка событий**.

    ![Выбор службы "Сетка событий"](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. Нажмите кнопку **+Подписка на события**.

3. Настройте для подписки следующие сведения.

   * В списке **Тип раздела** выберите **Подписки Azure**.
   * Снимите флажок **Подписка на все типы событий**.
   * Введите имя **AzureAutomation**.
   * В раскрывающемся списке **Определенные типы событий** снимите все флажки, кроме **Успешная запись ресурсов**.
   * Для **типа конечной точки** выберите **Веб-перехватчик**.
   * Щелкните **Выбрать конечную точку**. На открывшейся странице **Выбор веб-перехватчика** вставьте URL-адрес веб-перехватчика, созданный для модуля runbook с именем Watch-VMWrite.
   * В разделе **Фильтры** укажите подписку и группу ресурсов, в которых нужно искать новые виртуальные машины. Результат будет выглядеть так: `/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`.

4. Выберите действие **Создать**, чтобы сохранить подписку Сетки событий.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Создание виртуальной машины, которая запускает runbook

1. Создайте новую виртуальную машину в той группе ресурсов, которую вы указали в фильтре префиксов для подписки Сетки событий.

2. Если все настроено правильно, runbook Watch-VMWrite сработает и добавит к этой виртуальной машине новый тег.

    ![Тег виртуальной машины](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Новое сообщение отправляется в канал Microsoft Teams.

    ![Уведомление для Microsoft Teams](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы настроили интеграцию между Сеткой событий и службой автоматизации. Вы научились выполнять следующие задачи:

> [!div class="checklist"]
> * импорт примера runbook для Сетки событий;
> * создание перехватчика для Microsoft Teams (необязательно);
> * создание веб-перехватчика для runbook;
> * создание подписки в службе "Сетка событий";
> * создание виртуальной машины, которая запускает runbook.

> [!div class="nextstepaction"]
> [Создание и перенаправление пользовательского события при помощи службы "Сетка событий"](../event-grid/custom-event-quickstart.md)
