---
title: Установка Azure IoT Edge в Linux | Документация Майкрософт
description: Инструкции по установке Azure IoT Edge на устройствах AMD64 Linux под управлением Ubuntu
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 9d5d6fc436c48bd36e2da64d2b4c7c811e3ddc4f
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993356"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Установка среды выполнения Azure IoT Edge в Linux (x64)

Среда выполнения Azure IoT Edge превращает устройство в устройство IoT Edge. Среду выполнения можно развернуть на всех устройствах — от небольшого Raspberry Pi до технического сервера. Как только на устройстве будет настроена среда выполнения IoT Edge, вы можете начать развертывать в нее бизнес-логику из облака.

Дополнительные сведения о работе среды выполнения IoT Edge и ее компонентах см. в статье [Общие сведения о среде выполнения Azure IoT Edge и ее архитектуре](iot-edge-runtime.md).

В этой статье описаны этапы установки среды выполнения Azure IoT Edge на устройстве IoT Edge под управлением 64-разрядной ОС Linux (на базе процессора AMD или Intel). Ознакомьтесь с разделом [поддержки Azure IoT Edge](support.md#operating-systems), чтобы получить список поддерживаемых в настоящее время операционных систем AMD64.

> [!NOTE]
> К пакетам в репозиториях программного обеспечения Linux применяются условия лицензии, которые можно найти в каждом пакете (/usr/share/doc/*имя-пакета*). Прежде чем использовать пакет, ознакомьтесь с условиями лицензии. Установка и использование пакета свидетельствуют о принятии этих условий. Если вы не согласны с условиями лицензии, не используйте пакет.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Регистрация канала репозитория программного обеспечения и ключей (Майкрософт)

Подготовьте устройство для установки среды выполнения IoT Edge, заменив ```<release>``` на **16.04** или **18.04** в соответствии с вашим выпуском Ubuntu.

```bash
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/<release>/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/

# Perform apt update
sudo apt-get update
```

## <a name="install-the-container-runtime"></a>Установка среды выполнения контейнера

Служба Azure IoT Edge использует среду выполнения контейнера, [совместимую с OCI](https://www.opencontainers.org/). Для разработки в рабочей среде мы настоятельно рекомендуем использовать платформу [на основе Moby](https://mobyproject.org/), описанную ниже. Это единственная платформа контейнеров, которая официально поддерживается с Azure IoT Edge. Образы контейнеров Docker (Community Edition или Enterprise Edition) совместимы со средой выполнения Moby.

Обновите apt-get.

```bash
sudo apt-get update
```

Установите модуль Moby.

```bash
sudo apt-get install moby-engine
```

Установите интерфейс командной строки (CLI) Moby. CLI применяется на стадии разработки, но при развертывании в рабочей среде он может не понадобится.

```bash
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>Установка управляющей программы безопасности Azure IoT Edge

**Управляющая программа безопасности IoT Edge** обеспечивает безопасность и соответствие стандартам безопасности на устройстве Edge. Управляющая программа запускается при каждой загрузке устройства и перезагружает устройство, запуская остальные компоненты среды выполнения IoT Edge.

Приведенные ниже команды также позволяют установить стандартную версию **iothsmlib**, если она отсутствует.

Обновите apt-get.

```bash
sudo apt-get update
```

Установите управляющую программу безопасности. Пакет устанавливается в `/etc/iotedge/`.

```bash
sudo apt-get install iotedge
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Настройка управляющей программы безопасности Azure IoT Edge

Настройте среду выполнения IoT Edge, чтобы связать свое физическое устройство с идентификатором, который существует в Центре Интернета вещей Azure.

Управляющую программу можно настроить с помощью файла конфигурации `/etc/iotedge/config.yaml`. По умолчанию этот файл защищен от записи, и вам потребуется использовать повышенные разрешения для его изменения.

Одно устройство IoT Edge можно подготовить к работе вручную, используя строку подключения устройства, предоставленную Центром Интернета вещей. Вы также можете использовать службу подготовки устройств, что удобно, если нужно автоматически подготовить несколько устройств. В зависимости от выбранного способа подготовки выберите соответствующий скрипт установки.

### <a name="option-1-manual-provisioning"></a>Вариант 1. Подготовка вручную

Чтобы вручную подготовить устройство, вам необходимо предоставить ему [​​строку подключения устройства](how-to-register-device-portal.md), которую вы можете создать, зарегистрировав новое устройство в Центре Интернета вещей.

Откройте файл конфигурации.

```bash
sudo nano /etc/iotedge/config.yaml
```

Найдите раздел подготовки файла и раскомментируйте режим подготовки **вручную**. Замените значение **device_connection_string** строкой подключения для устройства IoT Edge.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning:
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

Сохраните и закройте файл.

   `CTRL + X`, `Y`, `Enter`

Когда введете сведения о подготовке в файл конфигурации, перезапустите управляющую программу:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Вариант 2. Автоматическая подготовка

Для автоматической подготовки устройства [настройте службу подготовки устройств и получите идентификатор регистрации устройства](how-to-auto-provision-simulated-device-linux.md). Автоматическая подготовка работает только с устройствами, которые содержат микросхему доверенного платформенного модуля (TPM). Например, устройства Raspberry Pi не поставляются с TPM по умолчанию.

Откройте файл конфигурации.

```bash
sudo nano /etc/iotedge/config.yaml
```

Найдите раздел подготовки файла и раскомментируйте режим подготовки **dps**. Замените значения **scope_id** и **registration_id** значениями для вашего экземпляра Службы подготовки устройств к добавлению в Центр Интернета вещей и устройства IoT Edge с доверенным платформенным модулем.

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Сохраните и закройте файл.

   `CTRL + X`, `Y`, `Enter`

Когда введете сведения о подготовке в файл конфигурации, перезапустите управляющую программу:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Проверка установки

Если вы выполняли **настройку вручную**, как описано в предыдущем разделе, подготовленная среда выполнения IoT Edge должна работать на вашем устройстве. Если вы выполняли **автоматическую настройку**, вам нужно сделать еще кое-что, чтобы среда выполнения могла зарегистрировать ваше устройство в центре Интернета вещей от вашего имени. Инструкции см. в руководстве по [созданию и подготовке имитированного устройства IoT Edge TPM в Linux](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Вы можете проверить состояние управляющей программы IoT Edge, используя следующий код:

```bash
systemctl status iotedge
```

Чтобы просмотреть журналы управляющей программы, используйте следующий код:

```bash
journalctl -u iotedge --no-pager --no-full
```

Чтобы получить список запущенных модулей, используйте следующий код.

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>Советы и рекомендации

Для запуска команд `iotedge` требуется более высокий уровень привилегий. После установки среды выполнения выйдите из системы компьютера, а затем снова войдите для автоматического обновления разрешений. До тех пор используйте перед любой командой `iotedge` префикс **sudo**.

В устройствах с ограниченными ресурсами настоятельно рекомендуется присвоить переменной среды *OptimizeForPerformance* значение *false* согласно инструкциям в [руководстве по устранению неполадок](troubleshoot.md).

Если в вашей сети используется прокси-сервер, выполните действия, описанные в статье [Настройка устройства IoT Edge для обмена данными через прокси-сервер](how-to-configure-proxy-support.md).

## <a name="uninstall-iot-edge"></a>Удаление IoT Edge

Если вы хотите удалить установку IoT Edge с устройства Linux, используйте следующие команды из командной строки.

Удалите среду выполнения IoT Edge.

```bash
sudo apt-get remove --purge iotedge
```

При удалении среды выполнения IoT Edge созданные с ее помощью контейнеры остановятся, но будут по-прежнему храниться на устройстве. Просмотрите все контейнеры, чтобы увидеть, какие из них остаются.

```bash
sudo docker ps -a
```

Удалите контейнеры с устройства, включая два контейнера в среде выполнения.

```bash
sudo docker rm -f <container name>
```

Наконец, удалите среду выполнения контейнера с устройства.

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Дополнительная информация

Теперь, когда подготовлено устройство IoT Edge и установлена среда выполнения, вы можете [развернуть модули IoT Edge](how-to-deploy-modules-portal.md).

Если вам не удается установить среду выполнения Edge, перейдите на страницу со сведениями об [устранении неполадок](troubleshoot.md).

Чтобы обновить существующую установку до последней версии IoT Edge, см. раздел [Обновление управляющей программы безопасности и среды выполнения IoT Edge](how-to-update-iot-edge.md).
