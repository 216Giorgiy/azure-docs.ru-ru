---
title: Общие сведения о проверке IP-потока в Наблюдателе за сетями Azure | Документация Майкрософт
description: Эта страница содержит обзор функции проверки IP-потока в Наблюдателе за сетями.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: jdial
ms.openlocfilehash: 88cb7e2cd04d13ade5c581a1ff2dc09669d89ab2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838010"
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Общие сведения о проверке IP-потока в Наблюдателе за сетями Azure

Функция проверки IP-потока позволяет определить, разрешена или запрещена передача пакета на виртуальную машину или с нее. Эта информация включает в себя направление, протокол, локальный и удаленный IP-адрес, локальный и удаленный порт. Если пакет отклонен группой безопасности, возвращается имя правила, которое запретило этот пакет. Хотя может быть выбран любой исходный или конечный IP-адрес, проверка IP-потока помогает администраторам быстро диагностировать проблемы с входящим или исходящим подключением к Интернету и входящим или исходящим подключением к локальной среде.

Функция проверки IP-потока проверяет правила для всех групп безопасности сети (NSG), которые применяются к сетевому интерфейсу, например подсети или сетевому адаптеру виртуальной машины. После чего на основе настроенных параметров проверяется входящий или исходящий поток трафика этого сетевого интерфейса. Проверку IP-потока удобно использовать, чтобы проверить, блокирует ли правило в группе безопасности сети входящий или исходящий трафик виртуальной машины.

Во всех регионах, в которых планируется выполнять проверку IP-потока, нужно создать экземпляр Наблюдателя за сетями. Наблюдатель за сетями — это региональная служба, которая может выполняться только для ресурсов в одном регионе. Используемый экземпляр не влияет на результаты проверки IP-потока, так как маршрут, связанный с сетевым адаптером или подсетью, по-прежнему возвращается.

![1][1]

## <a name="next-steps"></a>Дополнительная информация

Ознакомьтесь с приведенной статьей, чтобы узнать, как с помощью портала определить, разрешен или запрещен пакет для конкретной виртуальной машины. [Проверка состояния входящего и исходящего трафика виртуальной машины (разрешен или запрещен) путем проверки IP-потока (компонент Наблюдателя за сетями Azure)](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png

