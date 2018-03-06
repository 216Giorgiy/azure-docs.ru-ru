---
title: "Интеграция со службами под управлением Azure с помощью открытого компонента Service Broker для Azure (OSBA)"
description: "Интеграция со службами под управлением Azure с помощью открытого компонента Service Broker для Azure (OSBA)"
services: container-service
author: sozercan
manager: timlt
ms.service: container-service
ms.topic: overview
ms.date: 12/05/2017
ms.author: seozerca
ms.openlocfilehash: 594cb0afbdb0a44e9f092b9afc5af13b21e763a4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="integrate-with-azure-managed-services-using-open-service-broker-for-azure-osba"></a>Интеграция со службами под управлением Azure с помощью открытого компонента Service Broker для Azure (OSBA)

Вместе с [каталогом услуг Kubernetes][kubernetes-service-catalog] открытый компонент Service Broker для Azure (OSBA) позволяет разработчикам использовать службы под управлением Azure в Kubernetes. Это руководство посвящено развертыванию каталога услуг Kubernetes, открытого компонента Service Broker для Azure (OSBA) и приложений, использующих службы под управлением Azure с помощью Kubernetes.

## <a name="prerequisites"></a>предварительным требованиям
* Подписка Azure

* Azure CLI 2.0. Вы можете [установить его локально][azure-cli-install] или использовать в [Azure Cloud Shell][azure-cloud-shell].

* Helm CLI 2.7+. Вы можете [установить его локально][helm-cli-install] или использовать в [Azure Cloud Shell][azure-cloud-shell].

* Разрешения для создания субъекта-службы с ролью "Участник" для подписки Azure.

* Имеющийся кластер Службы контейнеров Azure (AKS). Если вам нужен кластер службы контейнеров Azure, выполните действия из краткого руководства по [созданию кластера AKS][create-aks-cluster].

## <a name="install-service-catalog"></a>Установка каталога услуг

Сначала установите каталог услуг в кластере Kubernetes с помощью чарта Helm. Обновите установку Tiller (сервер Helm) в кластере с помощью этой команды.

```azurecli-interactive
helm init --upgrade
```

Теперь добавьте диаграмму "Каталог услуг" в репозиторий Helm.

```azurecli-interactive
helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com
```

Наконец, установите каталог услуг с чартом Helm.

```azurecli-interactive
helm install svc-cat/catalog --name catalog --namespace catalog --set rbacEnable=false
```

После запуска чарта Helm убедитесь, что в `servicecatalog` появляются выходные данные следующей команды.

```azurecli-interactive
kubectl get apiservice
```

Например, вы должны увидеть результат, аналогичный приведенному ниже (показанный в усеченном виде).

```
NAME                                 AGE
v1.                                  10m
v1.authentication.k8s.io             10m
...
v1beta1.servicecatalog.k8s.io        34s
v1beta1.storage.k8s.io               10
```

## <a name="install-open-service-broker-for-azure"></a>Установка открытого компонента Service Broker для Azure

Далее установите [открытый компонент Service Broker для Azure][open-service-broker-azure], включая каталог для служб под управлением Azure. Примеры доступных служб Azure: База данных Azure для PostgreSQL, кэш Redis для Azure, База данных Azure для MySQL, Azure Cosmos DB, База данных SQL Azure и др.

Начните с добавления открытого компонента Service Broker для репозитория Azure Helm.

```azurecli-interactive
helm repo add azure https://kubernetescharts.blob.core.windows.net/azure
```

Затем используйте следующий скрипт для создания [субъекта-службы][create-service-principal] и заполните несколько переменных. Эти переменные используются при выполнении чарта Helm, чтобы установить компонент Service Broker.

```azurecli-interactive
SERVICE_PRINCIPAL=$(az ad sp create-for-rbac)
AZURE_CLIENT_ID=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 4)
AZURE_CLIENT_SECRET=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 16)
AZURE_TENANT_ID=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 20)
AZURE_SUBSCRIPTION_ID=$(az account show --query id --output tsv)
```

Указав эти переменные среды, выполните следующую команду, чтобы установить компонент Service Broker.

```azurecli-interactive
helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET
```

После развертывания OSBA установите [интерфейс командной строки каталога услуг][service-catalog-cli], простой интерфейс командной строки для обращения к брокерам служб, классам служб, планам обслуживания и т. д.

Выполните следующие команды для установки двоичных файлов интерфейса командной строки каталога услуг.

```azurecli-interactive
curl -sLO https://servicecatalogcli.blob.core.windows.net/cli/latest/$(uname -s)/$(uname -m)/svcat
chmod +x ./svcat
```

Теперь получите список брокеров установленной службы.

```azurecli-interactive
./svcat get brokers
```

Вы должны увидеть результат, аналогичный приведенному ниже:

```
  NAME                               URL                                STATUS
+------+--------------------------------------------------------------+--------+
  osba   http://osba-open-service-broker-azure.osba.svc.cluster.local   Ready
```

Затем получите список доступных классов службы. Отображаемые классы службы являются доступными службами под управлением Azure, которые могут предоставляться через открытый компонент Service Broker для Azure.

```azurecli-interactive
./svcat get classes
```

Наконец, получите список всех планов обслуживания. Они являются уровнями служб для служб под управлением Azure. Например, для Базы данных Azure для MySQL доступны планы от `basic50` для уровня "Базовый" с 50 единицами транзакций базы данных (DTU) до `standard800` для уровня "Стандартный" с 800 DTU.

```azurecli-interactive
./svcat get plans
```

## <a name="install-wordpress-from-helm-chart-using-azure-database-for-mysql"></a>Установка WordPress из чарта Helm с помощью Базы данных Azure для MySQL

На этом шаге используется Helm для установки обновленного чарта Helm для WordPress. Чарт подготавливает внешний экземпляр Базы данных Azure для MySQL, который можно использовать в WordPress. Это может занять несколько минут.

```azurecli-interactive
helm install azure/wordpress --name wordpress --namespace wordpress --set resources.requests.cpu=0
```

Чтобы убедиться, что при установке подготовлены нужные ресурсы, перечислите экземпляры и привязки установленной службы.

```azurecli-interactive
./svcat get instances -n wordpress
./svcat get bindings -n wordpress
```

Получите список установленных секретов.

```azurecli-interactive
kubectl get secrets -n wordpress -o yaml
```

## <a name="next-steps"></a>Дополнительная информация

Следуя указаниям в этой статье, вы развернули каталог услуг в кластер Службы контейнеров Azure (AKS). Вы использовали открытый компонент Service Broker для Azure, чтобы развернуть установку WordPress, которая использует службы под управлением Azure (в данном случае — базу данных Azure для MySQL).

Ознакомьтесь с репозиторием [Azure/helm-charts][helm-charts], чтобы получить доступ к другим обновленным чартам Helm на основе OSBA. Если вы заинтересованы в создании собственных чартов, которые работают с OSBA, ознакомьтесь с [этим разделом][helm-create-new-chart].

<!-- LINKS - external -->
[helm-charts]: https://github.com/Azure/helm-charts
[helm-cli-install]: kubernetes-helm.md#install-helm-cli
[helm-create-new-chart]: https://github.com/Azure/helm-charts#creating-a-new-chart
[kubernetes-service-catalog]: https://github.com/kubernetes-incubator/service-catalog
[open-service-broker-azure]: https://github.com/Azure/open-service-broker-azure
[service-catalog-cli]: https://github.com/Azure/service-catalog-cli

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[create-aks-cluster]: ./kubernetes-walkthrough.md
[create-service-principal]: ./kubernetes-service-principal.md
