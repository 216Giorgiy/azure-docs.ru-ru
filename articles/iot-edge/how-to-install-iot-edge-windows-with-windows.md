---
title: Установка Azure IoT Edge в Windows с контейнерами Windows | Документация Майкрософт
description: Инструкции по установке Azure IoT Edge в Windows с контейнерами Windows
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: kgremban
ms.openlocfilehash: 39e0de6b378ed61ab375c6468b58c8c4a87b5fb9
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39575970"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-windows-containers"></a>Установка среды выполнения Azure IoT Edge в Windows для использования с контейнерами Windows

Среда выполнения Azure IoT Edge развертывается на всех устройствах IoT Edge. Она состоит из трех компонентов. **Управляющая программа безопасности IoT Edge** обеспечивает безопасность и соответствие стандартам безопасности на устройстве Edge. Управляющая программа запускается при каждой загрузке устройства и перезагружает устройство, запуская агент IoT Edge. **Агент IoT Edge** упрощает развертывание и мониторинг модулей на устройстве IoT Edge, в том числе центр IoT Edge. **Центр IoT Edge** управляет взаимодействием между модулями на устройстве IoT Edge, а также между устройством и Центром Интернета вещей.

В этой статье описаны этапы установки среды выполнения Azure IoT Edge на компьютере под управлением 64-разрядной ОС Windows (на базе процессора AMD или Intel). 

Сейчас поддержка Windows доступна в предварительной версии.

## <a name="supported-windows-versions"></a>Поддерживаемые версии Windows
Azure IoT Edge с контейнерами Windows можно использовать с такими версиями операционной системы:
  * "Windows 10 IoT Корпоративная" или "Windows 10 IoT Базовая" с обновлением за апрель 2018 года (сборка 17134);
  * Windows Server 1803.

## <a name="install-the-container-runtime"></a>Установка среды выполнения контейнера 

>[!NOTE]
>Чтобы установить платформу контейнеров в ОС "Windows IoT Базовая", следуйте инструкциям из [статьи о подготовке устройства под управлением ОС "Windows 10 IoT Базовая"][lnk-iot-core], а затем выполните инструкции, представленные ниже.

Служба Azure IoT Edge основана на среде выполнения контейнера, [совместимого с OCI][lnk-oci] (например, Docker). Для разработки и тестирования можно использовать [Docker для Windows][lnk-docker-for-windows]. 

Настройте клиент Docker для Windows [на использование контейнеров Windows][lnk-docker-config].

## <a name="install-the-azure-iot-edge-security-daemon"></a>Установка управляющей программы безопасности Azure IoT Edge

>[!NOTE]
>Использование программных пакетов Azure IoT Edge регулируется условиями лицензии, содержащейся в самих пакетах (в каталоге LICENSE). Прежде чем использовать пакет, ознакомьтесь с условиями лицензии. Установка и использование пакета означают, что вы принимаете эти условия. Если вы не согласны с условиями лицензии, не используйте пакет.

Одно устройство IoT Edge можно подготовить к работе вручную, используя строку подключения устройства, предоставленную Центром Интернета вещей. Вы также можете использовать службу подготовки устройств, что удобно, если нужно автоматически подготовить несколько устройств. В зависимости от выбранного способа подготовки выберите соответствующий скрипт установки. 

### <a name="install-and-manually-provision"></a>Установка и подготовка вручную

1. Выполните действия, описанные в статье [Регистрация нового устройства Azure IoT Edge на портале Azure][lnk-dcs], чтобы зарегистрировать устройство и получить для него строку подключения. 

2. На устройстве IoT Edge запустите PowerShell от имени администратора. 

3. Скачайте и установите среду выполнения IoT Edge. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Windows
   ```

4. При появлении запроса на ввод строки **DeviceConnectionString** укажите строку подключения, полученную из Центра Интернета вещей. Не заключайте строку подключения в кавычки. 

### <a name="install-and-automatically-provision"></a>Установка и автоматическая подготовка

1. Выполните действия, описанные в статье [Создание и подготовка имитированного устройства IoT Edge TPM в Windows][lnk-dps], чтобы установить службу подготовки устройств и получить для нее **идентификатор области**. После этого сымитируйте устройство доверенного платформенного модуля (TPM) и получите для него **идентификатор регистрации**, а затем создайте отдельную регистрацию. Зарегистрировав устройство в Центре Интернета вещей, продолжите установку.  

   >[!TIP]
   >Не закрывайте окно с запущенным симулятором TPM во время установки и тестирования. 

2. На устройстве IoT Edge запустите PowerShell от имени администратора. 

3. Скачайте и установите среду выполнения IoT Edge. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Dps -ContainerOs Windows
   ```

4. При появлении запроса укажите **ScopeID** для службы подготовки и **RegistrationID** для устройства. 

## <a name="verify-successful-installation"></a>Проверка установки

Чтобы проверить состояние службы IoT Edge, используйте следующий код: 

```powershell
Get-Service iotedge
```

Чтобы просмотреть журналы службы за последние пять минут, используйте следующий код:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Чтобы получить список запущенных модулей, используйте следующий код.

```powershell
iotedge list
```

## <a name="next-steps"></a>Дополнительная информация

Теперь, когда подготовлено устройство IoT Edge и установлена среда выполнения, вы можете [развернуть модули IoT Edge][lnk-modules].

Если вам не удается установить среду выполнения Edge, см. страницу со сведениями об [устранении неполадок][lnk-trouble].


<!-- Images -->
[img-nat]: ./media/how-to-install-iot-edge-windows-with-windows/nat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-iot-core]: how-to-install-iot-core.md
[lnk-modules]: how-to-deploy-modules-portal.md
