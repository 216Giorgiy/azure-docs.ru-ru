---
title: Настройка входящего трафика с помощью кластера Службы контейнеров Azure (AKS)
description: Установка и настройка контроллера входящего трафика NGINX в кластере Службы контейнеров Azure (AKS).
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/03/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 908910b44a9de28f184906dd4e904e651fe034ce
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2018
---
# <a name="https-ingress-on-azure-container-service-aks"></a>Входящий трафик HTTPS в Службе контейнеров Azure (AKS)

Контроллер входящего трафика является программным компонентом, предоставляющим для служб Kubernetes обратный прокси-сервер, настраиваемую маршрутизацию трафика и обработку подключений TLS для последующей передачи. Ресурсы входящего трафика Kubernetes используются при настройке правил входящего трафика и маршрутов для отдельных служб Kubernetes. Применяя контроллер и правила входящего трафика, для маршрутизации трафика в несколько служб в кластере Kubernetes можно использовать один внешний адрес.

В этом документе описывается пример развертывания [контроллера входящего трафика NGINX][nginx-ingress] в кластере Службы контейнеров Azure (AKS). Кроме того, проект [KUBE-LEGO][kube-lego] используется для автоматического создания и настройки сертификатов [Let's Encrypt][lets-encrypt]. Наконец, в кластере AKS запущено несколько приложений, каждое из которых доступно по одному адресу.

## <a name="install-an-ingress-controller"></a>Установка контроллера входящего трафика

Используйте Helm для установки контроллера входящего трафика NGINX. Дополнительные сведения см. в [документации по контроллеру входящего трафика NGINX][nginx-ingress]. 

Обновите репозиторий с диаграммой.

```console
helm repo update
```

Установите контроллер входящего трафика NGINX.

```
helm install stable/nginx-ingress
```

Во время установки для контроллера входящего трафика создается общедоступный IP-адрес Azure. Чтобы получить общедоступный IP-адрес, используйте команду kubectl get service. Процесс назначения службе IP-адреса может занять некоторое время.

```console
$ kubectl get service

NAME                                          TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE
kubernetes                                    ClusterIP      10.0.0.1       <none>           443/TCP                      3d
toned-spaniel-nginx-ingress-controller        LoadBalancer   10.0.236.223   52.224.125.195   80:30927/TCP,443:31144/TCP   18m
toned-spaniel-nginx-ingress-default-backend   ClusterIP      10.0.5.86      <none>           80/TCP                       18m
```

Так как правила входящего трафика не созданы, при переходе к общедоступному IP-адресу по умолчанию будет отображаться страница с ошибкой "404 — страница не найдена" для контролеров входящего трафика NGINX.

![Сервер NGINX по умолчанию](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Настройка DNS-имени

Так как используются сертификаты HTTPS, для IP-адреса контроллеров входящего трафика необходимо настроить полное доменное имя. В этом примере полное доменное имя в Azure создается с помощью Azure CLI. Укажите в скрипте IP-адрес контроллера входящего трафика и имя, которое вы хотите использовать в качестве полного доменного имени.

```
#!/bin/bash

# Public IP address
IP="52.224.125.195"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get resource group and public ip name
RESOURCEGROUP=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[resourceGroup]" --output tsv)
PIPNAME=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[name]" --output tsv)

# Update public ip address with dns name
az network public-ip update --resource-group $RESOURCEGROUP --name  $PIPNAME --dns-name $DNSNAME
```

При необходимости запустите следующую команду, чтобы получить полное доменное имя. Укажите значение IP-адреса контроллера входящего трафика.

```azurecli
az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '52.224.125.195')].[dnsSettings.fqdn]" --output tsv
```

Теперь к контроллеру входящего трафика можно получить доступ по полному доменному имени.

## <a name="install-kube-lego"></a>Установка KUBE-LEGO

Контроллер входящего трафика NGINX поддерживает обработку подключений TLS для последующей передачи. Существует несколько способов получения и настройки сертификатов для использования протокола HTTPS. В этом документе демонстрируется способ с использованием [KUBE-LEGO][kube-lego], который обеспечивает автоматические возможности создания сертификата [Lets Encrypt][lets-encrypt] и управления им.

Чтобы установить контроллер KUBE-LEGO, используйте следующую команду установки Helm. Укажите адрес электронной почты вашей организации.

```
helm install stable/kube-lego \
  --set config.LEGO_EMAIL=user@contoso.com \
  --set config.LEGO_URL=https://acme-v01.api.letsencrypt.org/directory
```

Дополнительные сведения о настройке KUBE-LEGO см. в [этой документации][kube-lego].

## <a name="run-application"></a>Запуск приложения

На этом этапе уже настроены контроллер входящего трафика и решение по управлению сертификатами. Теперь запустите несколько приложений в кластере AKS.

В этом примере Helm применяется для запуска нескольких экземпляров простого приложения Hello World.

Перед запуском приложения добавьте репозиторий Helm образцов Azure в систему разработки.

```
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

 Запустите диаграмму AKS для приложения Hello World, используя следующую команду.

```
helm install azure-samples/aks-helloworld
```

Теперь установите второй экземпляр приложения Hello World.

Укажите новый заголовок для второго экземпляра, чтобы оба приложения визуально отличались. Вам также необходимо задать уникальное имя службы. Эти конфигурации видны в следующей команде.

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Настройка маршрутизации входящего трафика

Оба приложения запущены на кластере Kubernetes, несмотря на то что были настроены с помощью службы типа `ClusterIP`. Таким образом приложения не доступны через Интернет. Чтобы сделать их доступными, создайте ресурс входящего трафика Kubernetes. Ресурс входящего трафика настраивает правила, по которым осуществляется маршрутизация трафика к одному из двух приложений.

Создайте файл `hello-world-ingress.yaml` и скопируйте в него следующий код YAML.

Обратите внимание, что трафик по адресу `https://demo-aks-ingress.eastus.cloudapp.azure.com/` направляется в службу `aks-helloworld`. Трафик по адресу `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` направляется в службу `ingress-demo`.

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/tls-acme: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Создайте ресурс входящего трафика с помощью команды `kubectl apply`.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Проверка конфигурации входящего трафика

Перейдите к контроллеру входящего трафика Kubernetes по полному доменному имени. Вы увидите приложение Hello World.

![Первый пример приложения](media/ingress/app-one.png)

Теперь перейдите к контроллеру входящего трафика по полному доменному имени, используя путь `/hello-world-two`. Вы увидите приложение Hello World с пользовательским заголовком.

![Второй пример приложения](media/ingress/app-two.png)

Обратите также внимание на то, что соединение зашифровано и используется сертификат, полученный через Let's Encrypt.

![Сертификат Let's Encrypt](media/ingress/certificate.png)

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о программном обеспечении, рассматриваемом в этом документе: 

- [Контроллер входящего трафика NGINX][nginx-ingress]
- [KUBE-LEGO][kube-lego]

<!-- LINKS - external -->
[kube-lego]: https://github.com/jetstack/kube-lego
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
