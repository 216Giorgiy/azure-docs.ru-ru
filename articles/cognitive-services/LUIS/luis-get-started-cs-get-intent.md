---
title: Анализ текста на естественном языке в Интеллектуальной службе распознавания речи с помощью C# — Azure Cognitive Services | Документация Майкрософт
description: Из этого краткого руководства вы узнаете, как использовать общедоступное приложение LUIS для определения намерений пользователя в разговоре. Вы отправите намерение пользователя в виде текста в конечную точку прогнозирования HTTP для общедоступного приложения, используя C#. В конечной точке LUIS применяет модель общедоступного приложения, чтобы проанализировать смысл текста на естественном языке, определить общее намерение и извлечь данные, релевантные для предметной области приложения.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 676546a215bbb8964f1cb2d26ae0fb9fd2ed9289
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/24/2018
ms.locfileid: "43771658"
---
# <a name="quickstart-analyze-text-using-c"></a>Краткое руководство. Анализ текста с помощью C#

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Предварительные требования

* [выпуск Visual Studio Community 2017](https://visualstudio.microsoft.com/vs/community/);
* язык программирования C# (входит в состав выпуска Visual Studio Community 2017);
* идентификатор общедоступного приложения: df67dcdb-c37d-46af-88e1-8b97951ca1c2.


[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Получение ключа LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Анализ текста с помощью браузера

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-c"></a>Анализ текста с помощью C# 

С помощью C# выполните запрос к конечной точке прогнозирования [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) GET, чтобы получить такие же результаты, как при использовании браузера в предыдущем разделе. 

1. Создайте в Visual Studio консольное приложение. 

    ![Доступ к меню параметров пользователя LUIS](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. В обозревателе решений проекта Visual Studio выберите **Добавить ссылку**, затем на вкладке "Сборки" выберите **System.Web**.

    ![Доступ к меню параметров пользователя LUIS](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Перезапишите содержимое файла Program.cs следующим кодом:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Замените значение `YOUR_KEY` ключом LUIS.

5. Скомпилируйте и запустите консольное приложение. Отобразится тот же результат JSON, который вы видели ранее в окне браузера.

    ![Окно консоли, в котором отображается результат JSON из LUIS](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="luis-keys"></a>Ключи LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Очистка ресурсов

По окончании работы с этим кратким руководством закройте проект Visual Studio и удалите каталог проекта из файловой системы. 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Добавление высказываний и обучение на C#](luis-get-started-cs-add-utterance.md)