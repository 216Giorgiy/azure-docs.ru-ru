---
title: Предложение виртуальной машины в Azure Marketplace | Документация Майкрософт
description: Общие сведения о процессе публикации предложения виртуальной машины в Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: bbe757ccd1d6a37cbcf04f3ecd6dd088ef1ff211
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353028"
---
# <a name="virtual-machine-offer"></a>Предложение виртуальной машины

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| В этом разделе объясняется, как опубликовать новое предложение виртуальной машины в [Azure Marketplace](https://azuremarketplace.microsoft.com). Поддержка предоставляется для виртуальных машин Windows или Linux, содержащих виртуальный жесткий диск (VHD) с операционной системой и ноль или несколько виртуальных жестких дисков данных. | ![Значок виртуальной машины](./media/virtual-machine-icon.png)  |


## <a name="publishing-overview"></a>Обзор процесса публикации

В следующем видеоролике, посвященном [оптимизации предложения Azure Marketplace](https://channel9.msdn.com/Events/Build/2017/P4026?ocid=player), содержится подробное описание Azure Marketplace, включая способы публикации в этом ресурсе (используя решение виртуальной машины), способы оптимизации взаимодействия пользователей со страницей продукта и дополнительного тестового интерфейса, способы создания и использования потенциальных клиентов, а также способы оптимизации взаимодействия с клиентами.

> [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4026/player]


## <a name="vm-publishing-process-flow"></a>Поток процесса публикации виртуальной машины

На следующей схеме показаны основные действия публикации предложения виртуальной машины. 

![Процесс публикации виртуальной машины](./media/publishvm_001.png)

1. Создание предложения. Предоставьте все сведения о предложении, включая его описание, маркетинговые материалы, юридическую информацию, сведения о поддержке и характеристики ресурса.

2. Создание бизнес- и технических ресурсов. Создайте бизнес-ресурсы (юридические документы и маркетинговые материалы) и технические ресурсы для связанного решения (здесь это виртуальные машины и подключенные диски). 

3. Создание номера SKU. Создайте номера SKU, связанные с предложением, и отправьте их.  Уникальный номер SKU нужен для каждого публикуемого образа. 
 
4. Сертификация и публикация предложения. Оформив предложение и выполнив технические требования, вы можете отправить предложение на публикацию. После отправки начнется процесс публикации, в ходе которого решение тестируется, проверяется, сертифицируется и активируется на Marketplace.  

## <a name="next-steps"></a>Дополнительная информация

Прежде чем следовать этим действиям, необходимо выполнить [технические и бизнес-требования](./cpp-prerequisites.md) для публикации виртуальной машины в Microsoft Azure Marketplace. 
