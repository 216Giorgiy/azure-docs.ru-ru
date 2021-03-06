---
title: Настройка произношения с помощью служб распознавания речи
titlesuffix: Azure Cognitive Services
description: Узнайте, как настроить произношение с помощью службы распознавания речи. Используя пользовательское произношение, вы можете определить фонетическую и отображаемую форму слова или термина. Это удобно для обработки настраиваемых терминов, например имен продуктов или аббревиатур. Все, что требуется для начала работы — это файл с записями произношения (простой TXT-файл).
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: f825cf8f381a7a2974b150a74a091412b24b09bc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59791189"
---
# <a name="enable-custom-pronunciation"></a>Создание пользовательского произношения

С помощью пользовательских произношение, можно определить фонетического формы и отображения слово или термин (аббревиатура). Это удобно для обработки настраиваемых терминов, например имен продуктов или аббревиатур. Все, что требуется для начала работы — это файл с записями произношения (простой TXT-файл).

Вот как это работает. В один TXT-файл можно ввести несколько пользовательских записей произношения. Его структура выглядит следующим образом.

```
Display form <Tab> Spoken form <Newline>
```

Следующая таблица содержит несколько примеров.

| Форма отображения | Речевая форма |
|----------|-------|
| 3CPO | see three pea o |
| L8R | late are |
| CNTK | k c n t|

## <a name="requirements-for-the-spoken-form"></a>Требования к речевой форме

Речевая форма должна содержать строчные буквы. Это условие можно применять принудительно во время импорта. Также вам потребуется реализовать проверки в средстве импорта данных. Ни в речевой, ни в отображаемой форме не должно быть знаков табуляции. Тем не менее отображаемая форма может содержать запрещенные знаки (например, ~ и ^).

Каждый TXT-файл может содержать несколько записей, как показано на следующем изображении:

![Примеры произношения аббревиатур](media/stt/custom-speech-pronunciation-file.png)

Речевая форма — это фонетическая последовательность отображаемой формы. Она состоит из букв, слов или слогов. В настоящее время нет каких-либо дополнительных инструкций или набора стандартов для формулирования речевых форм.

## <a name="supported-pronunciation-characters"></a>Поддерживаемые знаки произношения

На данный момент пользовательское произношение поддерживается для английского (en-US) и немецкого (de-DE) языков. В следующей таблице показана кодировка, которая может использоваться для выражения речевой формы термина (в файле пользовательских произношений).

| Язык | Знаки |
|---------- |----------|
| Английский (en-US) | a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |
| Немецкий (de-DE) | д, ц, ь,?, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |

> [!NOTE]
> Отображаемая форма термина (в файле произношений) должна быть записана так же, как в наборе данных адаптации языка.

## <a name="requirements-for-the-display-form"></a>Требования к отображаемой форме

Форма просмотра может быть только пользовательские слова, акроним или составные слова, в которых сочетаются существующие слова.

>[!NOTE]
>Не рекомендуется использовать эту функцию, чтобы перефразировать общеупотребительные слова или изменять речевую форму. Это лучше проверка ли некоторых нестандартных слов (например, сокращения технические термины и иностранные слова) неправильно, transribed, прежде чем использовать эту функцию. Если они декодированы, добавьте их в файл пользовательских произношений. В языковой модели следует всегда использовать только отображаемую форму слова.

## <a name="requirements-for-the-file-size"></a>Требования к размеру файла
Размер TXT-файла, содержащего записи произношения, ограничен 1 МБ (1 КБ для ключей уровня "Бесплатный"). Как правило, нет необходимости передавать большие объемы данных в этом файле. Размер большинства файлов пользовательских произношений будет исчисляться килобайтами. Для кодирования TXT-файла для всех языковых стандартов следует использовать метку порядка байтов UTF-8. Для английского языкового стандарта можно также использовать ANSI.

## <a name="next-steps"></a>Дальнейшие действия
* Повысьте точность распознавания, создав [пользовательскую акустическую модель](how-to-customize-acoustic-models.md).
* Повысьте точность распознавания, создав [пользовательскую языковую модель](how-to-customize-language-model.md).
