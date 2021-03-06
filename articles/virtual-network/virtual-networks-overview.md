---
title: Виртуальная сеть Azure | Документация Майкрософт
description: Узнайте об основных понятиях и функциях виртуальной сети Azure.
services: virtual-network
documentationcenter: na
author: jimdial
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2018
ms.author: jdial
ms.openlocfilehash: 9fb6aa0c2bf585862f61d7c78bd09b340ff8a3ce
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015905"
---
# <a name="what-is-azure-virtual-network"></a>Что такое виртуальная сеть Azure?

Виртуальная сеть Azure позволяет ресурсам Azure различных типов (например, виртуальным машинам Azure) обмениваться данными друг с другом, через локальные сети и через Интернет. Виртуальная сеть ограничена одним регионом, но взаимодействие между несколькими виртуальными сетями из разных регионов можно обеспечить с помощью пиринговой связи.

Виртуальная сеть Azure предоставляет описанные ниже основные возможности.

## <a name="isolation-and-segmentation"></a>Изоляция и сегментирование

Вы можете реализовать несколько виртуальных сетей в пределах [подписки](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) и [региона](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) Azure. Все виртуальные сети изолированы друг от друга. Для каждой виртуальной сети можно сделать следующее:
- Указать пользовательское пространство частных IP-адресов, в котором используются общедоступные и частные адреса (RFC 1918). Azure назначает ресурсам в виртуальной сети частный IP-адрес из определенного вами адресного пространства.
- Разделить виртуальную сеть на подсети и выделить каждой из них часть адресного пространства виртуальной сети.
- Использовать разрешение имен Azure или указать собственный DNS-сервер, который будут использовать ресурсы в виртуальной сети.

## <a name="communicate-with-the-internet"></a>Обмен данными через Интернет

По умолчанию все ресурсы в виртуальной сети могут устанавливать исходящие подключения к Интернету. Можно также установить входящее подключение к ресурсу, присвоив ему общедоступный IP-адрес, или общедоступный экземпляр Load Balancer. Общедоступный IP-адрес или общедоступную подсистему балансировки нагрузки также можно использовать для управления исходящими подключениями.  См. дополнительные сведения об [исходящих интернет-подключениях](../load-balancer/load-balancer-outbound-connections.md), [общедоступных IP-адресах](virtual-network-public-ip-address.md) и [Load Balancer](../load-balancer/load-balancer-overview.md).

>[!NOTE]
>При использовании только внутреннего экземпляра [Load Balancer (цен. категория "Стандартный")](../load-balancer/load-balancer-standard-overview.md) исходящие подключения будут недоступными, пока вы не настроите для [исходящих подключений](../load-balancer/load-balancer-outbound-connections.md) использование общедоступного IP-адреса уровня экземпляра или общедоступного экземпляра Load Balancer.

## <a name="communicate-between-azure-resources"></a>Обмен данными между ресурсами Azure

Безопасный обмен данными между ресурсами обеспечивается одним из следующих способов:

- **Через виртуальную сеть.** Вы можете развернуть в виртуальной сети виртуальные машины и несколько других типов ресурсов Azure, например среды Службы приложений Azure, Службу Azure Kubernetes и Масштабируемые наборы виртуальных машин Azure. Полный список ресурсов Azure, которые можно развернуть в виртуальной сети, см. в статье [Интеграция виртуальной сети для служб Azure](virtual-network-for-azure-services.md). 
- **Через конечную точку службы виртуальной сети.** Расширьте пространство частных адресов и возможности идентификации своей виртуальной сети до ресурсов службы Azure (например, учетные записи службы хранилища Microsoft Azure и базы данных SQL Azure), используя прямое подключение. Конечные точки служб позволяют защищать критически важные ресурсы служб Azure в пределах вашей виртуальной сети. Дополнительные сведения см. в статье [Конечные точки службы виртуальной сети](virtual-network-service-endpoints-overview.md).
 
## <a name="communicate-with-on-premises-resources"></a>Обмен данными через локальные ресурсы

