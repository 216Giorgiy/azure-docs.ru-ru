---
title: Создание рабочей области Log Analytics с помощью Azure CLI | Документация Майкрософт
description: Узнайте, как создать рабочую область Log Analytics, чтобы включить решения по управлению и сбор данных из облачной и локальной сред с помощью Azure CLI.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: magoedte
ms.openlocfilehash: bc3b96ee55ccd28cce89b1f37494b836851977f1
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259253"
---
# <a name="create-a-log-analytics-workspace-with-azure-cli-20"></a>Создание рабочей области Log Analytics с помощью Azure CLI 2.0

Azure CLI 2.0 используется для создания ресурсов Azure и управления ими из командной строки или с помощью сценариев. В этом кратком руководстве показано, как использовать Azure CLI, чтобы развернуть рабочее пространство Log Analytics в Azure, которое представляет собой уникальную среду с собственным репозиторием данных, источниками данных и решениями.  Действия, описанные в этой статье, помогут вам настроить сбор данные из следующих источников:

* ресурсы Azure в подписке;  
* локальные компьютеры, которые отслеживает System Center Operations Manager;  
* коллекции устройств из System Center Configuration Manager;  
* Данные диагностики и журнала из службы хранилища Azure  
 
Сведения о других источниках, таких как виртуальные машины Azure и виртуальные машины Windows или Linux в вашей среде, см. в статьях ниже.

* [Сбор данных о виртуальных машинах Azure](../../azure-monitor/learn/quick-collect-azurevm.md)
* [Настройка агента Log Analytics для компьютеров Linux в гибридной среде](../../azure-monitor/learn/quick-collect-linux-computer.md)
* [Настройка агента Log Analytics для компьютеров Windows в гибридной среде](quick-collect-windows-computer.md)

Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, для выполнения инструкций из этого руководства вам потребуется Azure CLI 2.0.30 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Создание рабочей области
Создайте рабочую область с помощью команды [az group deployment create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). В следующем примере создается рабочая область с именем *TestWorkspace* в группе ресурсов *Lab* в регионе *eastus* с помощью шаблона Resource Manager на локальном компьютере. Шаблон в формате JSON настроен так, чтобы осталось только указать имя рабочей области и задать значения по умолчанию для других параметров, которые скорее всего будут использоваться в качестве стандартной конфигурации в вашей среде. Кроме того, шаблон можно хранить в учетной записи хранения Azure для общего доступа в организации. Дополнительную информацию о работе с шаблонами см. в статье [Развертывание ресурсов с использованием шаблонов Resource Manager и Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md).

Для следующих параметров задаются значения по умолчанию.

* location — по умолчанию используется значение East US.
* sku — по умолчанию используется новый тарифный план с платой за гигабайт, выпущенный в апреле 2018 года.

>[!WARNING]
>При создании или настройке рабочей области Log Analytics в подписке, использующей модель ценообразования от апреля 2018 года, доступна только ценовая категория **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Создание и развертывание шаблона

1. Скопируйте и вставьте в него следующий синтаксис JSON:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. Отредактируйте шаблон с учетом ваших требований.  Просмотрите справочник по [шаблону Microsoft.OperationalInsights/workspaces](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) с описанием поддерживаемых свойств и значений. 
3. Сохраните этот файл как **deploylaworkspacetemplate.json** в локальной папке.   
4. Теперь вы можете развернуть этот шаблон. Используйте следующие команды из папки, содержащей шаблон:

    ```azurecli
    azure group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deploylaworkspacetemplate.json
    ```

Развертывание может занять несколько минут. По завершении выполнения появится сообщение с результатами наподобие приведенного ниже.

![Пример результатов по завершении развертывания](media/quick-create-workspace-cli/template-output-01.png)

## <a name="next-steps"></a>Дополнительная информация
Теперь, когда рабочая область доступна, вы можете настроить сбор данных телеметрии для мониторинга, выполнять поиск по журналам для анализа этих данных, а также добавить решение по управлению для предоставления дополнительных данных и аналитических сведений.  

* Сведения о том, как включить сбор данных из ресурсов Azure с помощью системы диагностики Azure или службы хранилища Azure, см. в статье [Сбор журналов и метрик для служб Azure для использования в Log Analytics](../../azure-monitor/platform/collect-azure-metrics-logs.md).  
* Добавьте [System Center Operations Manager в качестве источника данных](../../azure-monitor/platform/om-agents.md), чтобы собирать данные с агентов, которые предоставляют отчеты группе управления Operations Manager, и хранить эти данные в рабочей области Log Analytics.  
* Подключите [Configuration Manager](../../azure-monitor/platform/collect-sccm.md) для импорта данных с компьютеров, которые являются элементами коллекций в иерархии.  
* Просмотрите список доступных [решений по управлению](../../azure-monitor/insights/solutions.md) и узнайте, как добавить решение в рабочую область или удалить его из нее.
