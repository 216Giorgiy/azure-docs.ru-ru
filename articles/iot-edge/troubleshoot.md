---
title: "Устранение неполадок в Azure IoT Edge | Документация Майкрософт"
description: "Решения распространенных проблем и получение навыков по устранению неполадок для Azure IoT Edge"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 7b9f9f8295aac0920ae4726289c535aae12c4482
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2018
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Распространенные проблемы и их решения для Azure IoT Edge

Если при запуске Azure IoT Edge в вашей среде возникают проблемы, используйте эту статью как руководство для устранения неполадок. 

## <a name="standard-diagnostic-steps"></a>Стандартные действия диагностики 

При возникновении проблемы необходимо узнать о состоянии устройства IoT Edge. Для этого можно просмотреть журналы контейнера и сообщения, переданные на устройство и из него. Для сбора сведений используйте команды и инструменты, приведенные в этом разделе. 

* Просмотрите журналы контейнеров Docker, чтобы обнаружить проблемы. Начните с развернутых контейнеров, а затем просмотрите контейнеры, которые составляют среду выполнения IoT Edge: агент и центр Edge. В журналах агента Edge обычно содержатся сведения о жизненном цикле каждого контейнера, а в журналах центра Edge — об обмене сообщениями и маршрутизации. 

   ```cmd
   docker logs <container name>
   ```

* Просмотрите сообщения, отправляемые через центр Edge, и соберите данные аналитики касательно обновлений свойств устройства из подробных журналов контейнеров в среде выполнения. Если вы изучаете статьи с краткими руководствами, можете добавить параметр --auto-cert-gen-force-no-passwords.

   ```cmd
   iotedgectl setup --connection-string "{device connection string}" --runtime-log-level debug
   ```

* Если возникают проблемы с подключением, проверьте переменные среды устройства Edge, например строку подключения устройства:

   ```cmd
   docker exec edgeAgent printenv
   ```

Можно также проверить сообщения, отправленные между Центром Интернета вещей и устройствами IoT Edge. Просмотрите эти сообщения с помощью расширения [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) для Visual Studio Code. Дополнительные сведения см. в записи блога об [удобном средстве при разработке с помощью Центра Интернета вещей Azure](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

После изучения журналов и сообщений можно попробовать перезапустить среду выполнения Azure IoT Edge:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>Прекращение работы агента Edge в течение минуты

После запуска агент Edge работает в течение минуты и останавливается. В журналах указывается, что агент Edge пытается установить подключение к Центру Интернета вещей через расширенный протокол управления очередью сообщений (AMQP), а затем по истечении приблизительно 30 секунд пытается выполнить подключение с помощью того же расширенного протокола AMQP через WebSocket. Когда это не удается выполнить, агент Edge завершает свою работу. 

Примеры журналов агента Edge:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Первопричина
Сетевая конфигурация сети узла препятствует получению доступа к сети агентом Edge. Сначала агент пытается подключиться через AMQP (порт 5671). Если это не удается, он пытается подключиться через WebSocket (порт 443).

Среда выполнения IoT Edge настраивает сеть для всех модулей, с которыми нужно взаимодействовать. В Linux этой сетью является мостовая сеть. В Windows используется NAT. Эта проблема чаще всего встречается на устройствах с Windows, использующих контейнеры Windows с сетью NAT. 

### <a name="resolution"></a>Способы устранения:
Должен быть маршрут к Интернету для IP-адресов, назначенных этой мостовой сети или сети NAT. Иногда конфигурация VPN на узле переопределяет сеть IoT Edge. 

## <a name="edge-hub-fails-to-start"></a>Сбой запуска Центра Edge

Когда не удается запустить Центр Edge, в журналах отображается следующее сообщение: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Первопричина
На сервере связанный порт 443 используется для другого процесса. Центр Edge сопоставляет порты 5671 и 443 для использования в шлюзах в различных условиях. Если с этим портом связан другой процесс, такое сопоставление портов завершается сбоем. 

### <a name="resolution"></a>Способы устранения:
Найдите и остановите процесс, который использует порт 443. Таким процессом обычно является веб-сервер.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Агент Edge не может получить доступ к образу модуля (403)
Запуск контейнера завершается сбоем, и в журналах агента Edge отображается ошибка 403. 

### <a name="root-cause"></a>Первопричина
У агента Edge нет разрешений на доступ к образу модуля. 

### <a name="resolution"></a>Способы устранения:
Попробуйте снова выполнить команду `iotedgectl login`.

## <a name="iotedgectl-cant-find-docker"></a>iotedgectl не удается найти Docker
iotedgectl не удается выполнить команду установки или запуска. При этом в журналы записывается следующее сообщение:
```output
File "/usr/local/lib/python2.7/dist-packages/edgectl/host/dockerclient.py", line 98, in get_os_type
  info = self._client.info()
File "/usr/local/lib/python2.7/dist-packages/docker/client.py", line 174, in info
  return self.api.info(*args, **kwargs)
File "/usr/local/lib/python2.7/dist-packages/docker/api/daemon.py", line 88, in info
  return self._result(self._get(self._url("/info")), True)
```

### <a name="root-cause"></a>Первопричина
iotedgectl не удается найти среду Docker, которая является необходимым компонентом.

### <a name="resolution"></a>Способы устранения:
Установите среду Docker, убедитесь, что она работает, и повторите попытку.

## <a name="next-steps"></a>Дополнительная информация
Считаете, что обнаружили ошибку в платформе IoT Edge? [Отправьте запрос](https://github.com/Azure/iot-edge/issues), чтобы мы как можно скорее устранили неисправность. 
