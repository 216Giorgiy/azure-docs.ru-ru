---
title: включение файла
description: включение файла
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 3/26/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: b8842ab4bcaf16b7345b25fa9ac4998981d9c458
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014595"
---
### <a name="what-is-azure-firewall"></a>Что такое брандмауэр Azure?

Брандмауэр Azure — это управляемая облачная служба сетевой безопасности, которая защищает ресурсы виртуальной сети Azure. Брандмауэр разработан как служба с полным отслеживанием состояния со встроенной высокой доступностью и неограниченной облачной масштабируемостью. Вы можете централизованно создавать, применять и регистрировать политики приложений и сетевых подключений в подписках и виртуальных сетях.

### <a name="what-capabilities-are-supported-in-azure-firewall"></a>Характеристики и функции Брандмауэра Azure:

* Брандмауэр как услуга с отслеживанием состояния.
* Встроенные функции обеспечения высокой доступности и неограниченная масштабируемость в облаке.
* фильтрацию FQDN;
* теги FQDN;
* правила фильтрации трафика;
* поддержку исходящих данных SNAT;
* Поддержка DNAT для входящего трафика
* Централизованное создание, применение и регистрация политик приложений и сетевых подключений в подписках Azure и виртуальных сетях.
* Полная интеграция с Azure Monitor для ведения журналов и аналитики.

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Что такое типичная модель развертывания для службы "Брандмауэр Azure"?

Хотя службу "Брандмауэр Azure" можно развернуть в любой виртуальной сети, клиенты обычно развертывают ее в центральной виртуальной сети и устанавливать пиринг к ней в звездообразной модели. Затем вы можете установить маршрут по умолчанию из пиринговых виртуальных сетей, чтобы указать на эту центральную виртуальную сеть брандмауэра. Глобальный пиринг поддерживается, но не рекомендуется из-за потенциальной производительности и задержек в регионах. Для наилучшей производительности разверните за одним брандмауэром на регион.

Преимуществом этой модели является возможность централизованно влиять на несколько периферийных виртуальных сетей, находящихся в разных подписках. Она также помогает сократить затраты, так как вам не нужно развертывать брандмауэр в каждой виртуальной сети отдельно. Экономию и соответствующие затраты на пиринг следует сравнивать на основе шаблонов трафика клиента.

### <a name="how-can-i-install-the-azure-firewall"></a>Как установить службу "Брандмауэр Azure"?

Службу "Брандмауэр Azure" можно настроить с помощью портала Azure, PowerShell, REST API или шаблонов. Пошаговые инструкции см. в [руководстве по развертыванию и настройке службы "Брандмауэр Azure" с помощью портала Azure](../articles/firewall/tutorial-firewall-deploy-portal.md).

### <a name="what-are-some-azure-firewall-concepts"></a>Каковы некоторые концепции службы "Брандмауэр Azure"?

Служба "Брандмауэр Azure" поддерживает правила и коллекции правил. Коллекция правил — это набор правил с одинаковым порядком и приоритетом. Коллекции правил выполняются в порядке их важности. Коллекции правил сети имеют более высокий приоритет, чем коллекции правил приложения, и выполнение всех правил прекращается.

Существует три типа коллекций правил.

* *Правила приложений*. Настройте полные доменные имена (FQDN), которые можно получить доступ из подсети.
* *Правила сети*. Настройка правил, которые содержат исходные адреса, протоколы, порты назначения и адреса назначения.
* *Правила преобразования сетевых адресов*: Настройте DNAT правила для входящих подключений.

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Поддерживает ли служба "Брандмауэр Azure" фильтрацию входящего трафика?

Брандмауэр Azure поддерживает фильтрацию входящего и исходящего трафика. Реализована защита входящего трафика, использующего протоколы, отличные от HTTP/S. Например, RDP, SSH, FTP.

### <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Какие службы ведения журнала и аналитики поддерживаются в службе "Брандмауэр Azure"?

