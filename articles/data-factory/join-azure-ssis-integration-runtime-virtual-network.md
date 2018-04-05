---
title: Присоединение среды выполнения интеграции Azure SSIS к виртуальной сети | Документация Майкрософт
description: Узнайте, как присоединить среду выполнения интеграции Azure SSIS к виртуальной сети Azure.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: douglasl
ms.openlocfilehash: cdda3fbe2aff40e26c6086e87ef3e05670c3419f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2018
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Присоединение среды выполнения интеграции Azure SSIS к виртуальной сети
Присоедините среду выполнения интеграции (IR) Azure SSIS к виртуальной сети в одной из следующих ситуаций: 

- Вы размещаете базу данных каталога SQL Server Integration Services (SSIS) в управляемом экземпляре Базы данных SQL Azure (закрытая предварительная версия) в виртуальной сети.
- Вы хотите подключиться к локальным хранилищам данных из пакетов SSIS, работающих в среде выполнения интеграции Azure SSIS.

 Фабрика данных Azure версии 2 (предварительная версия) позволяет присоединить среду выполнения интеграции Azure SSIS к виртуальной сети, созданной с использованием классической модели развертывания или модели развертывания с помощью Azure Resource Manager. 

> [!NOTE]
> Эта статья относится к версии 2 фабрики данных, которая в настоящее время доступна в предварительной версии. Если вы используете общедоступную версию 1 службы фабрики данных, см. статью [Введение в фабрику данных Azure](v1/data-factory-introduction.md).

## <a name="access-to-on-premises-data-stores"></a>Доступ к локальным хранилищам данных
Если пакеты служб SSIS используют доступ только к общедоступным облачным хранилищам данных, нет необходимости присоединять IR Azure SSIS к виртуальной сети. Если пакеты SSIS используют доступ к локальным хранилищам данных, необходимо присоединять IR Azure SSIS к виртуальной сети, подключенной к локальной сети. 

Если каталог SSIS размещен в экземпляре базы данных SQL, которая не находится в виртуальной сети, необходимо открыть соответствующие порты. 

Если каталог SSIS размещен в управляемом экземпляре Базы данных SQL Azure, который находится в виртуальной сети, IR Azure SSIS можно присоединить к:

- той же виртуальной сети;
- другой виртуальной сети, имеющей подключение типа "сеть — сеть" с той сетью, в которой находится управляемый экземпляр базы данных SQL. 

Виртуальная сеть может быть развернута с использованием классической модели развертывания или модели развертывания с помощью Azure Resource Manager. Если вы планируете присоединить IR Azure SSIS к *той же виртуальной сети*, в которой находится управляемый экземпляр Базы данных SQL Azure, IR Azure SSIS должна находиться в *другой подсети*.   

В следующих разделах приведены дополнительные сведения.

Необходимо учитывать следующие важные замечания. 

