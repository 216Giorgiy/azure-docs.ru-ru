---
title: Руководство. Настройка ролей администратора и пользователя для служб Azure Analysis Services | Документация Майкрософт
description: Узнайте, как настроить роли в Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: tutorial
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: owend
ms.openlocfilehash: 4c1a3f52c37dcaad4bc2f84d6d2fa04b61376cf1
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188782"
---
# <a name="tutorial-configure-server-administrator-and-user-roles"></a>Руководство. Настройка ролей администратора и пользователя сервера

 При работе с этим руководством вы с помощью SQL Server Management Studio (SSMS) подключитесь к серверу в Azure и настроите роли администратора сервера и шаблона базы данных. Также мы ознакомитесь с [языком TMSL](https://docs.microsoft.com/sql/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200). Язык написания скриптов TMSL основывается на формате JSON и предназначен для табличных моделей на уровнях совместимости 1200 и выше. Он позволяет автоматизировать многие задачи табличного моделирования. TMSL часто используется с PowerShell, но в этом руководстве мы ограничимся редактором запросов XMLA в среде SSMS. При работе с этим руководством вы выполните следующие задачи: 
  
> [!div class="checklist"]
> * получение имени сервера с помощью портала;
> * подключение к серверу с помощью среды SSMS;
> * добавление пользователя или группы к роли администратора сервера; 
> * добавление пользователя или группы к роли администратора шаблона базы данных;
> * добавление новой роли шаблона базы данных и добавление пользователя или группы.

Дополнительные сведения о защите пользователей в службах Azure Analysis Services см. в статье [Аутентификация и разрешения пользователей](../analysis-services-manage-users.md). 

## <a name="prerequisites"></a>Предварительные требования

- Подписка с поддержкой Azure Active Directory.
- Созданный в подписке [сервер Azure Analysis Services](../analysis-services-create-server.md).
- Разрешения [администратора сервера](../analysis-services-server-admins.md).
- [Модель из примера adventureworks](../analysis-services-create-sample-model.md), размещенная на сервере.
- Установленная последняя версия [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал](https://portal.azure.com/).

## <a name="get-server-name"></a>Получение имени сервера
Чтобы подключиться к серверу в среде SSMS, вам нужно знать имя этого сервера. Имя сервера можно получить на портале.

На **портале Azure** выберите сервер, а затем щелкните **Обзор** > **Имя сервера** и скопируйте имя сервера.
   
   ![Получение имени сервера в Azure](./media/analysis-services-tutorial-roles/aas-copy-server-name.png)

## <a name="connect-in-ssms"></a>Подключение в среде SSMS

Чтобы выполнить остальные задачи, вы подключитесь к серверу через SSMS для управления им.

1. В среде SSMS выберите **Обозреватель объектов** и щелкните **Подключиться** > **Analysis Services**.

    ![Подключение](./media/analysis-services-tutorial-roles/aas-ssms-connect.png)

2. В диалоговом окне **Подключение к серверу** укажите в соответствующем поле **имя сервера**, которое вы скопировали на портале. В разделе **Проверка подлинности** выберите **Active Directory — универсальная с поддержкой MFA**, затем укажите учетную запись пользователя и щелкните **Подключиться**.
   
    ![Подключение в среде SSMS](./media/analysis-services-tutorial-roles/aas-connect-ssms-auth.png)

    > [!TIP]
    > Мы рекомендуем выбрать универсальную аутентификацию Active Directory с поддержкой MFA. Такой тип позволяет применять [неинтерактивную и многофакторную проверку подлинности](../../sql-database/sql-database-ssms-mfa-authentication.md). 

3. В **обозревателе объектов** разверните узел объектов сервера. Щелкните имя сервера правой кнопкой мыши, чтобы открыть его свойства.
   
    ![Подключение в среде SSMS](./media/analysis-services-tutorial-roles/aas-connect-ssms-objexp.png)

## <a name="add-a-user-account-to-the-server-administrator-role"></a>Добавление учетной записи пользователя к роли администратора сервера

На этом этапе вы добавите учетную запись пользователя или группы из Azure AD в роль администратора сервера. Если вы добавляете группу безопасности, для свойства `MailEnabled` необходимо задать значение `True`.

1. В **обозревателе объектов** щелкните правой кнопкой мыши имя сервера и выберите **Свойства**. 
2. В окне **Свойства сервера анализа данных** выберите **Безопасность** > **Добавить**.
3. В окне **Выберите пользователя или группу** введите имя учетной записи пользователя или группы в Azure AD, затем щелкните **Добавить**. 
   
     ![Добавление администратора сервера](./media/analysis-services-tutorial-roles/aas-add-server-admin.png)

4. Нажмите кнопку **ОК**, чтобы закрыть окно **Свойства сервера анализа данных**.

    > [!TIP]
    > Также администраторов сервера можно добавить, используя раздел **Администраторы служб Analysis Services** на портале. 

## <a name="add-a-user-to-the-model-database-administrator-role"></a>Добавление пользователя к роли администратора шаблона базы данных

На этом этапе вы добавите учетную запись пользователя или группы к роли Internet Sales Administrator, которая уже существует в модели. Эта роль предоставляет разрешения на полный доступ (права администратора) для шаблона базы данных из примера adventureworks. В этой задаче выполняется команда TMSL [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) в специально подготовленном скрипте.

1. В **обозревателе объектов** разверните узлы **Базы данных** > **adventureworks** > **Роли**. 
2. Щелкните правой кнопкой мыши элемент **Internet Sales Administrator**, выберите **Script Role as** (Выбрать роль скрипта) > **CREATE OR REPLACE To** (Создание или замена для...) > **Новое окно редактора запросов**.

    ![Новое окно редактора запросов](./media/analysis-services-tutorial-roles/aas-add-db-admin.png)

3. В разделе **XMLAQuery** измените значение параметра **"memberName":** на имя учетной записи пользователя или группы в Azure AD. По умолчанию включена учетная запись, с которой вы вошли в систему, но вам нет смысла указывать ее, так как вы уже являетесь администратором сервера.

    ![Скрипт TMSL в запросе XMLA](./media/analysis-services-tutorial-roles/aas-add-db-admin-script.png)

4. Нажмите клавишу **F5**, чтобы выполнить скрипт.


## <a name="add-a-new-model-database-role-and-add-a-user-or-group"></a>Добавление новой роли шаблона базы данных и добавление пользователя или группы

На этом этапе вы примените команду [Create](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/create-command-tmsl?view=sql-analysis-services-2017) в скрипте TMSL, чтобы создать роль Internet Sales Global, установите для нее права *на чтение* и добавите учетную запись пользователя или группы из Azure AD.

1. В **обозревателе объектов** щелкните правой кнопкой мыши **adventureworks** и выберите **Новый запрос** > **XLMA**. 
2. Скопируйте приведенный ниже скрипт TMSL и вставьте его в редактор запроса:

    ```JSON
    {
    "create": {
      "parentObject": {
        "database": "adventureworks",
       },
       "role": {
         "name": "Internet Sales Global",
         "description": "All users can query model data",
         "modelPermission": "read",
         "members": [
           {
             "memberName": "globalsales@adventureworks.com",
             "identityProvider": "AzureAD"
           }
         ]
       }
      }
    }
    ```

3. Замените значение объекта `"memberName": "globalsales@adventureworks.com"` именем учетной записи пользователя или группы в Azure AD.
4. Нажмите клавишу **F5**, чтобы выполнить скрипт.

## <a name="verify-your-changes"></a>Проверка внесенных изменений

1. В **обозревателе объектов** щелкните имя сервера и выберите действие **Обновить** или нажмите клавишу **F5**.
2. Разверните узлы **Базы данных** > **adventureworks** > **Роли**. Убедитесь, что здесь отображаются учетная запись и новая роль, которые вы добавили на предыдущих шагах.   

    ![Проверка изменений в обозревателе объектов](./media/analysis-services-tutorial-roles/aas-connect-ssms-verify.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Удалите роли и учетные записи пользователей или групп, когда они станут не нужны. Чтобы удалить учетные записи пользователей, выберите **Свойства роли** > **Членства** или щелкните роль правой кнопкой мыши и выберите **Удалить**.


## <a name="next-steps"></a>Дополнительная информация
Из этого руководства вы узнали, как подключиться к серверу Azure AS и исследовать шаблон базы данных и свойства из примера adventureworks в среде SSMS. Вы также узнали, как использовать скрипты SSMS и TMSL для добавления пользователей или групп в существующие и новые роли. Теперь, когда вы настроили для сервера и шаблона базы данных нужные разрешения, вы и другие пользователи можете подключаться к ним через клиентские приложения, например Power BI. Чтобы получить дополнительные сведения, перейдите к следующему руководству. 

> [!div class="nextstepaction"]
> [Руководство. Подключение с помощью Power BI Desktop](analysis-services-tutorial-pbid.md)

