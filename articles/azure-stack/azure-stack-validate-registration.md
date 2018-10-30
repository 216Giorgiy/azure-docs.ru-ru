---
title: Проверка регистрации Azure для Azure Stack | Документация Майкрософт
description: Применение средства проверки готовности Azure Stack для проверки регистрации Azure.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/08/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 51753a5324bbbcbf4e951628a42dd3bf425354af
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957588"
---
# <a name="validate-azure-registration"></a>Проверка регистрации в Azure 
Средство проверки готовности Azure Stack (AzsReadinessChecker) позволяет убедиться, что ваша подписка Azure готова к работе с Azure Stack. Проверьте регистрацию перед тем, как развертывать Azure Stack. Средство проверки готовности выполняет следующие проверки:
- Используемая вами подписка Azure является поддерживаемой. Подписки должны быть типа "Поставщик облачных служб" или "Соглашение Enterprise". 
- Учетная запись, используемая для регистрации подписки в Azure, может войти в Azure и является владельцем подписки. 

Дополнительные сведения о регистрации Azure Stack см. в [этой статье](azure-stack-registration.md). 

## <a name="get-the-readiness-checker-tool"></a>Получение средства проверки готовности
Скачайте последнюю версию средства проверки готовности Azure Stack (AzsReadinessChecker), доступную в [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Предварительные требования
Убедитесь, что выполнены указанные ниже предварительные требования.

**На компьютере, где запускается это средство:**
 - Необходимо установить Windows 10 или Windows Server 2016 и обеспечить подключение к Интернету.
 - Необходимо установить PowerShell 5.1 или более поздней версии. Чтобы проверить используемую версию, запустите следующий командлет PowerShell и проверьте значения *Major* (основной номер версии) и *Minor* (дополнительный номер версии):  

    >`$PSVersionTable.PSVersion` 
 - Настройте [PowerShell для использования с Azure Stack](azure-stack-powershell-install.md). 
 - Загрузите последнюю версию [средства проверки готовности Microsoft Azure Stack](https://aka.ms/AzsReadinessChecker).  

**Среда Azure Active Directory:**
 - Определите имя пользователя и пароль для учетной записи, которая является владельцем подписки Azure, которую вы будете использовать с Azure Stack.  
 - Определите идентификатор подписки Azure, которая будет использоваться. 
 - Определите окружение AzureEnvironement, которое вы намерены использовать: *AzureCloud*, *AzureGermanCloud* или *AzureChinaCloud*.

## <a name="validate-azure-registration"></a>Проверка регистрации в Azure
1. На компьютере, который соответствует всем предварительным условиям, откройте командную строку PowerShell с правами администратора и выполните следующую команду, чтобы установить AzsReadinessChecker.
    > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. В командной строке PowerShell выполните следующее, чтобы задать *$registrationCredential* в качестве учетной записи, которая является владельцем подписки.   Замените *subscriptionowner@contoso.onmicrosoft.com* именами учетной записи и клиента, соответственно. 
    > `$registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"`

3. В командной строке PowerShell выполните следующее, чтобы задать *$subscriptionID* в качестве используемой подписки Azure. Замените *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* собственным идентификатором подписки.  
     > `$subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"` 

4. В командной строке PowerShell выполните следующую команду, чтобы начать проверку подписки. 
   - Для параметра AzureEnvironement укажите значение *AzureCloud*, *AzureGermanCloud* или *AzureChinaCloud*.  
   - Предоставьте имя администратора Azure Active Directory и имя клиента Azure Active Directory. 

   > `Invoke-AzsRegistrationValidation -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID`

5. Когда средство завершит работу, просмотрите выходные данные. Убедитесь, что в них указано состояние "ОК" для требований ко входу в систему и к регистрации. При успешном завершении проверки отобразится следующий результат:  
````PowerShell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: OK

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
````


## <a name="report-and-log-file"></a>Файлы отчета и журнала
При каждом запуске проверки все результаты сохраняются в файлах **AzsReadinessChecker.log** и **AzsReadinessCheckerReport.json**. Расположение этих файлов указывается в PowerShell вместе с результатами проверки. 

Эти файлы помогут передать сведения о состоянии проверки другим заинтересованным лицам перед развертыванием Azure Stack или для исследования проблем, обнаруженных при проверке. В обоих файлах сохраняются результаты каждой очередной проверки. В отчете содержатся подтверждения команды развертывания по конфигурации удостоверений. Файл журнала поможет командам развертывания или поддержки диагностировать проблемы с проверкой. 

По умолчанию оба файла сохраняются в расположении *C:\Users\<имя_пользователя>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Чтобы задать другое расположение отчетов, при запуске проверки можно указать в конце командной строки параметр **-OutputPath** ***&lt;путь&gt;***.   
 - Укажите параметр **-CleanReport** в конце команды, чтобы удалить из файла *AzsReadinessCheckerReport.json* сведения.  о предыдущих запусках средства. Дополнительные сведения об отчетах проверки Azure Stack можно найти [здесь](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Ошибки при проверке
Если проверка завершается ошибкой, сведения о сбое отображаются в окне PowerShell. Кроме того, сведения записываются в файл AzsReadinessChecker.log.

Следующие примеры дают некоторые рекомендации по распространенным ошибкам проверки.

### <a name="user-must-be-an-owner-of-the-subscription"></a>Пользователь должен быть владельцем подписки   
````PowerShell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
The user admin@contoso.onmicrosoft.com is role(s) Reader for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d. User must be an owner of the subscription to be used for registration.
Additional help URL https://aka.ms/AzsRemediateRegistration

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
````
**Причина** — текущая учетная запись не имеет прав администратора подписки Azure.   

**Разрешение**. Используйте учетную запись администратора подписки Azure, которой будет выставлен счет за использование развертывания Azure Stack.


### <a name="expired-or-temporary-password"></a>Пароль с истекшим сроком действия или временный пароль 
````PowerShell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d using account admin@contoso.onmicrosoft.com failed with AADSTS50055: Force Change P
assword.
Trace ID: 48fe06f5-a5b4-4961-ad45-a86964689900
Correlation ID: 3dd1c9b2-72fb-46a0-819d-058f7562cb1f
Timestamp: 2018-10-22 11:16:56Z: The remote server returned an error: (401) Unauthorized.

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
````
**Причина** — вход с помощью данной учетной записи невозможен, так как истек срок действия пароля или используется временный пароль.     

**Разрешение** — в PowerShell выполните инструкции на экране и следуйте им, чтобы сбросить пароль. 
  > `Login-AzureRMAccount` 

Также вы можете войти с этой учетной записью в https://portal.azure.com. В таком случае пользователь должен будет сменить пароль.


### <a name="unknown-user-type"></a>Неизвестный тип пользователя  
````PowerShell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d using account admin@contoso.onmicrosoft.com failed with unknown_user_type: Unknown Us
er Type

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
````
**Причина** — вход с учетной записью в указанную среду Azure Active Directory невозможен. В нашем примере параметр *AzureChinaCloud* имеет значение *AzureEnvironment*.  

**Разрешение** — убедитесь, что учетная запись существует в указанном окружении Azure. Выполните в PowerShell следующую команду, чтобы проверить допустимость учетной записи для конкретного окружения.     
  > `Login-AzureRmAccount -EnvironmentName AzureChinaCloud`


## <a name="next-steps"></a>Дальнейшие действия
[Проверка идентификатора Azure](azure-stack-validate-identity.md)
[Просмотр отчета о готовности](azure-stack-validation-report.md)
[Общие рекомендации по интеграции Azure Stack](azure-stack-datacenter-integration.md)

