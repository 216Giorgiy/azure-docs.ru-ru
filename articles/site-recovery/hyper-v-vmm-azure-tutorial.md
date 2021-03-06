---
title: Настройка аварийного восстановления в Azure для локальных виртуальных машин Hyper-V в облаках VMM с помощью Azure Site Recovery | Документация Майкрософт
description: Узнайте, как настроить аварийное восстановление в Azure для локальных виртуальных машин Hyper-V в облаках System Center VMM с помощью службы Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 64559f653ba8a466de7bec10db34383b508e3e4b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59361294"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Настройка аварийного восстановления в Azure для локальных виртуальных машин Hyper-V в облаках VMM

В этой статье описывается, как включить репликацию для виртуальных машин в локальной Hyper-V, управляемых по System Center Virtual Machine Manager (VMM), для аварийного восстановления в Azure с помощью [Azure Site Recovery](site-recovery-overview.md) службы. Если вы не используете VMM, затем [следуйте указаниям этого учебника](hyper-v-azure-tutorial.md).

Это третье руководство в серии, в котором показано, как настроить аварийное восстановление для локальных виртуальных машин VMware в Azure. В предыдущем учебном курсе мы [Подготовив среду Hyper-V в локальной](hyper-v-prepare-on-premises-tutorial.md) для аварийного восстановления в Azure. 

Из этого руководства вы узнаете, как выполнять следующие задачи:


> [!div class="checklist"]
> * Выбор источника и целевого объекта репликации.
> * Настройка среды исходной репликации, включая локальные компоненты Site Recovery и целевую среду репликации.
> * Настройка сетевого сопоставления между сетями виртуальных машин VMM и виртуальными сетями Azure.
> * Создание политики репликации
> * Включение репликации для виртуальных машин.


> [!NOTE]
> В учебниках описан самый простой способ развертывания для определенного сценария. В них везде, где возможно, используются значения по умолчанию, и описаны не все возможные параметры и пути. Подробные инструкции см. в разделе с инструкциями в материалах по Site Recovery.

## <a name="before-you-begin"></a>Перед началом работы

Это третье руководство в серии. В этом руководстве предполагается, что вы уже выполнили задачи в предыдущих руководствах:

1. [Подготовка Azure](tutorial-prepare-azure.md).
2. [Подготовка локальных Hyper-V](tutorial-prepare-on-premises-hyper-v.md) это третье руководство в серии. В этом руководстве предполагается, что вы уже выполнили задачи в предыдущих руководствах:


## <a name="select-a-replication-goal"></a>Выбор цели репликации

1. В колонке **Хранилища служб восстановления** выберите хранилище. Мы подготовили хранилище **ContosoVMVault** в предыдущем учебнике.
2. В разделе **Приступая к работе** щелкните **Site Recovery**. Затем выберите **Подготовка инфраструктуры**.
3. В **цель защиты** > **которых вашей машины, расположенные?** выберите **локальной**.
4. В разделе **Куда следует реплицировать компьютеры?** выберите **To Azure** (В Azure).
5. В разделе **Ваши компьютеры виртуализированы?** выберите **Yes, with Hyper-V** (Да, с помощью Hyper-V).
6. В разделе **Are you using System Center VMM** (Используете ли вы System Center VMM?) выберите **Да**. Нажмите кнопку **ОК**.

    ![Цель репликации](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)


## <a name="confirm-deployment-planning"></a>Подтверждение планирования развертывания

1. Если вы планируете выполнить крупномасштабное развертывание, скачайте Планировщик развертывания для Hyper-V, перейдя по ссылке, указанной на странице **Планирование развертывания**. [Ознакомьтесь с дополнительными сведениями](hyper-v-deployment-planner-overview.md) о планировании развертывания Hyper-V.
2. В рамках этого учебника мы не будем использовать планировщик развертывания. В **вы выполнили Планирование развертывания?** выберите **выполню позже**. Нажмите кнопку **ОК**.


## <a name="set-up-the-source-environment"></a>Настройка исходной среды

При настройке исходной среды, вы установите поставщик Azure Site Recovery на сервере VMM и зарегистрируйте сервер в хранилище. Установите агент служб восстановления Azure на каждом узле Hyper-V. 

