---
title: Обновление средств Azure Dev Spaces | Документация Майкрософт
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/03/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Быстрая разработка в Kubernetes с использованием контейнеров и микрослужб в Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
ms.openlocfilehash: ad4c608d773dd6838fa95ab05c59e09c29906f8a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2019
ms.locfileid: "55476491"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Обновление средств Azure Dev Spaces

Если вы уже используете Azure Dev Spaces, при выходе нового выпуска вам следует обновить клиентские средства Azure Dev Spaces.

## <a name="update-the-azure-cli"></a>Обновление Azure CLI

При обновлении до последней версии Azure CLI вы автоматически получаете последнюю версию расширения интерфейса командной строки для Azure Dev Spaces.

Предыдущую версию удалять не нужно, просто найдите нужный файл для скачивания на странице [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Обновление расширения CLI для Azure Dev Spaces и средств командной строки

Выполните следующую команду:

```cmd
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>Обновление расширения VS Code

После установки расширение автоматически обновляется. Возможно, потребуется перезагрузить расширение, чтобы использовать новые возможности. В VS Code откройте панель **Расширения**, выберите расширения **Azure Dev Spaces** и щелкните **Перезагрузить**.

## <a name="update-the-visual-studio-extension"></a>Обновление расширения Visual Studio

Как и для других расширений и обновлений, Visual Studio сообщит вам о наличии обновления средств Visual Studio для Kubernetes, в пакет которых входит Azure Dev Spaces. Найдите значок флага в верхней правой части экрана.

Чтобы обновить средства в Visual Studio, выберите пункты меню **Сервис > Расширения и обновления**, а затем слева выберите **Обновления**. Найдите **Средства Visual Studio для Kubernetes** и нажмите кнопку **Обновить**.

## <a name="next-steps"></a>Дополнительная информация

Протестируйте новые средства, создав новый кластер. Изучите руководства по [Azure Dev Spaces](/azure/dev-spaces).

> [!WARNING]
> Azure Dev Spaces в существующих кластерах не будут изменяться немедленно. Это означает, что для использования самой свежей версии для развертываний Azure следует создать новый кластер после обновления средств.
