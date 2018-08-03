---
title: Управление параметрами учетной записи в LUIS | Документация Майкрософт
description: Веб-сайт службы LUIS можно использовать для управления параметрами учетной записи.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: 73e90e5ae86db2c2c4625762b285f8c86f0e241b
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398051"
---
# <a name="manage-account-and-authoring-key"></a>Управление учетной записью и ключом разработки
Чтобы использовать учетную запись LUIS необходимо иметь в наличии два таких ключевых элемента, как учетная запись пользователя и ключ разработки. Сведения для входа управляются с сайта [account.microsoft.com](https://account.microsoft.com). Ключ разработки управляется из веб-сайта [LUIS](luis-reference-regions.md) со страницы **Параметры**. 

## <a name="authoring-key"></a>Ключ разработки

Этот единый ключ разработки для конкретного региона на странице **Параметры**, который позволяет разрабатывать все приложения на веб-сайте [LUIS](luis-reference-regions.md) так же, как на веб-сайте по [разработке API](https://aka.ms/luis-authoring-api). Для удобства ключу разработки разрешается запрашивать конечные точки на протяжении месяца [ограниченное](luis-boundaries.md) количество раз. 

![Страница "Параметры LUIS"](./media/luis-how-to-account-settings/account-settings.png)

Ключ разработки используется для любых приложений, которыми вы владеете, так и для приложений, в которых вы отображены как сотрудник.

## <a name="authoring-key-regions"></a>Регионы ключа разработки
Ключ разработки соответствует [региону разработки](luis-reference-regions.md#publishing-regions). Для каждого региона существует свой уникальный ключ. 

## <a name="reset-authoring-key"></a>Сброс ключа разработки
Если ключ разработки скомпрометирован, его необходимо сбросить. На веб-сайте [LUIS](luis-reference-regions.md) можно сбросить ключ для всех приложений. При создании приложения через веб-сайт разработки API необходимо изменить значение `Ocp-Apim-Subscription-Key` на новый ключ. 

## <a name="delete-account"></a>Удаление учетной записи
Дополнительные сведения о том, какие данные удаляются при удалении учетной записи см. статью [Data storage and removal](luis-concept-data-storage.md#accounts) (Хранение и удаление данных). 

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в разделе [Keys in LUIS](luis-concept-keys.md#authoring-key) (Ключи в службе LUIS). 

