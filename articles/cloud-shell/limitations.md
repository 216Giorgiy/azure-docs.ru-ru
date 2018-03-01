---
title: "Ограничения Azure Cloud Shell | Документация Майкрософт"
description: "Обзор ограничений Azure Cloud Shell."
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: juluk
ms.openlocfilehash: 21a050373a9901220e106534464a8c13565bb3d8
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2018
---
# <a name="limitations-of-azure-cloud-shell"></a>Ограничения Azure Cloud Shell

Azure Cloud Shell имеет следующие известные ограничения:

## <a name="general-limitations"></a>Общие ограничения

### <a name="system-state-and-persistence"></a>Состояние системы и сохраняемость

Компьютер, предоставляющий сеанс Cloud Shell, является временным и перезапускается, если сеанс не используется в течение 20 минут. Для Cloud Shell требуется подключение файлового ресурса Azure. Поэтому ваша подписка должна иметь возможность настраивать ресурсы хранилища для доступа к Cloud Shell. Дополнительные рекомендации:

* Если подключено хранилище, то сохраняются только изменения в каталоге `clouddrive`. Кроме того, в Bash сохраняется ваш каталог `$Home`.
* Файловые ресурсы Azure можно подключить только в пределах [назначенного региона](persisting-shell-storage.md#mount-a-new-clouddrive).
  * В Bash выполните команду `env`, чтобы найти регион, заданный для `ACC_LOCATION`.

### <a name="browser-support"></a>Поддержка браузеров

Cloud Shell поддерживает последние версии Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox и Apple Safari. Использование Safari в режиме защищенного просмотра не поддерживается.

### <a name="copy-and-paste"></a>Копирование и вставка

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Для заданного пользователя может быть активна только одна оболочка.

Пользователи могут запускать только один тип оболочки одновременно, **Bash** или **PowerShell**. Однако можно запустить несколько экземпляров Bash или PowerShell одновременно. Переключение между Bash и PowerShell приводит к перезапуску Cloud Shell и завершение существующих сеансов.

### <a name="usage-limits"></a>Ограничения использования

Cloud Shell предназначен для интерактивного использования. В результате любые длительные неинтерактивные сеансы завершаются без предупреждения.

## <a name="bash-limitations"></a>Ограничения Bash

### <a name="user-permissions"></a>Разрешения пользователя

Разрешения задаются как для обычных пользователей без доступа к sudo. Все, что устанавливается за пределами каталога `$Home` или `clouddrive`, не сохраняется.

### <a name="editing-bashrc"></a>Изменение файла .bashrc

Будьте внимательны при изменении файла .bashrc, так как некоторые изменения могут привести к непредвиденным ошибкам Bash в Cloud Shell.

## <a name="powershell-limitations"></a>Ограничения PowerShell

### <a name="slow-startup-time"></a>Медленный запуск

Инициализация PowerShell в Azure Cloud Shell (предварительная версия) может занимать до 60 секунд на этапе предварительной версии.

### <a name="no-home-directory-persistence"></a>Каталог $Home не сохраняется

Данные, записанные в каталог `$Home` любым приложением (например, git, vim и пр.), не сохраняются между сеансами PowerShell. Решение этой проблемы приведено [здесь](troubleshooting.md#powershell-resolutions).

### <a name="default-file-location-when-created-from-azure-drive"></a>Расположение файла по умолчанию при создании с помощью диска Azure:

С помощью командлетов PowerShell пользователи не могут создавать файлы на диске Azure. Когда пользователи создают новые файлы с помощью других инструментов, таких как vim или nano, по умолчанию файлы сохраняются в папке C:\Users. 

### <a name="gui-applications-are-not-supported"></a>Приложения с графическим пользовательским интерфейсом не поддерживаются

Если пользователь выполняет команду для создания диалогового окна Windows, например `Connect-AzureAD` или `Login-AzureRMAccount`, отобразится следующее сообщение об ошибке: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

## <a name="next-steps"></a>Дополнительная информация

[Устранение неполадок Cloud Shell](troubleshooting.md) <br>
[Краткое руководство по Bash](quickstart.md) <br>
[Краткое руководство по PowerShell](quickstart-powershell.md)
