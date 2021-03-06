---
title: Развертывание Двойника OPC зависимостей cloud в Azure | Документация Майкрософт
description: Как развернуть Azure Двойника OPC зависимости.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ae9f2b05bfc6ea6315022d04c8d267d916cf282e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59491992"
---
# <a name="deploying-dependencies-for-local-development"></a>Развертывание зависимостей для локальной разработки

В этой статье описывается развертывание только служб на платформе Azure необходимости выполнить локальную разработку и отладку.   В конце необходимо развернуть группу ресурсов, содержащий все необходимое для локальной разработки и отладки.

## <a name="deploy-azure-platform-services"></a>Развертывание служб платформы Azure

1. Убедитесь, что у вас есть PowerShell и [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-1.1.0) установлены расширения.  Откройте командную строку или терминала и выполните:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Следуйте указаниям, чтобы присвоить имя группы ресурсов для развертывания.  Скрипт подготовит только зависимости в эту группу ресурсов в подписке Azure, но не микрослужб.  Скрипт также регистрирует приложение в Azure Active Directory.  Это необходимо для поддержки проверки подлинности на основе OAUTH.  Развертывание может занять несколько минут.

3. После завершения сценария можно выбрать для сохранения env-файле.  Env-файле среды является файл конфигурации всех служб и средств, которые вы хотите запустить на компьютере разработки.  

## <a name="troubleshooting-deployment-failures"></a>Устранение неполадок развертывания

### <a name="resource-group-name"></a>Имя группы ресурсов

Убедитесь, что использовать короткие и простые имя группы ресурсов.  Имя также используется для имени ресурсы таким образом оно должно подчиняться с требованиями к именованию ресурсов.  

### <a name="azure-active-directory-aad-registration"></a>Регистрация в Azure Active Directory (AAD)

Скрипт развертывания выполняет попытку регистрации AAD приложения в Azure Active Directory.  В зависимости от того, ваши права для выбранного клиента AAD это может оказаться невозможным.   Существует три варианта:

1. Если вы выбрали с клиентом AAD из списка клиентов, перезапустите сценарий и выберите его из списка.
2. Кроме того развертывание частного клиента AAD, перезапустите сценарий и выберите, чтобы использовать его.
3. Продолжить без проверки подлинности.  Поскольку micro служб выполняются локально, это приемлемо, но не повторяет рабочих средах.  

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда служб Двойника OPC успешно развернуто в существующий проект, вот мы предлагаем:

> [!div class="nextstepaction"]
> [Дополнительные сведения о развертывании модулей OPC Двойника](howto-opc-twin-deploy-modules.md)
