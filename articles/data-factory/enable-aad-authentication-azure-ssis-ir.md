---
title: Включение аутентификации Azure Active Directory в среде выполнения интеграции Azure-SSIS | Документация Майкрософт
description: В этой статье описано, как настроить поддержку подключений, использующих аутентификацию Azure Active Directory, в среде выполнения интеграции Azure-SSIS.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: douglasl
ms.openlocfilehash: d2000e626166304e92556e3c965df175a27046ad
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2018
ms.locfileid: "53321073"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Включение аутентификации Azure Active Directory в среде выполнения интеграции Azure-SSIS

В этой статье показано, как создать среду выполнения интеграции с удостоверением службы "Фабрика данных Azure". Для создания среды выполнения интеграции Azure-SSIS можно использовать аутентификацию Azure Active Directory (Azure AD) с помощью управляемого удостоверения службы "Фабрика данных Azure" вместо аутентификации SQL.

Дополнительные сведения об управляемом удостоверении фабрики данных см. в статье [Удостоверение службы Фабрики данных Azure](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Сейчас, если в созданной среде выполнения интеграции Azure-SSIS используется аутентификация SQL, вы можете перенастроить ее на аутентификацию Azure AD с помощью PowerShell.

## <a name="enable-azure-ad-on-azure-sql-database"></a>Включение Azure AD в службе "База данных SQL Azure"

Служба "База данных SQL Azure" поддерживает создание базы данных с использованием пользователя Azure AD. В результате пользователя Azure AD можно назначить администратором Active Directory, а затем войти в SQL Server Management Studio (SSMS) как пользователь Azure AD. Затем вы можете создать автономного пользователя в группе Azure AD. Это позволит среде выполнения интеграции создать каталог SQL Server Integration Services (SSIS) на сервере.

### <a name="create-a-group-in-azure-ad-and-make-the-managed-identity-for-your-data-factory-a-member-of-the-group"></a>Создание группы в Azure AD и включение в нее управляемого удостоверения для фабрики данных

Можно использовать существующую группу Azure AD или создать новую с помощью Azure AD PowerShell.

1.  Установите модуль [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2).

2.  Войдите с помощью командлета  `Connect-AzureAD` и выполните следующую команду, чтобы создать группу и сохранить ее в переменной:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Ниже приведен пример выходных данных. В них содержатся подробные сведения о значении переменной.

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Добавьте в группу управляемое удостоверение фабрики данных. Чтобы получить идентификатор удостоверения службы субъекта (например, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc), выполните инструкции, приведенные в статье [Удостоверение службы фабрики данных Azure](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity). Не используйте идентификатор приложения удостоверения службы.

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Позже также можно проверить членство в группе.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Включение аутентификации Azure AD в службе "База данных SQL Azure"

Чтобы  [настроить аутентификацию Azure AD в службе "База данных SQL Azure"](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure), сделайте следующее.

1.  На портале Azure выберите  **Все службы** -> **Серверы SQL Server**  на панели навигации слева.

2.  Выберите службу "База данных SQL Azure", в которой нужно включить аутентификацию Azure AD.

3.  В разделе  **Параметры**  этой колонки выберите  **Администратор Active Directory**.

4.  На панели команд щелкните  **Задать администратора**.

5.  Выберите учетную запись пользователя AAD, которой необходимо предоставить права администратора сервера, и щелкните  **Выбрать**.

6.  На панели команд щелкните  **Сохранить**.

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Создание в базе данных автономного пользователя, представляющего группу Azure AD

На следующем шаге вам потребуется  [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)  (SSMS).

1.  Запустите среду SQL Server Management Studio.

2.  В диалоговом окне  **Подключение к серверу**  введите имя сервера SQL в поле  **Имя сервера** .

3.  В поле  **Аутентификация**  выберите  **Active Directory — универсальная с поддержкой MFA**. (Ви также можете использовать два других типа аутентификации Active Directory. Дополнительные сведения см. в статье [Настройка аутентификации Azure Active Directory и управление ею с использованием базы данных SQL, управляемого экземпляра или хранилища данных SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).)

4.  В поле  **Имя пользователя** введите имя учетной записи Azure AD, установленной в качестве администратора сервера, например testuser@xxxonline.com.

5.  Выберите  **Подключение**. Завершите процесс входа в систему.

6.  В  **обозревателе объектов** разверните папку  **Базы данных** -> "Системные базы данных".

7.  Щелкните правой кнопкой мыши базу данных **master** и выберите  **Создать запрос**.

8.  В окне запроса введите следующую строку и щелкните  **Выполнить**  на панели инструментов:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    В результате выполнения команды должен быть создан автономный пользователь для группы.

9.  Очистите окно запроса, введите следующую строку и на панели инструментов нажмите кнопку **Выполнить**:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    В результате выполнения команды автономный пользователь получает права на создание базы данных.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Включение Azure AD в службе "Управляемый экземпляр Базы данных SQL Azure"

Управляемый экземпляр Базы данных SQL Azure поддерживает создание базы данных с использованием MSI напрямую. Не нужно соединять MSI фабрики данных с группой AD или создавать пользователя, содержащегося в Управляемом экземпляре.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database-managed-instance"></a>Включение аутентификации Azure AD в Управляемом экземпляре Базы данных SQL Azure

1.   На портале Azure выберите **Все службы** -> **Серверы SQL Server** на левой панели навигации.

1.   Выберите сервер SQL Server, на котором нужно включить аутентификацию Azure AD.

1.   В разделе колонки **Параметры** выберите **Администратор Active Directory**.

1.   На панели команд щелкните **Задать администратора**.

1.   Выберите учетную запись пользователя Azure AD, которую необходимо сделать администратором сервера, а затем нажмите кнопку **Выбрать**.

1.   На панели команд нажмите кнопку **Сохранить**.

### <a name="add-data-factory-msi-as-a-user-to-the-azure-sql-database-managed-instance"></a>Добавление MSI фабрики данных как пользователя в Управляемый экземпляр Базы данных SQL Azure

1.  Запустите среду SQL Server Management Studio.

2.  Войдите с использованием учетной записи администратора SQL или Active Directory.

3.  В обозревателе объектов разверните папку "Базы данных" -> "Системные базы данных".

4.  Щелкните правой кнопкой мыши базу данных master и выберите **Создать запрос**.

5.  Вы можете выполнить указания статьи [Удостоверение службы фабрики данных Azure](data-factory-service-identity.md) и получить идентификатор приложения удостоверения службы. (Для этой цели не используйте идентификатор удостоверения службы.)

6.  В окне запроса выполните следующий сценарий, чтобы преобразовать идентификатор приложения удостоверения службы в двоичный тип данных:

    ```sql
    DECLARE @applicationId uniqueidentifier = {your service identity application id}
    select CAST(@applicationId AS varbinary)
    ```

7.  Значение можно получить в окне результатов.

8.  Очистите окно запроса и выполните следующий сценарий:

    ```sql
    CREATE LOGIN [{MSI name}] FROM EXTERNAL PROVIDER with SID ={your service identity application id in binary type}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{MSI name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{MSI name}]
    ```

9.  Команда будет успешно выполнена.

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Подготовка к работе среды выполнения интеграции Azure-SSIS на портале

При подготовке к работе среды выполнения интеграции Azure-SSIS на портале Azure на странице **Параметры SQL** установите флажок "Use AAD authentication with the managed identity for your ADF" (Использовать аутентификацию AAD с управляемым удостоверением Фабрики данных Azure). (На приведенном ниже снимке экрана показаны параметры среды выполнения интеграции со службой "База данных SQL Azure". Если в этой среде используется управляемый экземпляр, свойство Catalog Database Service Tier (Уровень службы базы данных каталога. Все остальные параметры такие же.)

Дополнительные сведения о создании среды выполнения интеграции Azure-SSIS в службе "Фабрика данных Azure" см. в [этой статье](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Параметры среды выполнения интеграции Azure-SSIS](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Подготовка к работе среды выполнения интеграции Azure-SSIS с помощью PowerShell

Чтобы подготовить к работе среду выполнения интеграции Azure-SSIS с помощью PowerShell, сделайте следующее:

1.  Установите модуль [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) .

2.  В скрипте не устанавливайте *CatalogAdminCredential* . Например: 

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
