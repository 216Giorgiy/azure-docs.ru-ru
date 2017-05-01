---
title: "Рекомендации по обработке ошибок в концентраторах событий Azure | Документация Майкрософт"
description: "Обработка ошибок и повторные попытки в концентраторах событий Azure"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/09/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 200bc0828574bca72739dea5badb22da129cd896
ms.lasthandoff: 04/12/2017

---

# <a name="-event-hubs-best-practices-for-handling-errors-and-retry"></a>🔧 Рекомендации по обработке ошибок и повторным попыткам в концентраторах событий

> [!NOTE]
> 
> Текст этого раздела еще не написан! 
>
> Будем рады рассмотреть ваши предложения по оформлению и написанию этого содержимого. Вы можете отслеживать состояние, а также оставить свои предложения, используя эту [запись в GitHub](https://github.com/Azure/azure-event-hubs/issues/305).
> 
> Если вас заинтересовала эта возможность, вы можете продолжить на сайте [GitHub](https://github.com/Microsoft/azure-docs/blob/master/contributor-guide/contributor-guide-index.md).

В этой статье рассматриваются следующие темы:

- Какие существуют виды ошибок?
- Что происходит, когда отображается ошибка?
- При каких видах ошибок следует повторить попытку? При каких не следует?
- Пользовательская политика повтора

