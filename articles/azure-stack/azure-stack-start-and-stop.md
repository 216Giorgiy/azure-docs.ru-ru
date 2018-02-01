---
title: "Запуск и остановка Azure Stack | Документация Майкрософт"
description: "Сведения о том, как запустить и завершить работу Azure Stack."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: mabrigg
ms.openlocfilehash: 98bf75f5883b734c785ed1a3ed924afca1737c56
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2018
---
# <a name="start-and-stop-azure-stack"></a>Запуск и остановка Azure Stack

*Область применения: интегрированные системы Azure Stack (1712 и более поздних версий).*

## <a name="stop-azure-stack"></a>Остановка Azure Stack 

Чтобы завершить работу Azure Stack, сделайте следующее:

1. Откройте сеанс привилегированной конечной точки (PEP) на компьютере с сетевым доступом к виртуальным машинам ERCS Azure Stack. См. инструкции по [использованию привилегированной конечной точки в Azure Stack](azure-stack-privileged-endpoint.md).

2. Из сеанса PEP выполните следующую команду:

    ```powershell
      Stop-AzureStack
    ```

3. Дождитесь отключения питания на всех физических узлах Azure Stack.

> [!Note]  
> Чтобы проверить состояние питания физического узла, следуйте инструкциям от изготовителя оборудования (OEM), поставившего оборудование Azure Stack. 

## <a name="start-azure-stack"></a>Запуск Azure Stack 

Чтобы запустить Azure Stack, сделайте следующее. Выполните следующие действия, независимо от того, как остановлена работа Azure Stack.

1. Включите питание на всех физических узлах в вашей среде Azure Stack. Убедитесь, что питание для физических узлов включено по инструкциям изготовителя оборудования, поставившего оборудование для Azure Stack.

2. Дождитесь запуска служб инфраструктуры Azure Stack. Для завершения запуска служб инфраструктуры Azure Stack может потребоваться два часа. Вы можете проверить состояние запуска Azure Stack с помощью командлета [**Get-ActionStatus**](#get-the-startup-status-for-azure-stack).


## <a name="get-the-startup-status-for-azure-stack"></a>Получение сведений о состоянии запуска для Azure Stack

Чтобы получить сведения о запуске для подпрограммы запуска Azure Stack, сделайте следующее:

1. Откройте сеанс PEP на компьютере с сетевым доступом к виртуальным машинам ERCS Azure Stack.

2. Из сеанса PEP выполните следующую команду:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Устранение неполадок при запуске и завершении работы Azure Stack

Если через два часа после включения питания в среде Azure Stack не удалось запустить службы инфраструктуры и клиентские службы, выполните следующие действия: 

1. Откройте сеанс PEP на компьютере с сетевым доступом к виртуальным машинам ERCS Azure Stack.

2. Выполните команду: 

    ```powershell
      Test-AzureStack
      ```

3. Просмотрите выходные данные и устраните ошибки работоспособности. Дополнительные сведения см. в статье [Запуск проверочного теста в Azure Stack](azure-stack-diagnostic-test.md).

4. Выполните команду:

    ```powershell
      Start-AzureStack
    ```

5. Если команда **Start-AzureStack** завершится ошибкой, обратитесь в службу поддержки пользователей Майкрософт. 

## <a name="next-steps"></a>Дополнительная информация 

Дополнительные сведения о средстве диагностики Azure Stack и регистрации проблем см. в статье "Средства диагностики Azure Stack". Средства диагностики Azure Stack. Средства диагностики Azure Stack. Средства диагностики Azure Stack.
