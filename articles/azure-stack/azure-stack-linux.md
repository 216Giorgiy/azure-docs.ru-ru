---
title: Добавление образов Linux в Azure Stack
description: Сведения о том, как добавлять образы Linux в Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: 277af6f139e815f46894f5b8df82a1d92ef573d1
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537139"
---
# <a name="add-linux-images-to-azure-stack"></a>Добавление образов Linux в Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Виртуальные машины Linux можно развернуть в Azure Stack, добавив образ на базе Linux в Azure Stack Marketplace. Проще всего добавить образ Linux в Azure Stack с помощью управления Marketplace. Эти образы были подготовлены и протестированы на совместимость с Azure Stack.

## <a name="marketplace-management"></a>Управление Marketplace

Чтобы загрузить образы Linux из Azure Marketplace, выполните процедуры, описанные в статье [Скачивание элементов Marketplace из Azure в Azure Stack](azure-stack-download-azure-marketplace-item.md). Выберите образы Linux, которые вы хотите предложить пользователям в Azure Stack. 

Обратите внимание, что эти образы часто обновляются, поэтому чаще посещайте портал управления marketplace, чтобы обеспечить их актуальное состояние.

## <a name="prepare-your-own-image"></a>Подготовка собственного образа

По возможности скачивайте образы, доступные на портале управления marketplace. Они будут подготовлены и протестированы для Azure Stack.

Необходим агент Linux для Azure (обычно называется `WALinuxAgent` или `walinuxagent`), и не все версии агента будут работать с Azure Stack. При создании собственного образа необходимо использовать последнюю версию WALA или версию 2.2.20. Обратите внимание, что все промежуточные версии между 2.2.20 и 2.2.35.1 (не включая) не работают в Azure Stack. Обратите внимание на то, что в настоящее время [cloud-init](https://cloud-init.io/) не поддерживается в Azure Stack.

Можно подготовить свой собственный образ Linux с помощью следующих инструкций.

* [Подготовка виртуальной машины на основе CentOS для Azure](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Подготовка виртуального жесткого диска Debian для Azure](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Подготовка виртуальной машины на основе Red Hat для Azure](azure-stack-redhat-create-upload-vhd.md)
* [Подготовка виртуальной машины SLES или openSUSE для Azure](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Сервер Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="add-your-image-to-the-marketplace"></a>Добавление образа в marketplace

Следуйте указаниям по [добавлению образа в marketplace](azure-stack-add-vm-image.md). Убедитесь, что для параметра `OSType` задано значение `Linux`.

После добавления образа в Marketplace создается элемент Marketplace, и пользователи могут развернуть виртуальную машину Linux.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в этих статьях:

- [Скачивание элементов Marketplace из Azure в Azure Stack](azure-stack-download-azure-marketplace-item.md)
- [Общие сведения об Azure Stack Marketplace](azure-stack-marketplace.md)
