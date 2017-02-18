---
title: "Развертывание кластера контейнера Docker с помощью Azure CLI | Документация Майкрософт"
description: "Развертывание кластера Службы контейнеров Azure с использованием предварительной версии интерфейса командной строки Azure 2.0"
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: saudas
translationtype: Human Translation
ms.sourcegitcommit: df916670743158d6a22b3f17343630114584fa08
ms.openlocfilehash: 65f1c812472f4a3b6d4a4e6fb7666a2c022af102


---
# <a name="using-the-azure-cli-20-preview-to-create-an-azure-container-service-cluster"></a>Использование предварительной версии интерфейса командной строки Azure 2.0 для создания кластера Службы контейнеров Azure

Чтобы создать кластер и управлять им в Службе контейнеров Azure, используйте команды `az acs` в предварительной версии интерфейса командной строки Azure 2.0. Вы также можете развернуть кластер Службы контейнеров Azure с помощью [портала Azure](container-service-deployment.md) или с помощью интерфейсов API Службы контейнеров Azure.

Чтобы получить справку по командам `az acs`, добавьте параметр `-h` в любую команду. Например, `az acs create -h`.



## <a name="prerequisites"></a>Предварительные требования
Чтобы создать кластер Службы контейнеров Azure с помощью предварительной версии интерфейса командной строки Azure 2.0, необходимо следующее:
* учетная запись Azure ([получите бесплатную пробную версию](https://azure.microsoft.com/pricing/free-trial/));
* установленный и настроенный [Azure CLI версии 2.0 (предварительная версия)](/cli/azure/install-az-cli2).

## <a name="get-started"></a>Начало работы 
### <a name="log-in-to-your-account"></a>Вход в учетную запись
```azurecli
az login 
```

Следуйте указаниям, чтобы выполнить вход в интерактивном режиме. Другие способы входа см. в статье [Get started with AzureCLI 2.0 (Preview)](/cli/azure/get-started-with-az-cli2) (Начало работы с Azure CLI 2.0 (предварительная версия)).

### <a name="set-your-azure-subscription"></a>Настройка подписки Azure

При наличии нескольких подписок Azure укажите подписку по умолчанию. Например:

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a>Создание группы ресурсов
Мы рекомендуем создать группу ресурсов для каждого кластера. Укажите регион Azure, в котором [доступна](https://azure.microsoft.com/en-us/regions/services/) Служба контейнеров Azure. Например:

```azurecli
az group create -n acsrg1 -l "westus"
```
Результат аналогичен приведенному ниже:

![Создание группы ресурсов](media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a>Создание кластера Службы контейнеров Azure

Чтобы создать кластер, используйте команду `az acs create`.
Имя кластера и имя группы ресурсов, созданной на предыдущем шаге, являются обязательными. 

Для других входных данных устанавливаются значения по умолчанию (см. снимок экрана), если они не переопределены с помощью соответствующих параметров. Например, для orchestrator по умолчанию установлено значение DC/OS. Если префикс DNS-имени не указан, он будет создан на основе имени кластера.

![Использование az acs create](media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a>Быстрое выполнение `acs create` с использованием значений по умолчанию
Если у вас есть файл открытого ключа SSH `id_rsa.pub` в расположении по умолчанию (или вы его создали для [OS X и Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) или [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md)), используйте следующую команду:

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
Если у вас нет открытого ключа SSH, используйте вторую команду. Эта команда с переключателем `--generate-ssh-keys` создается автоматически.

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

После введения команды подождите около 10 минут, пока будет создан кластер. Выходные данные команды содержат полные доменные имена (FQDN) главных узлов и узлов агентов и команду SSH для подключения к первому главному узлу. Ниже приведены сокращенные выходные данные.

![Изображение создания ACS](media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> В статье [Обработчик Службы контейнеров Microsoft Azure. Пошаговое руководство по использованию Kubernetes](container-service-kubernetes-walkthrough.md) показано, как использовать `az acs create` со значениями по умолчанию для создания кластера Kubernetes.
>

## <a name="manage-acs-clusters"></a>Управление кластерами ACS

Для управления кластером используйте дополнительные команды `az acs`. Ниже приведены некоторые примеры.

### <a name="list-clusters-under-a-subscription"></a>Получение списка кластеров в рамках подписки

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a>Получение списка кластеров в группе ресурсов

```azurecli
az acs list -g acsrg1 --output table
```

![acs list](media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a>Отображение сведений о кластере службы контейнеров

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![acs show](media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-the-cluster"></a>Масштабирование кластера
Увеличение и уменьшение масштабирования узлов агента разрешено. Параметр `new-agent-count` представляет собой новое количество агентов в кластере ACS.

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![acs scale](media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a>Удаление кластера службы контейнеров
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
Эта команда не удаляет все ресурсы (сеть и хранилище), созданные вместе со службой контейнеров. Чтобы легко удалить все ресурсы, рекомендуется развертывать каждый кластер в отдельной группе ресурсов. Если кластер больше не используется, группу ресурсов можно удалить.

## <a name="next-steps"></a>Дальнейшие действия
Теперь, когда у вас есть работающий кластер, выберите ссылки ниже, чтобы узнать о возможностях подключения и управления.

* [Подключение к кластеру службы контейнеров Azure](container-service-connect.md)
* [Работа со службой контейнеров Azure и DC/OS](container-service-mesos-marathon-rest.md)
* [Работа со службой контейнеров Azure и Docker Swarm](container-service-docker-swarm.md)
* [Работа со службой контейнеров Azure и Kubernetes](container-service-kubernetes-walkthrough.md)


<!--HONumber=Feb17_HO1-->


