---
title: 'Краткое руководство: использование пакета SDK визуального поиска Bing, C#'
titleSuffix: Azure Cognitive Services
description: Установка компонентов консольного приложения пакета SDK для визуального поиска C#.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 05/16/2018
ms.author: v-gedod
ms.openlocfilehash: 25b01de47767e335d614aa0a8cf32c344c7305d8
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442852"
---
# <a name="quickstart-bing-visual-search-sdk-c"></a>Краткое руководство: пакет SDK визуального поиска Bing для C#

Пакет SDK для визуального поиска Bing содержит функции REST API для обработки веб-запросов и анализа результатов.
На сайте GitHub доступен [исходный код примеров для пакетов поиска SDK Bing Visual C#](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVisualSearch).

Сценарии выполнения кода описаны ниже приведенного заголовка.
* [Клиент визуального поиска](#client)
* [Готовое консольное приложение](#complete)
* [Запрос POST с двоичным файлом изображения и объектом cropArea](#binary-crop)
* [Параметр KnowledgeRequest](#knowledge-req)
* [Теги, действия и тип действия](#tags-actions)
* [Число тегов, число действий, первый actionType](#num-tags-actions)

## <a name="prerequisites"></a>Предварительные требования

* Visual Studio 2017. При необходимости можно скачать бесплатную версию сообщества. Ее можно найти по адресу https://www.visualstudio.com/vs/community/.
* Для выполнения действий, описанных в этом кратком руководстве, нужно создать подписку в ценовой категории S9, как указано на странице [Цены на Cognitive Services. API-интерфейсы поиска Bing](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Чтобы создать подписку на портале Azure:
1. Введите запрос BingSearchV7 в строке поиска с текстом `Search resources, services, and docs` в верхней части страницы на портале Azure.  
2. В меню Marketplace в раскрывающемся списке выберите `Bing Search v7`.
3. В поле `Name` (Имя) введите имя нового ресурса.
4. Выберите подписку `Pay-As-You-Go` (Оплата по мере использования).
5. Выберите ценовую категорию `S9`.
6. Щелкните `Enable` (Активировать), чтобы создать подписку.

* Поддерживается возможность запуска пакета SDK для .NET Core и приложений для .NET Core 1.1. Загрузить .NET Core, Framework и среду выполнения можно по адресу https://www.microsoft.com/net/download/.

## <a name="application-dependencies"></a>Установка зависимостей

Чтобы настроить консольное приложение с помощью пакета SDK для API "Поиск в Интернете Bing", в обозревателе решений Visual Studio перейдите к параметру `Manage NuGet Packages`.  Добавьте пакет `Microsoft.Azure.CognitiveServices.Search.VisualSearch`.

При установке [пакета SDK NuGet для API "Поиск в Интернете"](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VisualSearch/1.0) также будут установлены следующие зависимости:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json.

<a name="client"></a>

## <a name="visual-search-client"></a>Клиент визуального поиска
Чтобы создать экземпляр клиента `VisualSearchAPI`, добавьте директивы using:

```csharp
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
```

Затем создайте экземпляр клиента.

```csharp
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
```

Используйте его для поиска изображений.

```csharp
 System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open;
 // The knowledgeRequest parameter is not required if an image binary is passed in the request body
 var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
```

Проанализируйте результаты предыдущего запроса.

```csharp
// Visual Search results
if (visualSearchResults.Image?.ImageInsightsToken != null)
{
    Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
}
else
{
    Console.WriteLine("Couldn't find image insights token!");
}

// List of tags
if (visualSearchResults.Tags.Count > 0)
{
    var firstTagResult = visualSearchResults.Tags.First();
    Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

    // List of actions in first tag
    if (firstTagResult.Actions.Count > 0)
    {
        var firstActionResult = firstTagResult.Actions.First();
        Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
        Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
    }
    else
    {
        Console.WriteLine("Couldn't find tag actions!");
    }
```

<a name="complete"></a> 

## <a name="complete-console-application"></a>Готовое консольное приложение

Следующее консольное приложение выполняет ранее определенный запрос и анализирует результаты.

```csharp
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using System;
using System.IO;
using System.Linq;

namespace VisualSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            String subKey = "YOUR-SUBSCRIPTION-KEY";

            VisualSearchImageBinary(subKey);
            //VisualSearchImageBinaryWithCropArea(subKey);
            //VisualSearchUrlWithFilters(subKey);
            //VisualSearchInsightsTokenWithCropArea(subKey);
            //VisualSearchUrlWithJson(subKey);

            Console.WriteLine("\r\nAny key to quit...");
            Console.ReadKey();
        }


        // This will send an image binary in the body of the post request and print out the imageInsightsToken, 
        //  the number of tags, the number of actions, and the first actionType.

        public static void VisualSearchImageBinary(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                using (System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
                {
                    // The knowledgeRequest parameter is not required if an image binary is passed in the request body
                    var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
                    Console.WriteLine("\r\nVisual search request with binary of image");

                    if (visualSearchResults == null)
                    {
                        Console.WriteLine("No visual search result data.");
                    }
                    else
                    {
                        // Visual Search results
                        if (visualSearchResults.Image?.ImageInsightsToken != null)
                        {
                            Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image insights token!");
                        }

                        // List of tags
                        if (visualSearchResults.Tags.Count > 0)
                        {
                            var firstTagResult = visualSearchResults.Tags.First();
                            Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                            // List of actions in first tag
                            if (firstTagResult.Actions.Count > 0)
                            {
                                var firstActionResult = firstTagResult.Actions.First();
                                Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                                Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tag actions!");
                            }

                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image tags!");
                        }
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}
```

В примерах поиска Bing демонстрируются различные функции пакета SDK.  Добавьте следующие функции в ранее определенный класс `VisualSrchSDK`.

<a name="binary-crop"></a>

## <a name="image-binary-post-with-croparea"></a>Запрос POST с двоичным файлом изображения и объектом cropArea

Ниже приведенный код отправляет двоичный файл изображения в тексте запроса POST, вместе с объектом cropArea.  Затем он выводит токен imageInsightsToken, количество тегов, количество действий и первый actionType.

```csharp
public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
{
    var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

    try
    {
        using (FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
        {
            // The ImageInfo struct contains a crop area specifying a region to crop in the uploaded image
            CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
            ImageInfo ImageInfo = new ImageInfo(cropArea: CropArea);
            VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);

            // The knowledgeRequest here holds additional information about the image, which is passed in in binary form
            var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: VisualSearchRequest).Result;
            Console.WriteLine("\r\nVisual search request with binary of image with knowledgeRequest of CropArea");

            if (visualSearchResults == null)
            {
                Console.WriteLine("No visual search result data.");
            }
            else
            {
                // Visual Search results
                if (visualSearchResults.Image?.ImageInsightsToken != null)
                {
                    Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                }
                else
                {
                    Console.WriteLine("Couldn't find image insights token!");
                }

                // List of tags
                if (visualSearchResults.Tags.Count > 0)
                {
                    var firstTagResult = visualSearchResults.Tags.First();
                    Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                    // List of actions in first tag
                    if (firstTagResult.Actions.Count > 0)
                    {
                        var firstActionResult = firstTagResult.Actions.First();
                        Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                        Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find tag actions!");
                    }

                }
                else
                {
                    Console.WriteLine("Couldn't find image tags!");
                }
            }
        }
    }

    catch (Exception ex)
    {
        Console.WriteLine("Encountered exception. " + ex.Message);
    }
}
```

<a name="knowledge-req"></a>

## <a name="knowledgerequest-parameter"></a>Параметр KnowledgeRequest

Приведенный ниже код отправляет URL-адрес изображения вместе с параметром `knowledgeRequest` и фильтром \"site:pinterest.com\".  Затем с помощью кода будет выведено `imageInsightsToken`, количество тегов, количество действий и первый actionType.

```csharp
public static void VisualSearchUrlWithFilters(string subscriptionKey)
{
    var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

    try
    {
        // The image can be specified via URL, in the ImageInfo object
        var ImageUrl = "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80";
        ImageInfo ImageInfo = new ImageInfo(url: ImageUrl);

        // Optional filters inside the knowledgeRequest will restrict similar products and images to certain domains
        Filters Filters = new Filters(site: "pinterest.com");
        KnowledgeRequest KnowledgeRequest = new KnowledgeRequest(filters: Filters);

        // An image binary is not necessary here, as the image is specified via URL
        VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo, knowledgeRequest: KnowledgeRequest);
        var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
        Console.WriteLine("\r\nVisual search request with url of image and knowledgeRequest based on filters");

        if (visualSearchResults == null)
        {
            Console.WriteLine("No visual search result data.");
        }
        else
        {
            // Visual Search results
            if (visualSearchResults.Image?.ImageInsightsToken != null)
            {
                Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
            }
            else
            {
                Console.WriteLine("Couldn't find image insights token!");
            }

            // List of tags
            if (visualSearchResults.Tags.Count > 0)
            {
                var firstTagResult = visualSearchResults.Tags.First();
                Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                // List of actions in first tag
                if (firstTagResult.Actions.Count > 0)
                {
                    var firstActionResult = firstTagResult.Actions.First();
                    Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                    Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                }
                else
                {
                    Console.WriteLine("Couldn't find tag actions!");
                }

            }
            else
            {
                Console.WriteLine("Couldn't find image tags!");
            }
        }
    }

    catch (Exception ex)
    {
        Console.WriteLine("Encountered exception. " + ex.Message);
    }
}
```

<a name="tags-actions"></a>

## <a name="tags-actions-and-actiontype"></a>Теги, действия и actionType

Приведенный ниже код отправляет токен аналитики изображения параметру knowledgeRequest, вместе с объектом cropArea.  Затем он выводит токен imageInsightsToken, количество тегов, количество действий и первый actionType.

```csharp
public static void VisualSearchInsightsTokenWithCropArea(string subscriptionKey)
{
    var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

    try
    {
        // The image can be specified via an insights token, in the ImageInfo object
        var ImageInsightsToken = "bcid_CA6BDBEA28D57D52E0B9D4B254F1DF0D*ccid_6J+8V1zi*thid_R.CA6BDBEA28D57D52E0B9D4B254F1DF0D";

        // An optional crop area can be passed in to define a region of interest in the image
        CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
        ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: ImageInsightsToken, cropArea: CropArea);

        // An image binary is not necessary here, as the image is specified via insights token
        VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);
        var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
        Console.WriteLine("\r\nVisual search request with ImageInsightsToken and knowledgeRequest based on imageInfo of cropArea");

        if (visualSearchResults == null)
        {
            Console.WriteLine("No visual search result data.");
        }
        else
        {
            // Visual Search results
            if (visualSearchResults.Image?.ImageInsightsToken != null)
            {
                Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
            }
            else
            {
                Console.WriteLine("Couldn't find image insights token!");
            }

            // List of tags
            if (visualSearchResults.Tags.Count > 0)
            {
                var firstTagResult = visualSearchResults.Tags.First();
                Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                // List of actions in first tag
                if (firstTagResult.Actions.Count > 0)
                {
                    var firstActionResult = firstTagResult.Actions.First();
                    Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                    Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                }
                else
                {
                    Console.WriteLine("Couldn't find tag actions!");
                }

            }
            else
            {
                Console.WriteLine("Couldn't find image tags!");
            }
        }
    }

    catch (Exception ex)
    {
        Console.WriteLine("Encountered exception. " + ex.Message);
    }
}
```

<a name="num-tags-actions"></a>

## <a name="number-of-tags-number-of-actions-and-first-actiontype"></a>Число тегов, число действий и первый actionType

Следующий код отправляет токен аналитики изображения параметру knowledgeRequest, вместе с объектом cropArea.  Затем выводится токен imageInsightsToken, количество тегов, количество действий и первый actionType.

```csharp
public static void VisualSearchUrlWithJson(string subscriptionKey)
{
    var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

    try
    {

        //The visual search request can be passed in as a JSON string
        //The image is specified via URL in the ImageInfo object, along with a crop area as shown below:
        
        //     "imageInfo": {
        //         "url": "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80",
        //     "cropArea": {
        //           "top": 0.1,
        //           "bottom": 0.5,
        //           "left": 0.1,
        //           "right": 0.9
        //         }
        //     },
        //     "knowledgeRequest": {
        //        "filters": {
        //            "site": "pinterest.com"
        //        }              
        //     }

        var VisualSearchRequestJSON = "{\"imageInfo\":{\"url\":\"https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80\",\"cropArea\":{\"top\":0.1,\"bottom\":0.5,\"left\":0.1,\"right\":0.9}},\"knowledgeRequest\":{\"filters\":{\"site\":\"pinterest.com\"}}}";

        // An image binary is not necessary here, as the image is specified by URL in JSON text
        var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequestJSON).Result;
        Console.WriteLine("\r\nVisual search request with image url specified by JSON text");

        if (visualSearchResults == null)
        {
            Console.WriteLine("No visual search result data.");
        }
        else
        {
            // Visual Search results
            if (visualSearchResults.Image?.ImageInsightsToken != null)
            {
                Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
            }
            else
            {
                Console.WriteLine("Couldn't find image insights token!");
            }

            // List of tags
            if (visualSearchResults.Tags.Count > 0)
            {
                var firstTagResult = visualSearchResults.Tags.First();
                Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                // List of actions in first tag
                if (firstTagResult.Actions.Count > 0)
                {
                    var firstActionResult = firstTagResult.Actions.First();
                    Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                    Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                }
                else
                {
                    Console.WriteLine("Couldn't find tag actions!");
                }

            }
            else
            {
                Console.WriteLine("Couldn't find image tags!");
            }
        }
    }

    catch (Exception ex)
    {
        Console.WriteLine("Encountered exception. " + ex.Message);
    }
}
```

## <a name="next-steps"></a>Дополнительная информация

[Примеры пакета SDK для .NET Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
