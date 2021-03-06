---
title: Создание искусственного ключа секции в Azure Cosmos DB для равномерного распределения данных и рабочей нагрузки.
description: Сведения об использовании искусственных ключей секций в контейнерах Azure Cosmos
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.openlocfilehash: 6b3499c36ae7abd03c4a1f1ca3a3a46e2c315120
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59792104"
---
# <a name="create-a-synthetic-partition-key"></a>Создание искусственного ключа секции

Рекомендуется иметь ключ секции с большим количеством уникальных значений, таких как сотен или тысяч. Главное — это равномерное распределение данных и рабочей нагрузки по всем элементам, связанным с этими значениями ключа секции. Если такое свойство не существует в данных, можно создать *ключ секции искусственных*. В этом документе описываются некоторые основные методы для создания ключа искусственных секции для вашего контейнера Cosmos.

## <a name="concatenate-multiple-properties-of-an-item"></a>Сцепление нескольких свойств элемента

Вы можете создать ключ секции, объединив несколько значений свойств в одно искусственное свойство `partitionKey`. Эти ключи называются искусственными ключами. Рассмотрим следующий пример документа:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Для предыдущего документа одним из вариантов является установка /deviceId или /date в качестве ключа секции. Используйте этот параметр, чтобы секционировать контейнера на основе идентификатора устройства или даты. Другой вариант заключается в сцеплении этих двух значений в искусственное свойство `partitionKey`, которое используется в качестве ключа секции.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

В сценариях, в режиме реального времени может иметь тысячи элементов в базе данных. Вместо добавления синтетических ключ вручную, определите логику на стороне клиента для сцепления значений и вставки искусственных ключ в элементы в контейнеров Cosmos.

## <a name="use-a-partition-key-with-a-random-suffix"></a>Использование ключа секции со случайным суффиксом

Другой возможной стратегией равномерного распределения рабочей нагрузки является добавление случайного числа в конце значения ключа секции. При таком распределении элементов вы можете выполнять параллельные операции записи по секциям.

Например, если ключ секции представляет собой дату, Можно выбрать случайное число от 1 до 400 и СЦЕПИТЬ его в качестве суффикса к дате. Этот метод приводит к значениям ключа секции, например `2018-08-09.1`,`2018-08-09.2`, и т. д., через `2018-08-09.400`. Так как вы рандомизируете ключ секции, ежедневные операции записи в контейнере распределяются равномерно по нескольким секциям. Этот метод обеспечивает оптимальный параллелизм и высокую общую пропускную способность.

## <a name="use-a-partition-key-with-pre-calculated-suffixes"></a>Использовать ключ секции с предварительно вычисленными суффиксы 

Стратегия случайный суффикс может значительно повысить пропускную способность записи, но это трудно читать конкретного элемента. Вы не знаете значение суффикса, который использовался при написал элемента. Чтобы сделать его более удобным для чтения отдельных элементов, используйте стратегии предварительно вычисленные суффиксы. Вместо использования случайное число, для распределения элементов по разделам, используйте число, которое вычисляется на основе на то, что вам нужно создать запрос.

Рассмотрим предыдущий пример, где контейнер использует дату в качестве ключа секции. Теперь предположим, что каждый элемент имеет `Vehicle-Identification-Number` (`VIN`) атрибут, который необходимо получить доступ. Кроме того, предположим, что часто запускать запросы для поиска элементов с `VIN`, помимо даты. Прежде чем приложение запишет элемент в контейнер, оно может вычислить суффикс хэша на основе номера ТС и добавить его к дате ключа секции. Вычисление может сформировать число от 1 до 400, распределяется равномерно. Этот результат аналогичен результаты с помощью метода стратегии случайный суффикс. Значением ключа секции будет дата, сцепленная с вычисленным результатом.

С помощью этой стратегии записи равномерно распределяются по значениям ключа секции и по секциям. Позволяет отследить конкретного элемента и даты, потому что можно вычислить значение ключа секции для конкретного `Vehicle-Identification-Number`. Преимущество этого метода — что можно избежать создания ключа одной секции на "Горячий", т. е. ключ секции, который принимает все рабочей нагрузки. 

## <a name="next-steps"></a>Дальнейшие действия

Подробнее о концепции разделения вы можете узнать в следующих статьях:

* Подробнее о [логических секциях](partition-data.md).
* Подробнее о [подготовке пропускной способности в контейнерах и базах данных Azure Cosmos](set-throughput.md).
* Подробнее о [подготовке пропускной способности для контейнера Azure Cosmos](how-to-provision-container-throughput.md).
* Подробнее о [подготовке пропускной способности для базы данных Azure Cosmos](how-to-provision-database-throughput.md).
