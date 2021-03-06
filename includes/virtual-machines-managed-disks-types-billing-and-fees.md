---
title: включение файла
description: включение файла
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3e303ce2b6f28500406bacf5b66d26f9c78ba46d
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57783446"
---
**Передача исходящих данных.** [Передача исходящих данных](https://azure.microsoft.com/pricing/details/bandwidth/) (данных, передаваемых из центров обработки данных Azure) учитывается при определении платы за используемую пропускную способность.

**Транзакции.** Счет выставляется за количество транзакций, выполняемых на управляемом диске (цен. категория "Стандартный").

Подробные сведения о ценах на управляемые диски, включая затраты на транзакции, см. в разделе [цены на управляемые диски](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-ssd-vm-reservation-fee"></a>Плата за резервирование виртуальной машины с диском SSD (цен. категория "Ультра")

Виртуальные машины Azure поддерживают возможность указать, являются ли они совместимы с SSD (цен. категория "Ультра"). Виртуальная машина, совместимая с диском (цен. категория "Ультра"), предоставляет выделенную пропускную способность между вычислительным экземпляром виртуальной машины и единицей масштабирования блочного хранилища для оптимизации производительности и сокращения задержек. Если добавить эту возможность для виртуальной машины, это приведет к взиманию платы за резервирование, но это происходит, только если вы включили на виртуальной машине возможность использования диска (цен. категория "Ультра"), не подключая к ней сам диск. Если подключить диск (цен. категория "Ультра") к совместимой с ним виртуальной машине, эта плата не взимается. Эта плата взимается за каждый виртуальный ЦП, подготовленный на виртуальной машине.

Дополнительные сведения о ценах на диски (цен. категория "Ультра")см. [Страницу с ценами на диски Azure](https://azure.microsoft.com/pricing/details/managed-disks/).