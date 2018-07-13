---
title: Создание многоконтейнерного приложения (предварительная версия) на платформе Azure "Веб-приложение для контейнеров" с использованием конфигурации Docker Compose
description: Разверните свое первое многоконтейнерное приложение на платформе Azure "Веб-приложение для контейнеров" за считаные минуты
keywords: служба приложений azure, веб-приложение, linux, docker, compose, многоконтейнерное, несколько контейнеров, веб-приложение для контейнеров, много контейнеров, контейнер, kubernetes, wordpress, azure db для mysql, рабочая база данных с контейнерами
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: bf567402a66f9152c7eb9b97925fec2a159ffe56
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127426"
---
# <a name="create-a-multi-container-preview-app-using-web-app-for-containers"></a>Создание многоконтейнерного приложения (предварительная версия) с помощью платформы "Веб-приложение для контейнеров"

Платформа [Веб-приложение для контейнеров](app-service-linux-intro.md) предоставляет гибкие возможности для использования образов Docker. В этом кратком руководстве показано, как в [Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) развернуть многоконтейнерное приложение на платформе "Веб-приложение для контейнеров", используя конфигурацию Docker Compose. Для получения дополнительных сведений о Kubernetes и комплексном решении с базой данных Azure для MySQL см. [руководство по многоконтейнерным приложениям](tutorial-multi-container-app.md).

Действия, описанные в этом руководстве, выполняются в Cloud Shell, но эти же команды можно выполнить локально в [Azure CLI](/cli/azure/install-azure-cli) (2.0.32 или более поздней версии). 

![Пример многоконтейнерного приложения на платформе "Веб-приложение для контейнеров"][1]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Скачивание примера приложения

В этом кратком руководстве используется файл compose из [Docker](https://docs.docker.com/compose/wordpress/#define-the-project). Файл конфигурации можно найти в [репозитории примеров для Azure](https://github.com/Azure-Samples/multicontainerwordpress).

[!code-yml[Main](../../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

В Cloud Shell создайте каталог quickstart и перейдите в него.

```bash
mkdir quickstart

cd quickstart
```

Затем выполните следующую команду, чтобы клонировать репозиторий с примером приложения в локальный каталог quickstart. Затем перейдите в каталог `multicontainerwordpress`.

```bash
git clone https://github.com/Azure-Samples/multicontainerwordpress

cd multicontainerwordpress
```

## <a name="create-a-resource-group"></a>Создание группы ресурсов

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

В Cloud Shell создайте группу ресурсов с помощью команды [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create). В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *South Central US*. Чтобы просмотреть все поддерживаемые расположения для службы приложений в Linux на уровне **Стандартный**, выполните команду [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations).

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

Обычно группа ресурсов и ресурсы создаются в ближайших регионах.

По завершении команды в выходных данных JSON будут отображаться свойства группы ресурсов.

## <a name="create-an-azure-app-service-plan"></a>Создание плана службы приложений Azure

В Cloud Shell создайте план службы приложений в группе ресурсов с помощью команды [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create).

В следующем примере создается план службы приложений с именем `myAppServicePlan` в ценовой категории **Стандартный** (`--sku S1`) в контейнере Linux (`--is-linux`).

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

После создания плана службы приложений в Azure CLI отображается информация следующего вида:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

## <a name="create-a-docker-compose-app"></a>Создание приложения Docker Compose

В терминале Cloud Shell создайте многоконтейнерное [веб-приложение](app-service-linux-intro.md) в рамках плана службы приложений `myAppServicePlan` с помощью команды [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Не забудьте указать уникальное имя приложения вместо _\<app_name>_.

```bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Когда веб-приложение будет создано, в Azure CLI отобразится примерно следующее:

```json
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="browse-to-the-app"></a>Переход в приложение

Перейдите в развернутое приложение по адресу `http://<app_name>.azurewebsites.net`. Для загрузки приложения может потребоваться несколько минут. Если произойдет ошибка, подождите несколько минут, а затем обновите страницу в браузере.

![Пример многоконтейнерного приложения на платформе "Веб-приложение для контейнеров"][1]

**Поздравляем!** Вы создали многоконтейнерное приложение на платформе "Веб-приложение для контейнеров".

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание многоконтейнерного приложения WordPress на платформе Azure "Веб-приложение для контейнеров"](tutorial-multi-container-app.md)

<!--Image references-->
[1]: ./media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png