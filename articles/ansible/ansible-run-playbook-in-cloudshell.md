---
title: Запуск Ansible с помощью Bash в Azure Cloud Shell
description: Узнайте, как выполнять разные задачи Ansible с использованием Bash в Azure Cloud Shell.
ms.service: azure
keywords: ansible, azure, devops, bash, cloudshell, сборник тренировочных заданий, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: quickstart
ms.openlocfilehash: 37426f1cdc51734c28b2c9739e26ad35f08e606f
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791398"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Запуск Ansible с помощью Bash в Azure Cloud Shell

В этом руководстве вы узнаете, как использовать Bash в Cloud Shell для настройки подписки Azure в качестве рабочей области Ansible. 

## <a name="prerequisites"></a>Предварительные требования

- **Подписка Azure**. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Настройте Azure Cloud Shell**. Если вы еще не работали с Azure Cloud Shell, см. [краткое руководство по Bash в Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart), в котором объясняется, как запустить и настроить Cloud Shell. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Автоматическая настройка учетных данных

После входа в Cloud Shell Ansible выполняет проверку подлинности с помощью Azure для управления инфраструктурой без какой-либо дополнительной настройки. Если у вас есть несколько подписок, можно выбрать подписку, c которой Ansible будет работать, экспортируя переменную среду `AZURE_SUBSCRIPTION_ID`. Для получения списка всех подписок Azure выполните следующую команду.

```azurecli-interactive
az account list
```

При использовании **идентификатора** подписки, с которым необходимо работать, задайте **AZURE_SUBSCRIPTION_ID** следующее значение.

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Проверка конфигурации
Для проверки успешности конфигурации создайте группу ресурсов с помощью Ansible.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"] 
> [Создание базовой виртуальной машины в Azure с помощью Ansible](/azure/virtual-machines/linux/ansible-create-vm)