Локальные компьютеры и сети можно подключить к виртуальной сети, используя любое сочетание следующих способов:

- **Виртуальная частная сеть (VPN) типа "точка — сеть".** Устанавливается между виртуальной сетью и отдельным компьютером в вашей сети. Необходимо настроить подключение для каждого компьютера, который требуется подключить к виртуальной сети. Этот тип подключения идеально подходит для новичков, не умеющих работать в Azure, или для разработчиков, так как при его использовании существующую сеть почти не нужно менять. Обмен данными между компьютером и виртуальной сетью осуществляется через Интернет с помощью зашифрованного туннеля. Дополнительные сведения см. в разделе о [VPN-подключении "точка — сеть"](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S).
- **VPN-подключение типа "сеть — сеть".** Устанавливается между локальным VPN-устройством и VPN-шлюзом Azure, развернутым в виртуальной сети. Используя такой тип соединения, авторизованные локальные ресурсы могут получить доступ к виртуальной сети. Обмен данными между локальным VPN-устройством и VPN-шлюзом Azure осуществляется через Интернет с помощью зашифрованного туннеля. Дополнительные сведения см. в разделе о [VPN-подключении "сеть — сеть"](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute.** Устанавливается между вашей сетью и Azure через участник ExpressRoute. Это подключение является закрытым. Трафик не проходит через Интернет. Дополнительные сведения см. в разделе [ExpressRoute (частное подключение)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute).

## <a name="filter-network-traffic"></a>Фильтрация сетевого трафика
Сетевой трафик между подсетями можно фильтровать следующими способами:
- **Группы безопасности.** Группы безопасности сети и приложения могут содержать несколько правил безопасности относительно входящего и исходящего трафика, что позволяет фильтровать его по исходному и конечному IP-адресу, порту и протоколу. Дополнительные сведения см. в разделах о [группах безопасности сети](security-overview.md#network-security-groups) и [группах безопасности приложений](security-overview.md#application-security-groups).
- **Виртуальные сетевые модули.** Виртуальный сетевой модуль представляет собой виртуальную машину, которая выполняет сетевую функцию (например, брандмауэр, оптимизация WAN и др.). Список доступных сетевых виртуальных модулей, которые можно развернуть в виртуальной сети, см. на странице [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

## <a name="route-network-traffic"></a>Маршрутизация сетевого трафика

По умолчанию Azure маршрутизирует трафик между подсетями, подключенными виртуальными сетями, локальными сетями и Интернетом. Чтобы переопределить маршруты по умолчанию, создаваемые в Azure, используйте следующие варианты:
- **Таблицы маршрутов.** Вы можете создать пользовательские таблицы с маршрутами, которые определяют направление передачи трафика для каждой подсети. Подробнее о [таблицах маршрутов](virtual-networks-udr-overview.md#user-defined).
- **Маршруты протокола BGP.** При подключении виртуальной сети к локальной сети с помощью VPN-шлюза или канала ExpressRoute Azure можно распространить локальные маршруты BGP в виртуальных сетях. Подробнее об использовании протокола BGP с [VPN-шлюзом Azure](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) и [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

## <a name="connect-virtual-networks"></a>Подключение виртуальных сетей

Виртуальные сети можно подключать между собой. Таким образом, ресурсы в этих виртуальных сетях могут обмениваться данными, используя пиринговую связь между этими сетями. Виртуальные сети, которые вы подключаете, могут находиться в одном или в разных регионах Azure. Дополнительные сведения см. в статье [Пиринг между виртуальными сетями](virtual-network-peering-overview.md).

## <a name="next-steps"></a>Дополнительная информация

Теперь вы получили представление о виртуальной сети Azure. Чтобы начать использовать виртуальную сеть, создайте ее, разверните в ней несколько виртуальных машин и выполните обмен данными между виртуальными машинами. Чтобы узнать, как это сделать, см. краткое руководство по [созданию виртуальной сети](quick-create-portal.md).
