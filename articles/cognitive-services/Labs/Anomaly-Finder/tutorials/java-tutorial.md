---
title: Руководство. Обнаружение аномалий, Java
titlesuffix: Azure Cognitive Services
description: Изучите приложение Java, в котором используется API обнаружения аномалий. Отправьте исходные точки данных в API и получите ожидаемое значение и точки аномалий.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 8f087e07bbf5033698b18b4349c4d13e862a1555
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223346"
---
# <a name="tutorial-anomaly-detection-with-java-application"></a>Руководство. Обнаружение аномалий с использованием приложения Java

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

В этой статье демонстрируется использование простого приложения Java для вызова API обнаружения аномалий.  
В примере выполняется отправка данных временных рядов в API обнаружения аномалий с ключом подписки и последующее получение всех точек аномалий и ожидаемого значения для каждой точки данных из API-интерфейса.

## <a name="prerequisites"></a>Предварительные требования

### <a name="platform-requirements"></a>Требования платформы

Это руководство было разработано с помощью [IntelliJ IDEA](https://www.jetbrains.com/idea). Также потребуется установить пакет [Java Development Kit (JDK)](https://aka.ms/azure-jdks) версии 1.8+ и последнюю версию средства сборки [Apache Maven](http://maven.apache.org/).

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Подписка на службу обнаружения аномалий и получение ключа подписки 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Скачивание учебного проекта

1. Перейдите в [репозиторий Java](https://github.com/MicrosoftAnomalyDetection/java-sample) службы MicrosoftAnomalyDetection.
2. Нажмите кнопку Clone (Клонировать) или Download (Скачать).
3. Нажмите кнопку Download ZIP (Скачать ZIP) для скачивания ZIP-файла учебного проекта.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Открытие учебного проекта

1. Распакуйте ZIP-файл учебного проекта.
2. В среде IntelliJ IDEA выберите **File > Open** (Файл > Открыть). Появится диалоговое окно Open File or Project (Открытие файла или проекта).
3. Выберите путь к корневому каталогу извлеченного проекта и нажмите кнопку ОК.
4. На панели Projects (Проекты) разверните узел **src > main > java**.
5. Дважды щелкните com.microsoft.cognitiveservice.anomalydetection.Main.java, чтобы загрузить файл в редактор.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>Замена subscriptionKey и региона в URI

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Сборка и запуск учебного проекта

1. Вызовите меню, щелкнув правой кнопкой мыши в любом месте вкладки исходного кода com.microsoft.cognitiveservice.anomalydetection.Main.java. 
2. Выберите Run 'Main.main()' (Запустить Main.main())
3. Результат примера запроса будет возвращен и показан в терминале.

### <a name="result-of-the-tutorial-project"></a>Результат учебного проекта

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Справочник по REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
