---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: a8b4e3038bfa6a2e937de91804159e340ed13224
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554154"
---
| Ресурс | Цель |
|----------|---------------|
| Максимальный размер одной таблицы | 500 ТиБ |
| Максимальный размер сущности таблицы | 1 МиБ |
| Максимальное число свойств в сущности таблицы | 255, который включает в себя три системных свойства: PartitionKey, RowKey и Timestamp |
| Максимальное число хранимых политик доступа на таблицу | 5 |
| Максимальная частота запросов на учетную запись хранения | 20 000 транзакций в секунду, что предполагается, что размер сущностей 1 Киб |
| Целевая пропускная способность для отдельного табличного раздела (-размер сущностей 1 Киб) | До 2 000 сущностей в секунду |
