---
title: Руководство по установке и развертывании C ОС Linux агент центра безопасности Azure для Интернета вещей предварительной версии | Документация Майкрософт
description: Сведения об установке агента IoT в центре безопасности Azure в 32-разрядных и 64-разрядной Linux.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: mlottner
ms.openlocfilehash: 0089fd1af6576f9bcdebe4b7f270a573205dea82
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59783506"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Развертывание центра безопасности для безопасности на основе Интернета вещей C агента для Linux

> [!IMPORTANT]
> Центр безопасности Azure для Интернета вещей сейчас предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

В этом руководстве объясняется, как установить и развернуть центр безопасности Azure (ASC) для агента безопасности на основе C Интернета вещей в Linux.

Из этого руководства вы узнаете, как выполнить следующие задачи: 
> [!div class="checklist"]
> * Install
> * Проверка развертывания
> * Удаление агента.
> * Устранение неполадок 

## <a name="prerequisites"></a>Технические условия

Другие платформы и версии агента, см. в разделе [выберите агент защиты](how-to-deploy-agent.md).

1. Чтобы развернуть агент безопасности, необходимые права локального администратора на компьютере, которым вы хотите установить на (sudo).

1. [Создайте модуль безопасности](quickstart-create-security-twin.md) для устройства.

## <a name="installation"></a>Установка 

Чтобы установить и развернуть агент безопасности, сделайте следующее.


1. На сайте [GitHub](https://aka.ms/iot-security-github-c) скачайте самую последнюю версию на свой компьютер.

1. Извлеките содержимое пакета и перейдите в папку _/Install_.

1. Добавьте права на выполнение к **скрипту InstallSecurityAgent**, выполнив следующую команду:
    
   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Затем выполните приведенную ниже команду: 

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```
   
   Дополнительные сведения о параметрах проверки подлинности см. в [этой статье](concept-security-agent-authentication-methods.md).

Скрипт выполняет следующее:

1. Установка необходимых компонентов.

2. Добавление пользователя службы (с отключенным интерактивным входом).

3. Установка агента в качестве **управляющей программы** (предполагается, что устройство использует **systemd** для управления службой).

4. Настройка агента с предоставленными параметрами проверки подлинности. 

Для получения дополнительной справки запустите скрипт с параметром –help: 
    
    ./InstallSecurityAgent.sh --help

### <a name="uninstall-the-agent"></a>Удаление агента

Чтобы удалить агент, запустите скрипт с-удалить параметр:

    ./InstallSecurityAgent.sh -–uninstall

## <a name="troubleshooting"></a>Устранение неполадок
Проверьте состояние развертывания, выполнив следующую команду:

    systemctl status ASCIoTAgent.service


## <a name="next-steps"></a>Дальнейшие действия
- Просмотрите [обзор](overview.md) службы ASC для Интернета вещей.
- Ознакомьтесь с дополнительными сведениями об [архитектуре](architecture.md) ASC для Интернета вещей.
- Включите [службу](quickstart-onboard-iot-hub.md).
- Просмотрите [часто задаваемые вопросы](resources-frequently-asked-questions.md).
- Ознакомьтесь со сведениями об [оповещениях безопасности](concept-security-alerts.md).