- Если есть имеющаяся виртуальная сеть, подключенная к локальной сети, то сначала создайте [виртуальную сеть Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) или [классическую виртуальную сеть](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) для присоединения среды выполнения интеграции Azure SSIS. Затем настройте подключение типа "сеть — сеть" через [VPN-шлюз](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) или [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) между этой виртуальной сетью и вашей локальной сетью.
- Если есть существующая виртуальная сеть Azure Resource Manager или классическая виртуальная сеть, подключенная к локальной сети в том же расположении, что и ваша среда выполнения интеграции Azure SSIS, вы можете присоединить IR к этой виртуальной сети.
- Если есть существующая классическая виртуальная сеть, подключенная к локальной сети в расположении, отличном от вашей среды выполнения интеграции Azure SSIS, вы можете сначала создать [классическую виртуальную сеть](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) для присоединения IR Azure SSIS. Затем настройте подключение типа [классическая виртуальная сеть — классическая виртуальная сеть](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md). Или вы можете создать [виртуальную сеть Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network), чтобы присоединить к ней свою среду выполнения интеграции Azure SSIS. Затем настройте подключение типа [классическая виртуальная сеть — виртуальная сеть Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).
- Если существует виртуальная сеть Azure Resource Manager, подключенная к локальной сети в расположении, отличном от вашей среды выполнения интеграции Azure SSIS, вы можете сначала создать [виртуальную сеть Azure Resource Manager](../virtual-network/quick-create-portal.md##create-a-virtual-network) для присоединения IR Azure SSIS. Затем настройте подключение типа "виртуальная сеть Azure Resource Manager — виртуальная сеть Azure Resource Manager". Кроме того, вы можете создать [классическую виртуальную сеть](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) для присоединения IR Azure SSIS. Затем настройте подключение типа [классическая виртуальная сеть — виртуальная сеть Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="domain-name-services-server"></a>Сервер служб доменных имен 
Если необходимо использовать сервер служб доменных имен (DNS) в виртуальной сети, соединенной с помощью среды выполнения интеграции, следуйте инструкциям в статье [Разрешение имен для виртуальных машин и экземпляров ролей](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="network-security-group"></a>Группа безопасности сети
Если необходимо реализовать группу безопасности сети (NSG) в виртуальной сети, присоединенной средой выполнения интеграции Azure SSIS, разрешите входящий и исходящий трафик через следующие порты:

| порты; | Направление | Транспортный протокол | Назначение | Входящий источник/исходящее назначение |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100, 20100, 30100 (при присоединении среды выполнения интеграции к классической виртуальной сети)<br/><br/>29876, 29877 (при присоединении среды выполнения интеграции к виртуальной сети Azure Resource Manager) | Входящий трафик | TCP | Службы Azure используют эти порты для взаимодействия с узлами вашей среды выполнения интеграции Azure SSIS в виртуальной сети. | Интернет | 
| 443 | Исходящие | TCP | Узлы среды выполнения интеграции Azure SSIS в виртуальной сети используют этот порт для доступа к службам Azure, например, к службе хранилища и концентраторам событий Azure. | Интернет | 
| 1433<br/>11000–11999;<br/>14000–14999.  | Исходящие | TCP | Узлы среды выполнения интеграции Azure SSIS в виртуальной сети используют эти порты для доступа к базе данных SSISDB, размещенной на сервере Базы данных SQL Azure (это не относится к базе данных SSISDB, размещенной на управляемом экземпляре Базы данных SQL). | Интернет | 

## <a name="azure-portal-data-factory-ui"></a>Портал Azure (пользовательский интерфейс фабрики данных)
В этом разделе показано, как присоединить имеющуюся среду выполнения Azure SSIS к виртуальной сети (классической виртуальной сети или виртуальной сети Azure Resource Manager) с помощью портала Azure и пользовательского интерфейса фабрики данных. Во-первых, необходимо правильно настроить виртуальную сеть перед присоединением к ней среды выполнения интеграции Azure SSIS. Выполните действия, приведенные в одном из следующих двух разделов, в зависимости от типа виртуальной сети (классическая или Azure Resource Manager). Затем перейдите к третьему разделу, чтобы присоединить среду выполнения интеграции Azure SSIS к виртуальной сети. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Настройка классической виртуальной сети с помощью портала
Перед присоединением среды выполнения интеграции Azure SSIS к виртуальной сети необходимо настроить виртуальную сеть.

1. Запустите Microsoft Edge или Google Chrome. Сейчас пользовательский интерфейс фабрики данных поддерживают только эти браузеры.
2. Войдите на [портале Azure](https://portal.azure.com).
3. Выберите **Больше служб**. Примените фильтр и выберите **Виртуальные сети (классика)**.
4. Примените фильтр и выберите в списке свою виртуальную сеть. 
5. На странице **Виртуальные сети (классика)** выберите **Свойства**. 

    ![Идентификатор ресурса классической виртуальной сети](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Нажмите кнопку копирования напротив поля **Идентификатор ресурса**, чтобы скопировать идентификатор ресурса для классической сети в буфер обмена. Сохраните идентификатор из буфера обмена в OneNote или в файле.
6. В меню слева выберите **Подсети**. Убедитесь, что число **доступных адресов** больше, чем узлов в вашей среде выполнения интеграции Azure SSIS.

    ![Число доступных адресов в виртуальной сети](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Присоедините **MicrosoftAzureBatch** к роли **Участник классической виртуальной машины** для виртуальной сети.

    a. В меню слева выберите **Управление доступом (IAM)** и нажмите кнопку **Добавить** на панели инструментов. 
       ![Кнопки "Управление доступом" и "Добавить"](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    Б. На странице **Добавление разрешений** в поле **Роль** выберите **Участник классической виртуальной машины**. Вставьте **ddbf3205-c6bd-46ae-8127-60eb93363864** в поле **Выбор**, а затем из списка результатов поиска выберите **пакетную службу Microsoft Azure**.   
       ![Результаты поиска на странице "Добавление разрешений"](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    c. Нажмите кнопку **Сохранить**, чтобы сохранить параметры и закрыть страницу.  
       ![Сохранение параметров доступа](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    d. Убедитесь, что в списке участников отображается **пакетная служба Microsoft Azure**.  
       ![Подтверждение доступа к пакетной службе Azure](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

5. Убедитесь, что поставщик пакетной службы Azure зарегистрирован в подписке Azure, которая содержит виртуальную сеть. В противном случае зарегистрируйте этого поставщика. Если у вас в подписке уже есть учетная запись пакетной службы Azure, то ваша подписка зарегистрирована в пакетной службе Azure.

   a. На портале Azure в меню слева выберите **Подписки**.

   Б. Выберите свою подписку.

   c. Выберите **Поставщики ресурсов** в меню слева и убедитесь, что **Microsoft.Batch** является зарегистрированным поставщиком.     
      ![Подтверждение состояния "Зарегистрировано"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Если поставщик **Microsoft.Batch** не отображается в списке, то для его регистрации [создайте в своей подписке пустую учетную запись пакетной службы Azure](../batch/batch-account-create-portal.md). Затем ее можно будет удалить. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Настройка виртуальной сети Azure Resource Manager с помощью портала
Перед присоединением среды выполнения интеграции Azure SSIS к виртуальной сети необходимо настроить виртуальную сеть.

1. Запустите Microsoft Edge или Google Chrome. Сейчас пользовательский интерфейс фабрики данных поддерживают только эти браузеры.
2. Войдите на [портале Azure](https://portal.azure.com).
3. Выберите **Больше служб**. Примените фильтр и выберите **Виртуальные сети**.
4. Примените фильтр и выберите в списке свою виртуальную сеть. 
5. На странице **Виртуальная сеть** выберите **Свойства**. 
6. Нажмите кнопку копирования напротив поля **Идентификатор ресурса**, чтобы скопировать идентификатор ресурса для виртуальной сети в буфер обмена. Сохраните идентификатор из буфера обмена в OneNote или в файле.
7. В меню слева выберите **Подсети**. Убедитесь, что число **доступных адресов** больше, чем узлов в вашей среде выполнения интеграции Azure SSIS.
8. Убедитесь, что поставщик пакетной службы Azure зарегистрирован в подписке Azure, которая содержит виртуальную сеть. В противном случае зарегистрируйте этого поставщика. Если у вас в подписке уже есть учетная запись пакетной службы Azure, то ваша подписка зарегистрирована в пакетной службе Azure.

   a. На портале Azure в меню слева выберите **Подписки**.

   Б. Выберите свою подписку. 
   
   c. Выберите **Поставщики ресурсов** в меню слева и убедитесь, что **Microsoft.Batch** является зарегистрированным поставщиком.     
      ![Подтверждение состояния "Зарегистрировано"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Если поставщик **Microsoft.Batch** не отображается в списке, то для его регистрации [создайте в своей подписке пустую учетную запись пакетной службы Azure](../batch/batch-account-create-portal.md). Затем ее можно будет удалить.

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Присоединение среды выполнения интеграции Azure SSIS к виртуальной сети


1. Запустите Microsoft Edge или Google Chrome. Сейчас пользовательский интерфейс фабрики данных поддерживают только эти браузеры.
2. На [портале Azure](https://portal.azure.com) в меню слева выберите **Фабрики данных**. Если пункт **Фабрики данных** не отображается в меню, щелкните **Больше служб**, а затем выберите **Фабрики данных** в разделе **АНАЛИТИКА**. 
    
   ![Список фабрик данных](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)
2. Выберите из списка фабрику данных со средой выполнения интеграции Azure SSIS. После этого откроется домашняя страница фабрики данных. Выберите плитку **Author & Deploy** (Создать и развернуть). На отдельной вкладке откроется пользовательский интерфейс фабрики данных. 

   ![Домашняя страница фабрики данных](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)
3. В пользовательском интерфейсе фабрики данных перейдите на вкладку **Правка**, щелкните **Подключения**, а затем выберите вкладку **сред выполнения интеграции**. 

   ![Вкладка сред выполнения интеграции](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)
4. Если среда выполнения интеграции Azure SSIS запущена, в списке сред выполнения интеграции нажмите кнопку **Остановить** в столбце **Действия** для нужной среды выполнения интеграции Azure SSIS. Среду выполнения интеграции невозможно изменить, не остановив ее. 

   ![Остановка среды выполнения интеграции](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)
1. В списке сред выполнения интеграции нажмите кнопку **Изменить** в столбце **Действия** для нужной среды выполнения интеграции Azure SSIS.

   ![Изменение среды выполнения интеграции](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)
5. На странице **Общие параметры** окна **настройки среды выполнения интеграции** нажмите кнопку **Далее**. 

   ![Общие параметры для настройки среды выполнения интеграции](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)
6. На странице **параметров SQL** введите пароль администратора и нажмите кнопку **Далее**.

   ![Параметры SQL Server для настройки среды выполнения интеграции](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)
7. На странице **Дополнительные параметры** выполните следующие действия. 

   a. Установите флажок **Select a VNet for your Azure-SSIS Integration Runtime to join and allow Azure services to configure VNet permissions/settings** (Выбрать виртуальную сеть для присоединения среды выполнения интеграции Azure SSIS и разрешить службам Azure настраивать разрешения и параметры виртуальной сети).

   Б. В поле **Тип** укажите тип виртуальной сети (классическая виртуальная сеть или виртуальная сеть Azure Resource Manager). 

   c. В поле **Имя виртуальной сети** выберите нужную виртуальную сеть.

   d. В поле **Имя подсети** выберите нужную подсеть в виртуальной сети.

   д. Нажмите кнопку **Обновить**. 

   ![Дополнительные параметры для настройки среды выполнения интеграции](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)
8. Теперь можно запустить среду выполнения интеграции, нажав кнопку **запуска** в столбце **Действия** для выбранной среды выполнения интеграции Azure SSIS. Запуск IR Azure SSIS занимает приблизительно 20 минут. 


## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>Настройка виртуальной сети
Перед присоединением среды выполнения интеграции Azure SSIS к виртуальной сети необходимо настроить виртуальную сеть. Добавьте следующий скрипт, чтобы автоматически настроить разрешения или параметры виртуальной сети, которую присоединит ваша среда интеграции Azure SSIS.

```powershell
# Register to the Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Создание среды выполнения интеграции Azure SSIS и ее присоединение к виртуальной сети
Вы можете создать среду выполнения интеграции Azure SSIS и присоединить ее к виртуальной сети одновременно. Полный сценарий и инструкции см. в разделе [Azure PowerShell](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Присоединение имеющейся среды выполнения интеграции Azure SSIS к виртуальной сети
В сценарии статьи [Создание среды выполнения интеграции Azure SSIS в фабрике данных Azure](create-azure-ssis-integration-runtime.md) показано, как создать среду выполнения интеграции Azure SSIS и присоединить ее к виртуальной сети в том же сценарии. Если у вас имеется среда выполнения интеграции Azure SSIS, выполните следующие действия, чтобы присоединить ее к виртуальной сети. 

1. Остановите среду выполнения интеграции Azure SSIS.
2. Настройте среду выполнения интеграции Azure SSIS для присоединения виртуальной сети. 
3. Запустите среду выполнения интеграции Azure SSIS. 

### <a name="define-the-variables"></a>Определение переменных

```powershell
$ResourceGroupName = "<Azure resource group name>"
$DataFactoryName = "<Data factory name>" 
$AzureSSISName = "<Specify Azure-SSIS IR name>"
## These two parameters apply if you are using a virtual network and Azure SQL Database Managed Instance (private preview) 
# Specify information about your classic or Azure Resource Manager virtual network.
$VnetId = "<Name of your Azure virtual network>"
$SubnetName = "<Name of the subnet in the virtual network>"
```

#### <a name="guidelines-for-selecting-a-subnet"></a>Рекомендации по выбору подсети
-   Не следует выбирать подсеть шлюза для развертывания среды выполнения интеграции SSIS и Azure, так как она предназначена для шлюзов виртуальных сетей.
-   Убедитесь, что выбранная подсеть содержит достаточный диапазон адресов для работы среды выполнения интеграции SSIS и Azure. Оставьте по крайней мере в два раза больше доступных IP-адресов, чем содержит узлов среда выполнения интеграции. Azure резервирует некоторые IP-адреса в каждой подсети, которые нельзя использовать. Зарезервированы первый и последний IP-адреса подсетей (для соответствия требованиям протокола), а также еще три адреса, используемые для служб Azure. Дополнительные сведения см. в разделе [Существуют ли ограничения на использование IP-адресов в пределах этих подсетей?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)


### <a name="stop-the-azure-ssis-ir"></a>Остановка среды выполнения интеграции Azure SSIS
Остановите среду выполнения интеграции Azure SSIS перед присоединением к виртуальной сети. Следующая команда освобождает все узлы и прекращает выставление счетов.

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force 
```
### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Настройка параметров виртуальной сети для присоединения средой выполнения интеграции Azure SSIS
Зарегистрируйте поставщик ресурсов пакетной службы Azure:

```powershell
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Настройка среды выполнения интеграции Azure SSIS
Выполните команду `Set-AzureRmDataFactoryV2IntegrationRuntime`, чтобы настроить среду выполнения интеграции Azure SSIS для присоединения виртуальной сети. 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Запуск среды выполнения интеграции Azure SSIS
Чтобы запустить среду выполнения интеграции Azure SSIS, выполните следующую команду. 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```
Выполнение этой команды занимает от 20 до 30 минут.

## <a name="use-azure-expressroute-with-the-azure-ssis-ir"></a>Использование Azure ExpressRoute со средой выполнения интеграции SSIS и Azure

Можно подключить канал [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) к своей инфраструктуре виртуальной сети, расширяя таким образом локальную сеть в Azure. 

Как правило, для этого используется принудительное туннелирование (объявление маршрута BGP (0.0.0.0/0) к виртуальной сети), из-за чего исходящий трафик Интернета из виртуальной сети перенаправляется на устройство локальной сети для проверки и ведения журнала. Такой поток трафика нарушает связь между средой выполнения интеграции SSIS и Azure в виртуальной сети и зависимыми службами фабрики данных Azure. Чтобы устранить эту проблему, следует указать один (или несколько) [определяемый пользователем маршрут в подсети](../virtual-network/virtual-networks-udr-overview.md), которая содержит среду выполнения интеграции SSIS и Azure. Определяемый пользователем маршрут задает маршруты для подсети, которые используются вместо маршрута BGP.

По возможности используйте следующую конфигурацию.
-   Конфигурация ExpressRoute объявляет маршрут 0.0.0.0/0 и по умолчанию организует принудительное туннелирование всех исходящих подключений в локальную среду.
-   Определяемый пользователем маршрут, применяемый к подсети, которая содержит среду выполнения интеграции SSIS и Azure, определяет маршрут 0.0.0.0/0 с типом следующего прыжка "Интернет".
- 
В результате этих действий определяемый пользователем маршрут уровня подсети получает приоритет над принудительным туннелированием ExpressRoute, обеспечивая передачу исходящего трафика Интернета из среды выполнения интеграции SSIS и Azure.

Если вас беспокоит потеря возможности проверять исходящий трафик Интернета из этой подсети, можно добавить правило NSG для подсети, ограничивающее назначения исходящего трафика [IP-адресами центров обработки данных Azure](https://www.microsoft.com/download/details.aspx?id=41653).

Ознакомьтесь с [этим сценарием PowerShell](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c) в качестве примера. Это сценарий необходимо запускать еженедельно, чтобы список IP-адресов центров обработки данных Azure оставался актуальным.

## <a name="next-steps"></a>Дополнительная информация
Дополнительные сведения о среде выполнения Azure SSIS см. в следующих разделах: 

- [Среда выполнения интеграции Azure SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). В этой статье содержатся общие сведения о средах выполнения интеграции в целом, включая IR Azure SSIS. 
- [Развертывание пакетов служб интеграции SQL Server (SSIS) в Azure](tutorial-create-azure-ssis-runtime-portal.md). Эта статья содержит пошаговые инструкции для создания IR Azure SSIS. Для размещения каталога SSIS в ней используется база данных SQL Azure. 
- [Создание среды выполнения интеграции Azure SSIS в фабрике данных Azure](create-azure-ssis-integration-runtime.md). Эта статья дополняет соответствующее руководство, а также предоставляет инструкции по использованию управляемого экземпляра Базы данных SQL Azure (закрытая предварительная версия) и присоединению среды выполнения интеграции к виртуальной сети. 
- [Мониторинг среды выполнения интеграции в фабрике данных Azure](monitor-integration-runtime.md#azure-ssis-integration-runtime). В этом статье показано, как извлечь сведения о среде выполнения интеграции Azure SSIS и описания состояний из возвращаемых данных. 
- [Управление средой выполнения интеграции Azure SSIS](manage-azure-ssis-integration-runtime.md). В этой статье показано, как остановить, запустить или удалить Azure SSIS IR. Кроме того, здесь показано, как развернуть IR Azure SSIS путем добавления узлов. 
