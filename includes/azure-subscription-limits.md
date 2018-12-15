---
title: включение файла
description: включение файла
services: billing
author: rothja
ms.service: billing
ms.topic: include
ms.date: 05/18/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: be75b982d3e63ecb1edd7398e32150156a06995d
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52886350"
---
| Ресурс | Ограничение по умолчанию | Максимальное ограничение |
| --- | --- | --- |
| Число виртуальных ЦП на [подписку](../articles/billing-buy-sign-up-azure-subscription.md) <sup>1</sup> |20 |10 000 |
| [Дополнительные администраторы](../articles/billing-add-change-azure-subscription-administrator.md) на подписку |200 |200 |
| [Учетные записи хранения](../articles/storage/common/storage-create-storage-account.md) на каждую подписку <sup>2</sup> |100 |100 |
| [Облачные службы](../articles/cloud-services/cloud-services-choose-me.md) на подписку |20 |200 |
| [Локальные сети](https://msdn.microsoft.com/library/jj157100.aspx) на подписку |10 |500 |
| Серверы DNS на подписку |9 |100 |
| Зарезервированные IP на подписку |20 |100 |
| [Территориальные группы](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) на подписку |256 |256 |


<sup>1</sup>Сверхмалые экземпляры считаются как один виртуальный ЦП в отношении ограничения числа виртуальных ЦП, даже если ядро ЦП загружено не полностью.

<sup>2</sup>Ограничение учетной записи хранения распространяются на учетные записи хранения класса "Стандартный" и "Премиум". 

