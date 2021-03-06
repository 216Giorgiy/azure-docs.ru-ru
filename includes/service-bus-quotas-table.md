---
title: включение файла
description: включение файла
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 12/13/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7add8c10fd3224b9c287ea4cc672191157f56a09
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58124397"
---
В следующей таблице перечислены сведения о квотах для обмена сообщениями служебной шины Azure. Сведения о ценах и других квотах для служебной шины, см. в разделе [служебная шина — цены](https://azure.microsoft.com/pricing/details/service-bus/).

| Имя квоты | Область | Примечания | Значение |
| --- | --- | --- | --- |
| Максимальное число пространств имен уровня Basic или Standard на подписку Azure |Пространство имен |Последующие запросы на дополнительные пространства имен Basic или "стандартный" отклоняются порталом Azure. |100|
| Максимальное число пространств имен уровня "премиум" в подписке Azure |Пространство имен |Последующие запросы на дополнительные пространства имен уровня "премиум" отклоняются порталом. |25 |
| Размер очереди или раздела |Сущность |Определяется при создании очереди или раздела. <br/><br/> Последующие входящие сообщения отклоняются, а вызывающий код получает исключение. |1, 2, 3, 4 или 5 ГБ.<br /><br />В SKU уровня "премиум" и SKU "стандартный" с [секционирование](/azure/service-bus-messaging/service-bus-partitioning) включено, максимальный размер очереди или раздела составляет 80 ГБ. |
| Число одновременных подключений для пространства имен |Пространство имен |Последующие запросы на дополнительные соединения отклоняются, а вызывающий код получает исключение. Операции REST не учитываются при параллельных соединений TCP. |NetMessaging: 1,000.<br /><br />AMQP: 5,000. |
| Количество параллельных получать запросы для сущности очереди, раздела или подписки |Сущность |Получить последующие запросы отклоняются, а вызывающий код получает исключение. Эта квота применяется к общему числу одновременных операций получения во всех подписках раздела. |5 000 |
| Количество разделов или очередей на одно пространство имен |Пространство имен |Последующие запросы на создание раздела или очереди в пространстве имен отклоняются. В результате при настройке на [портале Azure][Azure portal] создается сообщение об ошибке. При вызове из API управления в вызывающем коде возникает исключение. |1000 для уровня "базовый" или "стандартный". Общее количество разделов и очередей в пространстве имен должно быть меньше или равно 1000. <br/><br/>Для уровня "премиум", 1 000 на единицу обмена сообщениями (MU). Максимальное значение равно 4 000. |
| Число [секционированных разделов или очередей](/azure/service-bus-messaging/service-bus-partitioning) на пространство имен |Пространство имен |Последующие запросы на создание секционированного раздела или очереди в пространстве имен отклоняются. В результате при настройке на [портале Azure][Azure portal] создается сообщение об ошибке. При вызове из управления API, исключение **QuotaExceededException** поступает в вызывающем коде. |Основные и стандартных уровней: 100.<br/><br/>Секционированные сущности не поддерживаются в [уровня "премиум"](../articles/service-bus-messaging/service-bus-premium-messaging.md) уровня.<br/><br />Каждая секционированная очередь или раздел учитывается при подсчете числа квоты в 1000 сущностей на пространство имен. |
| Максимальный размер пути любой сущности обмена сообщениями: очередь или раздел |Сущность |- |260 символов. |
| Максимальный размер имени любой сущности обмена сообщениями: пространство имен, подписка, правило подписки или концентратор событий |Сущность |- |50 символов. |
| Максимальный размер [идентификатора сообщения](/dotnet/api/microsoft.azure.servicebus.message.messageid) | Сущность |- | 128 |
| Максимальный размер сообщения [идентификатор сеанса](/dotnet/api/microsoft.azure.servicebus.message.sessionid) | Сущность |- | 128 |
| Размер сообщения для сущности очереди, раздела или подписки |Сущность |Входящие сообщения, превышает эти значения, отклоняются, а вызывающий код получает исключение. |Максимальный размер сообщения — 256 КБ для [уровня "стандартный"](../articles/service-bus-messaging/service-bus-premium-messaging.md), 1 МБ для [уровня "премиум"](../articles/service-bus-messaging/service-bus-premium-messaging.md). <br /><br />Из-за системных служебных данных фактическое ограничение меньше этих значений.<br /><br />Максимальный размер заголовка — 64 КБ.<br /><br />Максимальное количество свойств заголовка в контейнере свойств: **байтов/int. MaxValue**.<br /><br />Максимальный размер свойства в контейнере свойств: без явного ограничения. Ограничен максимальным размером заголовка. |
| Размер свойства сообщения для сущности очереди, раздела или подписки |Сущность | Исключение **SerializationException** создается. |Максимальный размер свойства сообщения для каждого свойства — 32 000. Совокупный размер всех свойств не может превышать 64 000. Это ограничение относится ко всему заголовку [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), который имеет свойства пользователя и системные свойства, такие как [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [метка](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label)и [ MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid). |
| Количество подписок на раздел |Сущность |Последующие запросы на создание дополнительных подписок для раздела отклоняются. В результате, если это настроено на портале, отображается сообщение об ошибке. При вызове из API управления в вызывающем коде возникает исключение. |Уровень "стандартный": Каждая подписка учитываются квоты в 1000 сущностей, то есть, очереди, разделы и подписки на пространство имен. <br/> <br/> Уровень "премиум": 2,000. |
| Количество фильтров SQL на раздел |Сущность |Последующие запросы на создание дополнительных фильтров для раздела отклоняются, а вызывающий код получает исключение. |2 000 |
| Количество фильтров корреляции на раздел |Сущность |Последующие запросы на создание дополнительных фильтров для раздела отклоняются, а вызывающий код получает исключение. |100 000 |
| Размер фильтров SQL или действия |Пространство имен |Последующие запросы на создание дополнительных фильтров отклоняются, а вызывающий код получает исключение. |Максимальная длина строки условия фильтра — 1024 (1 КБ).<br /><br />Максимальная длина строки действия правила — 1024 (1 КБ).<br /><br />Максимальное количество выражений на действие правила — 32. |
| Количество правил [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) на одно пространство имен, очередь или раздел |Сущность, пространство имен |Последующие запросы на создание дополнительных правил будут отклонены, а вызывающий код получает исключение. |Максимальное количество правил — 12. <br /><br /> Правила, настроенные для пространства имен служебной шины, применяются ко всем очередям и разделам в этом пространстве. |
| Число сообщений на транзакцию | транзакция: | Последующие входящие сообщения отклоняются, а вызывающий код получает следующее исключение «не удается отправить более 100 сообщений в одной транзакции». | 100 <br /><br /> Для операций **Send()** и **SendAsync()**. |

[Azure portal]: https://portal.azure.com