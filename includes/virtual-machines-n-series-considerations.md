---
title: включение файла
description: включение файла
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 4bec8c8ea29c10b8c0d0351a41ebc9183bb45d4f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449517"
---
## <a name="deployment-considerations"></a>Рекомендации по развертыванию

* Сведения о доступности виртуальных машин серии N по регионам см. на [этой странице](https://azure.microsoft.com/regions/services/).

* Виртуальные машины серии N поддерживают только модель развертывания с помощью Resource Manager.

* Виртуальные машины серии N отличаются типом хранилища Azure, которое поддерживается для их дисков. Виртуальные машины серий NC и NV поддерживают только диски на основе хранилища дисков уровня "Стандартный" (HDD). Виртуальные машины серий NCv2, ND и NCv3 поддерживают только диски на основе хранилища дисков уровня "Премиум" (SSD).

* Если вы хотите развернуть большое количество виртуальных машин серии N, мы рекомендуем подписку с оплатой по мере использования или другие варианты покупки. Если вы используете [бесплатную учетную запись Azure](https://azure.microsoft.com/free/), вам доступно ограниченное количество вычислительных ядер Azure.

* Возможно, вам потребуется увеличить квоту на использование ядер (для каждого региона) в подписке Azure и отдельную квоту для ядер серии NC, NCv2, NCv3, ND или NV. Чтобы увеличить квоту, [отправьте запрос в службу поддержки](../articles/azure-supportability/how-to-create-azure-support-request.md). Это бесплатная услуга. Ограничения по умолчанию отличаются в зависимости от категории подписки.