Служба "Брандмауэр Azure" интегрирована с Azure Monitor для возможности просмотра и анализа журналов Брандмауэра. Журналы могут отправляться в Log Analytics, службу хранилища Azure или Центры событий. Их можно анализировать в Log Analytics или с помощью различных инструментов, таких как Excel и Power BI. Дополнительные сведения см. в статье [Руководство. Мониторинг журналов и метрик Брандмауэра Azure](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Как работа службы "Брандмауэр Azure" отличается от работы имеющихся на рынке служб, например виртуальных сетевых модулей?

Служба "Брандмауэр Azure" — это базовая служба брандмауэра, которая поддерживает определенные сценарии клиентов. Предполагается, что вы будете использовать сторонние виртуальные сетевые модули и брандмауэр Azure. Их совместное использование является приоритетной задачей.

### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Какова разница между WAF шлюза приложений и службой "Брандмауэр Azure"?

Брандмауэр веб-приложения (WAF) — это компонент шлюза приложений, обеспечивающий централизованную входящую защиту веб-приложений от распространенных эксплойтов и уязвимостей. Брандмауэр Azure обеспечивает защиту входящего трафика для протоколов, отличных от HTTP/S (таких как RDP, SSH, FTP), защиту исходящего трафика сетевого уровня для всех портов и протоколов и защиту уровня приложения для исходящего трафика HTTP/S.

### <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Какова разница между группами безопасности сети (NSG) и службой "Брандмауэр Azure"?

Служба "Брандмауэр Azure" дополняет функции группы безопасности сети. Вместе эти два компонента обеспечивают эшелонированную защиту сети. Группы безопасности сети предоставляют распределенную фильтрацию трафика сетевого уровня для ограничения отправки трафика на ресурсы в виртуальных сетях в каждой подписке. Служба "Брандмауэр Azure" — это полностью автономный, централизованный сетевой брандмауэр как услуга, обеспечивающий защиту на уровне сети и приложений в различных подписках и виртуальных сетях.

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Как настроить службу "Брандмауэр Azure" для использования с конечными точками служб?

Для обеспечения безопасного доступа к службам PaaS рекомендуем использовать конечные точки. Вы можете включить конечные точки служб в подсети службы "Брандмауэр Azure" и отключить их в подключенных периферийных виртуальных сетях. Таким образом, вы получаете преимущества обеих функций: безопасность конечной точки службы и централизованное ведение журналов всего трафика.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Каковы цены на службу "Брандмауэр Azure"?

В цене Брандмауэра Azure есть фиксированная и переменная составляющие.

* Фиксированная плата за использование: 1,25 долл. США на брандмауэр в час.
* Переменная плата: 0,03 долл. США за обработанный брандмауэром ГБ (входящий и исходящий сетевой трафик).

Плата за брандмауэр, для которого отменено распределение, не взимается.

Дополнительные сведения см. на странице [цен на брандмауэр Azure](https://azure.microsoft.com/pricing/details/azure-firewall/).

### <a name="how-can-i-stop-and-start-azure-firewall"></a>Как запустить и остановить Брандмауэр Azure?

Вы можете использовать методы Azure PowerShell *deallocate* и *allocate*.

Например: 

```azurepowershell
# Stop an exisitng firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzFirewall -AzureFirewall $azfw
```

```azurepowershell
#Start a firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$vnet = Get-AzVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzFirewall -AzureFirewall $azfw
```

> [!NOTE]
> Необходимо переместить брандмауэр или общедоступный IP-адрес в исходную группу ресурсов или подписку.

### <a name="what-are-the-known-service-limits"></a>Каковы известные ограничения службы?

Сведения об ограничениях службы "Брандмауэр Azure" см. в статье [Подписка Azure, границы, квоты и ограничения службы](../articles/azure-subscription-service-limits.md#azure-firewall-limits).

### <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Может ли Брандмауэр Azure, подключенный к концентратору виртуальной сети, передавать и фильтровать трафик, проходящий между двумя периферийными зонами виртуальной сети?

Да, подключенный к концентратору виртуальной сети Брандмауэр Azure может маршрутизировать и фильтровать трафик, которым обмениваются две периферийные зоны виртуальной сети. Чтобы такой сценарий был работоспособным, пользователю нужно задать маршруты к Брандмауэру Azure в качестве шлюза по умолчанию для подсетей каждой из периферийных виртуальных сетей.

### <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network-or-peered-virtual-networks"></a>Может ли Брандмауэр Azure передавать и фильтровать трафик, проходящий между подсетями одной виртуальной сети или пиринговыми виртуальными сетями?

Да. Тем не менее Настройка определяемых пользователем маршрутов для перенаправления трафика между подсетями в той же виртуальной сети требуется уделить дополнительное внимание. Хотя для определяемого пользователем маршрута достаточно использовать диапазон адресов виртуальной сети в качестве целевого префикса, при этом весь трафик с одного компьютера на другой также передается в одной и той же подсети через экземпляр Брандмауэра Azure. Чтобы избежать этого, добавьте маршрут для подсети в определяемый пользователем маршрут с типом следующего прыжка — **виртуальная сеть**. Управление этими маршрутами может быть весьма трудоемким и подвержено ошибкам. Для сегментации внутренней сети рекомендуется использовать группы безопасности сети, для которых не требуются определяемые пользователем маршруты.

### <a name="is-forced-tunnelingchaining-to-a-network-virtual-appliance-supported"></a>Принудительно туннелирование цепочки виртуальное сетевое устройство поддерживается?

Да.

Брандмауэр Azure должен иметь прямое подключение к Интернету. По умолчанию AzureFirewallSubnet имеет маршрут 0.0.0.0/0 со значением NextHopType, равным **Internet**.

Если включить Принудительное туннелирование в локальную через ExpressRoute или VPN-шлюз, может потребоваться явно настроить 0.0.0.0/0 определенный пользователем маршрут (UDR) с заданным значением NextHopType как Интернета и свяжите ее с вашей AzureFirewallSubnet. Это значение переопределяет потенциальных шлюз по умолчанию объявления BGP обратно к локальной сети. Если организации требуется Принудительное туннелирование для брандмауэра Azure для перенаправления трафика шлюза по умолчанию назад по локальной сети, обратитесь в службу поддержки. Мы можем добавить в список разрешений, сохраняется подписку, чтобы убедитесь, что требуется брандмауэр подключения к Интернету.

### <a name="are-there-any-firewall-resource-group-restrictions"></a>Существуют ли какие-либо ограничения для групп ресурсов брандмауэра?

Да. Брандмауэр, подсеть, виртуальная сеть и общедоступный IP-адрес должны относиться к одной группе ресурсов.

### <a name="when-configuring-dnat-for-inbound-network-traffic-do-i-also-need-to-configure-a-corresponding-network-rule-to-allow-that-traffic"></a>Настраивая DNAT для входящего трафика, нужно ли также настроить соответствующее правило сети для разрешения этого трафика?

№ Правила NAT позволяют неявно добавить соответствующее правило сети, чтобы разрешить преобразованный трафик. Чтобы переопределить эту реакцию, явно добавьте коллекцию правил сети с запрещающими правилами, которые соответствуют преобразованному трафику. Дополнительные сведения о логике обработки правил Брандмауэра Azure см. в [соответствующей статье](../articles/firewall/rule-processing.md).

### <a name="how-to-wildcards-work-in-an-application-rule-target-fqdn"></a>Как подстановочные знаки? в целевой объект правила приложения полное доменное имя

Если вы настраиваете ***. contoso.com**, он позволяет *anyvalue*. contoso.com, но не contoso.com (вершине домена). Если вы хотите разрешить на вершине домена, вы явным образом настроить его в качестве полного доменного ИМЕНИ целевого объекта.