1. В разделе **Подготовка инфраструктуры** щелкните **Источник**.
2. В поле **Подготовка источника** щелкните **+ VMM**, чтобы добавить сервер VMM. В колонке **Добавление сервера** в поле **Тип сервера** должно быть указано **System Center VMM server** (Сервер System Center VMM).
3. Скачайте программу установки поставщика Microsoft Azure Site Recovery.
4. Скачайте ключ регистрации хранилища. Он понадобится при запуске программы установки поставщика. Ключ действителен в течение пяти дней после создания.
5. Загрузите установщик для агента служб восстановления Microsoft Azure.

    ![Download (Скачать)](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Установка поставщика на сервер VMM

1. В мастере установки поставщика Azure Site Recovery в разделе **Центр обновления Майкрософт** согласитесь на использование Центра обновления Майкрософт для проверки наличия обновлений поставщика.
2. На странице **Установка** примите расположение установки по умолчанию для поставщика и нажмите кнопку **Установить**. 
3. После установки в мастере регистрации Microsoft Azure Site Recovery в разделе **Параметры хранилища** щелкните **Обзор**, а в поле **Key file** выберите скачанный файл ключа хранилища.
4. Укажите подписку Azure Site Recovery и имя хранилища (**ContosoVMVault**). Укажите понятное имя для сервера VMM, позволяющее идентифицировать его в хранилище.
5. В разделе **Параметры прокси** выберите **Подключиться к Azure Site Recovery напрямую без прокси-сервера**.
6. Примите расположение по умолчанию для сертификата, который используется для шифрования данных. После отработки отказа зашифрованные данные будут расшифрованы.
7. В поле **Синхронизация метаданных в облаке** выберите **Синхронизировать метаданные на портале Site Recovery**. На каждом сервере это действие требуется выполнять только один раз. Щелкните **Зарегистрировать**.
8. После регистрации сервера в хранилище нажмите кнопку **Готово**.

Когда регистрация завершится, Azure Site Recovery извлечет метаданные с сервера, а сервер VMM отобразится в колонке **Инфраструктура Site Recovery**.

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Установка агента служб восстановления на узлах Hyper-V

Установите агент на каждом узле Hyper-V, содержащем виртуальные машины, которые требуется реплицировать.

1. В окне мастера настройки агента служб восстановления Microsoft Azure **Проверка предварительных требований** нажмите кнопку **Далее**. Все отсутствующие необходимые компоненты будут установлены автоматически.
2. В разделе **Параметры установки** примите место установки и расположение кэша. Диску кэша требуется по крайней мере 5 ГБ места для хранения. Мы рекомендуем диск с 600 ГБ свободного места (минимум). Нажмите **Install**(Установить).
3. На странице **Установка** после завершения установки нажмите кнопку **Закрыть** для завершения работы мастера.

    ![Установка агента](./media/hyper-v-vmm-azure-tutorial/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>Настройка целевой среды

1. Выберите **Подготовка инфраструктуры** > **Цель**.
2. Выберите подписку и группу ресурсов (**ContosoRG**), в которых будут создаваться виртуальные машины Azure после отработки отказа.
3. Выберите **Resource Manager** модели развертывания.

Site Recovery проверяет наличие одной или нескольких совместимых учетных записей хранения и сетей Azure.


## <a name="configure-network-mapping"></a>Настройка сетевого сопоставления

1. Выберите **Site Recovery Infrastructure** (Инфраструктура Site Recovery) > **Сетевые сопоставления** > **Сетевое сопоставление** и щелкните значок **+Network Mapping** (+Сетевое сопоставление).
2. В разделе **Add network mapping** (Добавить сетевое сопоставление) выберите исходный сервер VMM. Выберите **Azure** в качестве целевой среды.
3. Проверьте подписку и модель развертывания после отработки отказа.
4. В поле **Исходная сеть** выберите исходную локальную сеть виртуальной машины.
5. На странице **Целевая сеть** выберите сеть Azure, в которой будут размещены реплицированные виртуальные машины Azure. Нажмите кнопку **ОК**.

    ![Сетевое сопоставление](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Настройка политики репликации

1. Щелкните **Prepare infrastructure** (Подготовка инфраструктуры) > **Параметры репликации** > **+Create and associate** (+Создание и связывание)
2. На странице **Создать и связать политику**укажите имя политики. Мы используем **ContosoReplicationPolicy**.
3. Оставьте параметры по умолчанию и нажмите кнопку **ОК**.
    - В поле **Периодичность копирования** указывается, что разностные данные (после начальной репликации) будут реплицироваться каждые пять минут.
    - **Хранение точки восстановления** указывает для каждой точки восстановления будут храниться в течение двух часов.
    - В поле **Периодичность создания моментальных снимков с согласованием приложений** указывается, что точки восстановления, содержащие моментальные снимки, согласованные на уровне приложений, будут создаваться ежечасно.
    - В поле **Время запуска начальной репликации** указывается, что начальная репликация начнется немедленно.
    - В поле **Шифровать данные, хранящиеся в Azure** значение по умолчанию **Отключено** указывает, что статические данные в Azure не шифруются.
4. После создания политики нажмите кнопку **OК**. При создании политики она автоматически сопоставляется с облаком VMM.

## <a name="enable-replication"></a>Включение репликации

1. В разделе **Репликация приложения** щелкните **Источник**. 
2. В поле **Source** (Источник) выберите облако VMM. Нажмите кнопку **ОК**.
3. В разделе **Цель** убедитесь, что в качестве целевого объекта выбрана платформа Azure, а также проверьте подписку хранилища и выберите модель **Resource Manager**.
4. Выберите учетную запись хранения **contosovmsacct1910171607** и сеть Azure **ContosoASRnet**.
5. Щелкните **Виртуальные машины** > **Выбрать** и выберите виртуальную машину, которую необходимо реплицировать. Нажмите кнопку **ОК**.

   Ход выполнения действия **включения защиты** можно отслеживать, выбрав **Задания** > **Задания Azure Site Recovery**. После завершения задания **Завершение подготовки защиты** начальная репликация завершается, а виртуальная машина готова к отработке отказа.



## <a name="next-steps"></a>Дальнейшие действия
> [!div class="nextstepaction"]
> [Run a disaster recovery drill to Azure](tutorial-dr-drill-azure.md) (Выполнение отработки аварийного восстановления в Azure)
