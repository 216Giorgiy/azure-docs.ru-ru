---
title: Общие сведения об использовании OpenShift в Azure | Документация Майкрософт
description: Общие сведения об использовании OpenShift в Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/19/2019
ms.author: haroldw
ms.openlocfilehash: 53bed2131e81ee5ed0f46bde389262ee8349339a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006970"
---
# <a name="openshift-in-azure"></a>Использование OpenShift в Azure

OpenShift — это открытая и расширяемая платформа приложений-контейнеров, которая позволяет использовать Docker и Kubernetes на предприятии.  

OpenShift включает Kubernetes для оркестрации контейнеров и управления ими. Здесь добавлены инструменты для разработчиков и операций, позволяющие:

- быстро разрабатывать приложения;
- легко развертывать и масштабировать решения;
- обслуживать жизненный цикл команд и приложений в долгосрочной перспективе.

Существует несколько версий OpenShift.  Из этих версий только два доступны сегодня клиентам самостоятельно развертывать в Azure: Платформа контейнеров OpenShift и OKD (прежнее название — OpenShift Origin).

## <a name="openshift-container-platform"></a>Платформа контейнеров OpenShift

OpenShift Container Platform — это [коммерческая версия](https://www.openshift.com) корпоративного класса, которая разрабатывается и поддерживается компанией Red Hat. Чтобы использовать эту версию, клиенты должен приобрести необходимые права доступа к платформе OpenShift Container Platform. Кроме того, они сами устанавливают и администрируют всю инфраструктуру.

Поскольку клиенты «владел» всей платформы, их можно установить в их локальном центре обработки данных или в общедоступном облаке (Azure).

## <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Azure Red Hat OpenShift — это полностью управляемое предложение OpenShift в Azure. Эта служба управляется и поддерживается совместно корпорацией Майкрософт и Red Hat. Кластер будет развернут в подписку Azure клиента. Планируется, что служба является GA вокруг мая 2019. Специальная документация для управляемой службы, будут доступны после выпуска общедоступной версии службы

## <a name="okd"></a>OKD

OKD — это высокоуровневый проект OpenShift с [открытым кодом](https://www.okd.io/), который поддерживается сообществом. OKD можно установить в ОС CentOS или Red Hat Enterprise Linux (RHEL).

## <a name="next-steps"></a>Дальнейшие действия

- [Общие предварительные требования для OpenShift в Azure](./openshift-prerequisites.md)
- [Развертывание платформы OpenShift Container Platform](./openshift-container-platform.md)
- [Развертывание предложения самостоятельным управлением Marketplace платформы контейнеров OpenShift](./openshift-marketplace-self-managed.md)
- [Развертывание OpenShift в Azure Stack](./openshift-azure-stack.md)
- [Задачи, выполняемые после развертывания](./openshift-post-deployment.md)
- [Устранение неполадок с развертыванием OpenShift](./openshift-troubleshooting.md)
