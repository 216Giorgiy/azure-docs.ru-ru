---
title: Обновление Центра Интернета вещей Azure | Документация Майкрософт
description: Измените цены и уровень масштабирования для Центра Интернета вещей, чтобы получить дополнительные возможности обмена сообщениями и управления устройством.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: robinsh
ms.openlocfilehash: 96c3a7b2cfda23f173f4caeff4fb7a92b1ddc438
ms.sourcegitcommit: e89b9a75e3710559a9d2c705801c306c4e3de16c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/15/2019
ms.locfileid: "59571283"
---
# <a name="how-to-upgrade-your-iot-hub"></a>Как обновить Центр Интернета вещей

При изменении вашего решения Центр Интернета вещей Azure поможет увеличить масштаб. Центр Интернета вещей предлагает клиентам, которые хотят использовать различные возможности, два уровня: "Базовый" (B) и "Стандартный" (S). В рамках каждого уровня доступны три размера (1, 2 и 3), определяющие количество сообщений, которые можно отправлять каждый день.

Если у вас несколько устройств и необходимы дополнительные функции, вы можете настроить Центр Интернета вещей в соответствии с потребностями тремя способами.

* Добавьте единицы в Центр Интернета вещей. Например, каждая дополнительная единица в Центре Интернета вещей B1 позволяет отправлять дополнительные 400 000 сообщений в день.

* Измените размер Центра Интернета вещей. Например миграцию с уровня B1 на уровень В2, чтобы увеличить количество сообщений, которые поддерживает каждая единица в день.

* Перейдите на более высокий уровень. Например обновите уровень B1 на уровень S1, для доступа к расширенным функциям с емкостью, обмена сообщениями.

Все эти изменения можно вносить, не прерывая выполняющиеся операции.

Если вы хотите понизить центра Интернета вещей, можно удалить единицы и уменьшить размер центра Интернета вещей, но нельзя понизить до более низкого уровня. Например, можно перейти с уровня S2 на уровень S1, но не с уровня S2 на уровень B1. Только один тип [выпуск центра Интернета вещей](https://azure.microsoft.com/pricing/details/iot-hub/) в пределах уровня можно выбрать для центра Интернета вещей. Например, можно создать центр Интернета вещей с несколькими единицами уровня S1, но не с комбинацией единиц из разных выпусков, например S1 и B3 или S1 и S2.

Эти примеры помогут понять, как настраивать Центр Интернета вещей при изменениях решения. Сведения о возможностях каждого уровня должны всегда относятся к [ценами на центр Интернета вещей Azure](https://azure.microsoft.com/pricing/details/iot-hub/).

## <a name="upgrade-your-existing-iot-hub"></a>Обновление имеющегося Центра Интернета вещей

1. Войдите на [портал Azure](https://portal.azure.com/) и перейдите к своему Центру Интернета вещей.

2. Выберите **Цены и масштабирование**.

   ![Цены и масштабирование](./media/iot-hub-upgrade/pricing-scale.png)

3. Чтобы изменить уровень центра, выберите **Ценовая категория и категория масштабирования**. Выберите новый уровень, а затем щелкните **Выбрать**.

   ![Ценовая категория и категория масштабирования](./media/iot-hub-upgrade/select-tier.png)

4. Чтобы изменить количество единиц в центре, введите новое значение в поле **Единицы центра IoT Hub**.

5. Выберите **Сохранить**, чтобы сохранить изменения.

Теперь Центр Интернета вещей настроен, а конфигурации не изменяются.

Максимальный предел для уровня "базовый" центра Интернета вещей "и" стандартный "центра Интернета вещей — 32. Для большинства центров Интернета вещей достаточно четырех секций. Ограничение на количество секций устанавливается при создании центра Интернета вещей. Оно привязывает сообщения, отправляемые с устройства в облако, к числу одновременно работающих модулей чтения этих сообщений. Это значение сохраняется при переходе с уровня "Базовый" на уровень "Стандартный".

## <a name="next-steps"></a>Дальнейшие действия

[Масштабирование решения для Центра Интернета вещей](iot-hub-scaling.md).