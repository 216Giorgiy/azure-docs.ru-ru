---
title: Совместное использование экземпляра пользовательского поиска (Пользовательский поиск Bing)
titlesuffix: Azure Cognitive Services
description: В данной статье описывается, как совместно использовать экземпляр пользовательского поиска.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: dc4ab9237929df3610d4dd53786bb98903fa5593
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57342400"
---
# <a name="share-your-custom-search-instance"></a>Совместное использование экземпляра пользовательского поиска

Вы можете разрешить совместное редактирование и тестирование своего экземпляра, предоставив к нему доступ участникам вашей команды. Вы можете предоставить доступ к своему экземпляру любому пользователю. Для этого нужен только его адрес электронной почты. Предоставление общего доступа к экземпляру:

- Войдите в службу [Пользовательский поиск](https://customsearch.ai).
- Выберите экземпляр службы.
- Щелкните значок параметров (в виде шестеренки). 
- В разделе **Share Your Instance** (Совместное использование экземпляра) введите адрес электронной почты пользователя, которому необходимо предоставить доступ к экземпляру, и нажмите кнопку **Share** (Общий доступ). 

После указания адреса электронной почты он добавляется в список **Instance shared with** (Экземпляр используется совместно с). Повторите этот процесс для каждого пользователя, которому вы хотите предоставить доступ к экземпляру. 

Для добавления адреса электронной почты в список владелец адреса не обязан иметь учетную запись пользовательского поиска. Ему необходимо будет зарегистрироваться в службе пользовательского поиска для внесения изменений в конфигурацию. После предоставления пользователю доступа к экземпляру он сможет увидеть его в своем списке экземпляров пользовательского поиска. Одновременно изменять экземпляр может только один пользователь. Если вы попытаетесь внести изменения в экземпляр, который редактируется другим пользователем, то отобразится предупреждение. Экземпляр можно использовать совместно с не более чем 10 пользователями.

## <a name="stop-sharing"></a>Отмена общего доступа

Чтобы прекратить совместное использование экземпляра с другим пользователем, удалите из списка его адрес электронной почты с помощью значка удаления. Это также приводит к удалению экземпляра из списка экземпляров пользователя.

## <a name="next-steps"></a>Дальнейшие действия

- [Настройка пользовательского автозаполнения](define-custom-suggestions.md)
