---
title: "Масштабирование емкости Power BI Embedded | Документация Майкрософт"
description: "В этой статье содержится пошаговое руководство по масштабированию емкости Power BI Embedded в Microsoft Azure."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/19/2018
ms.author: asaxton
ms.openlocfilehash: 7eb64cce37f2655b72ab9b5fadedf7581fe007fb
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2018
---
# <a name="scale-your-power-bi-embedded-capacity"></a>Масштабирование емкости Power BI Embedded

В этой статье содержится пошаговое руководство по масштабированию емкости Power BI Embedded в Microsoft Azure. Масштабирование позволяет увеличить или уменьшить размер доступной емкости.

При этом подразумевается, что емкость Power BI Embedded уже создана. Если нет, см. статью [Создание емкости Power BI Embedded на портале Azure](create-capacity.md).

> [!NOTE]
> Операция масштабирования может длиться около минуты. В это время емкость будет недоступна. Может не получиться загрузить встроенное содержимое.

## <a name="scale-a-capacity"></a>Масштабирование емкости

1. Войдите на [портал Azure](https://portal.azure.com/).

2. Выберите **Больше служб** > **Power BI Embedded**, чтобы просмотреть доступные емкости.

    ![Раздел "Больше служб" на портале Azure](media/scale-capacity/azure-portal-more-services.png)

3. Выберите емкость, которую нужно масштабировать.

    ![Список емкостей Power BI Embedded на портале Azure](media/scale-capacity/azure-portal-capacity-list.png)

4. Выберите **Ценовая категория** в разделе **Масштаб** в пределах доступной емкости.

    ![Параметр ценовой категории в разделе масштаба](media/scale-capacity/azure-portal-scale-pricing-tier.png)

    Текущая ценовая категория выделяется синим цветом.

    ![Текущая ценовая категория, выделенная синим цветом](media/scale-capacity/azure-portal-current-tier.png)

5. Для масштабирования выберите новую категорию, в которую следует перейти. Вокруг выбираемой категории появляется штриховой синий контур. Нажмите **Выбрать** для масштабирования до новой категории.

    ![Выбор новой категории](media/scale-capacity/azure-portal-select-new-tier.png)

    Масштабирование емкости может занять одну или две минуты.

6. Подтвердите категорию, просмотрев вкладку обзора. Будет отображена текущая ценовая категория.

    ![Подтверждение текущей категории](media/scale-capacity/azure-portal-confirm-tier.png)

## <a name="next-steps"></a>Дополнительная информация

Чтобы приостановить или запустить емкость, см. статью [Приостановка и запуск емкости Power BI Embedded на портале Azure](pause-start.md).

Чтобы начать внедрение содержимого Power BI в приложение, см. статью [Внедрение панелей мониторинга, отчетов и плиток Power BI](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

У вас имеются и другие вопросы? [Задайте их в сообществе Power BI](http://community.powerbi.com/).
