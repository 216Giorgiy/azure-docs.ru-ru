---
title: Примеры выражений фильтра, поддерживаемых в средстве подготовки данных службы "Машинное обучение Azure" | Документация Майкрософт
description: В этой статье приведены примеры выражений фильтра, поддерживаемых в средстве подготовки данных службы "Машинное обучение Azure".
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: a960197c89e1d051de0e9b8b73cbab231982cf52
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216743"
---
# <a name="sample-of-filter-expressions-python"></a>Пример выражений фильтра (Python) 
Перед ознакомлением с этим приложением см. [общие сведения о расширяемости Python](data-prep-python-extensibility-overview.md).

## <a name="filter-with-equivalence-test"></a>Фильтрация с помощью теста на эквивалентность
Фильтрация только в тех строках, где значение Col2 (числовое) больше 4. 

```python
    row["Col2"] > 4
```

## <a name="filter-with-multiple-columns"></a>Фильтрация по нескольким столбцам 
Фильтрация только в тех строках, где свойство Col1 имеет значение **Good**, а Col2 — значение 1 (числовое). 
```python
    row["Col1"] == 'Good' and row["Col2"] == 1
```

## <a name="test-filter-against-null"></a>Проверка фильтров на наличие значения Null
Фильтрация только в тех строках, где свойство Col1 имеет значение Null. 
```python
    pd.isnull(row["Col1"])
```
