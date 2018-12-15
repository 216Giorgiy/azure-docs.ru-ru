---
title: Использование Ansible с Azure
description: Основные сведения об использовании Ansible для автоматизированной подготовки облачных решений, управления конфигурацией и развертывания приложений.
ms.service: ansible
keywords: ansible, azure, devops, overview, cloud provision, configuration management, application deployment, ansible modules, ansible playbooks
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 09/02/2018
ms.topic: article
ms.openlocfilehash: 22eeb3993cd408a8369236683da3db466a348a30
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956785"
---
# <a name="ansible-with-azure"></a>Использование Ansible с Azure

[Ansible](https://www.ansible.com) — этот продукт с открытым кодом, который автоматизирует подготовку облачных решений, управление конфигурацией и развертывание приложений. С помощью Ansible можно подготовить виртуальные машины, контейнеры и сети, а также готовые облачные инфраструктуры. Кроме того, ПО Ansible позволяет автоматизировать развертывание и настройку ресурсов в среде.

Эта статья описывает некоторые преимущества использования Ansible с Azure.

## <a name="ansible-playbooks"></a>Модули playbook Ansible

[Модули playbook Ansible](https://docs.ansible.com/ansible/latest/playbooks.html) — это язык Ansible для настройки, развертывания и оркестрации. Они могут описывать политики, которые вы хотите применять в удаленных системах, или набор шагов в универсальном ИТ-процессе. При создании playbook используется формат YAML, который определяет модель конфигурации или процесса.

## <a name="ansible-modules"></a>Модули Ansible

Ansible содержит набор [модулей Ansible](https://docs.ansible.com/ansible/latest/modules_by_category.html), которые можно выполнять непосредственно на удаленных узлах или с помощью модулей [playbook](https://docs.ansible.com/ansible/latest/playbooks.html). Пользователи также могут создавать собственные модули. Модули можно использовать для управления системными ресурсами, например службами, пакетами или файлами, или выполнения системных команд.

Для взаимодействия со службами Azure в Ansible есть набор [облачных модулей Ansible](https://docs.ansible.com/ansible/list_of_cloud_modules.html#azure), который позволяет легко создавать инфраструктуру в Azure и управлять ею. 

## <a name="migrate-existing-workload-to-azure"></a>Миграция существующей рабочей нагрузки в Azure

Если вы определили инфраструктуру с помощью Ansible, вы можете применить playbook приложения, чтобы платформа Azure автоматически масштабировала вашу среду при необходимости. 

## <a name="automate-cloud-native-application-in-azure"></a>Автоматизация нативного облачного приложения в Azure

Ansible позволяет автоматизировать собственные облачные приложения в Azure с помощью таких микрослужб Azure, как [Функции Azure](https://azure.microsoft.com//services/functions/) и [Kubernetes в Azure](https://azure.microsoft.com/services/container-service/kubernetes/).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Управление развертываниями с помощью динамического списка
С помощью [динамического списка](https://docs.ansible.com/ansible/intro_dynamic_inventory.html) в Ansible можно извлекать список из ресурсов Azure. Вы можете обозначить тегами существующие развертывания Azure и управлять этими отмеченными развертываниями с помощью Ansible.

## <a name="additional-azure-marketplace-options"></a>Дополнительные параметры Azure Marketplace
Образ [Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) Red Hat, доступный в Azure Marketplace, позволяет организациям автоматизировать ИТ-процессы в нужном масштабе и управлять сложными развертываниями в физической, виртуальной или облачной инфраструктуре. Ansible Tower включает возможности, которые обеспечивают дополнительные уровни видимости, управления, безопасности и эффективности, необходимые современным предприятиям. Ansible Tower шифрует учетные данные, такие как ключи Azure и SSH, чтобы можно было делегировать задания менее опытным сотрудникам без риска раскрытия учетных данных.

## <a name="ansible-module-and-version-matrix-for-azure"></a>Таблица версий и модулей Ansible для Azure
Ansible поставляется с большим количеством модулей, которые могут выполняться прямо на удаленных узлах или с помощью наборов инструкций playbook.
В статье [Таблица версий и модулей Ansible](./ansible-matrix.md) перечислены модули Ansible для Azure, которые могут использоваться для подготовки таких облачных ресурсов Azure, как виртуальные машины, сети и службы контейнеров. 

## <a name="next-steps"></a>Дополнительная информация
- [Install and configure Ansible to manage virtual machines in Azure](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json) (Установка и настройка Ansible для управления виртуальными машинами в Azure)
- [Создание виртуальной машины Linux](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
