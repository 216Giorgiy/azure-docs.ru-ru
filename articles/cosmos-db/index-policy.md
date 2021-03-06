---
title: Политики индексирования в Azure Cosmos DB
description: Узнайте, как настроить и изменить политику для автоматического индексирования и повышения производительности в Azure Cosmos DB индексирования по умолчанию.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: thweiss
ms.openlocfilehash: 67bc3076be91ade140b39b7dd8037299902546a9
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "60005100"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Политики индексирования Azure Cosmos DB

В Azure Cosmos DB каждый контейнер имеет политику индексации, который определяет, каким образом должен быть индексирован элементов контейнера. По умолчанию политика для индексации только что созданы индексы контейнеров каждого свойства каждого элемента, применение диапазонные индексы для любого строка или число, и Пространственные индексы для любого объекта GeoJSON введите точку. Это позволяет получить высокую производительность запросов, не задумываясь о индексирования и Управление индексами заранее.

В некоторых ситуациях может потребоваться переопределить это автоматическое поведение, чтобы лучше соответствует вашим требованиям. Можно настроить политику индексации контейнера, задав его *режим индексирования*и включить или исключить *пути к свойствам*.

## <a name="indexing-mode"></a>Режим индексирования

Azure Cosmos DB поддерживает два режима индексирования:

- **Consistent** (Согласованный). Если политика индексирования контейнера имеет значение Consistent, индекс обновляется синхронно, как создавать, обновлять или удалять элементы. Это означает, что согласованность чтения запросов [согласованности, настроенного в учетной записи](consistency-levels.md).

- **Нет** — Если политика индексирования контейнера имеет значение None, индексирование эффективно отключено на этом контейнере. Обычно это используется, когда контейнер используется как чистые хранилище значений ключей без необходимости для вторичных индексов. Она также помогает ускорить массовой операции вставки.

## <a name="including-and-excluding-property-paths"></a>Включение и исключение пути к свойствам

Пользовательскую политику индексирования можно указать пути к свойствам, которые явно включаются или исключаются из индексирования. Оптимизировав число путей, которые индексируются, можно снизить объем хранилища, используемого контейнера и Уменьшение задержки операций записи. Эти пути определяются следующей [метод, описанный в разделе Обзор индексирования](index-overview.md#from-trees-to-property-paths) со следующими дополнениями:

- заканчивается пути, ведущем к скалярное значение (строка или число) `/?`
- элементы из массива адресуются друг с другом с помощью `/[]` нотации (а не `/0`, `/1` и т.д.)
- `/*` подстановочный знак можно использовать для сопоставления любых элементов под узлом

Используя тот же пример еще раз:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 }
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

- `headquarters`в `employees` путь `/headquarters/employees/?`
- `locations`" `country` путь `/locations/[]/country/?`
- путь к все `headquarters` — `/headquarters/*`

Если путь явно включить в политику индексирования, компания должна определить, какие типы индексов должны применяться к этому пути и для каждого типа индекса, тип данных, к которым применяется этот индекс:

| Тип индекса | Допустимые типы целевых данных |
| --- | --- |
| Диапазонный индекс | Строка или число |
| пространственный индекс | Point, LineString и Polygon |

Например, мы включаем `/headquarters/employees/?` пути и указать, что `Range` индекс в той же папке должны применяться для обоих `String` и `Number` значения.

### <a name="includeexclude-strategy"></a>Включение и исключение стратегии

Все политики индексирования должно включать путь к корневому каталогу `/*` как включенные или исключенные пути.

- Включите корневой путь, чтобы выборочно исключить пути, которые не должны быть проиндексированы. Это рекомендуемый подход, так как позволяет Azure Cosmos DB заранее индекса любое новое свойство, которая может быть добавлена в модель.
- Исключите корневой путь выборочно включать пути, которые должны быть проиндексированы.

См. в разделе [в этом разделе](how-to-manage-indexing-policy.md#indexing-policy-examples) для индексирования примеры политик.

## <a name="modifying-the-indexing-policy"></a>Изменение политики индексирования

Политики индексирования для контейнера, можно обновить в любое время [с помощью портала Azure или один из поддерживаемых пакетов SDK](how-to-manage-indexing-policy.md). Обновление в политику индексирования приводит к запуску преобразования от старого индекса в новую, которая выполняется через Интернет и на месте (поэтому пространство не дополнительное хранилище использовалось во время операции). Старая политика индекс эффективно преобразуются в новую политику без ущерба доступности для записи или пропускная способность для контейнера. Преобразование индекса является асинхронной операцией, и время, необходимое для завершения зависит от того, подготовленной пропускной способности, количество элементов и их размер. 

> [!NOTE]
> Повторная индексация во время выполнения, запросы могут не возвращать все подходящие результаты и сделает это, не возвращая все ошибки. Это означает, что результаты запроса могут быть не согласованы до завершения преобразования индекса. Можно отслеживать ход выполнения преобразования индекса [с помощью одного из пакетов SDK](how-to-manage-indexing-policy.md).

Если новые политики индексирования в режим Consistent, нет других изменение политики индексирования может применяться во время выполнения преобразования индекса. Выполнение преобразования индекса можно отменить, установив режим политики индексирования None (который будет немедленно удалить индекс).

## <a name="indexing-policies-and-ttl"></a>Политики индексирования и срок ЖИЗНИ

[Время жизни (TTL) функция](time-to-live.md) требуется индексирование должна быть активной в контейнере, он включен. Это означает следующее:

- Невозможно активировать срок ЖИЗНИ в контейнере, устанавливаемый режим индексирования None,
- Выберите режим отложенного индексирования нет в контейнере где активируется TTL невозможна.

Для сценариев, не путь к свойству нужно индексировать, когда TTL является обязательным можно использовать политику индексации с:

- режим индексирования, значение Consistent, и
- путь не включен, и
- `/*` как только Исключенные пути.

## <a name="obsolete-attributes"></a>Устаревшие атрибуты

При работе с политик индексирования, могут возникнуть следующие атрибуты, которые теперь являются устаревшими:

- `automatic` Логическое значение определяется в корне политику индексации. Он теперь учитывается и может быть присвоено `true`, если это необходимо для средства, вы используете.
- `precision` число, определенные на уровне индексов для включенного пути. Он теперь учитывается и может быть присвоено `-1`, если это необходимо для средства, вы используете.
- `hash` — Это вид индекса, который теперь заменен тип диапазона.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об индексировании см. по следующим ссылкам:

- [Обзор индексации](index-overview.md)
- [Управление политикой индексирования](how-to-manage-indexing-policy.md)
