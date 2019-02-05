---
title: Коллективная разработка с помощью Azure Dev Spaces, .NET Core и VS Code | Документация Майкрософт
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: tutorial
description: Быстрая разработка в Kubernetes с использованием контейнеров и микрослужб в Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
ms.openlocfilehash: c411a227c9cb277f6bf16df5085b1e674bd37176
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463911"
---
# <a name="team-development-with-azure-dev-spaces"></a>Коллективная разработка с помощью Azure Dev Spaces

В этом руководстве описано, как использовать несколько пространств разработки для параллельной работы в разных средах разработки, когда отдельные проекты выполняются в отдельных пространствах в пределах одного кластера.

## <a name="call-a-service-running-in-a-separate-container"></a>Вызов службы, запущенной в отдельном контейнере

В этом разделе создается вторая служба `mywebapi`, которую будет вызывать `webfrontend`. Каждая служба будет выполняться в отдельных контейнерах. Затем вы выполните отладку в обоих контейнерах.

![Несколько контейнеров](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Скачайте пример кода для *mywebapi*.
Ради экономии времени давайте загрузим образец кода из репозитория GitHub. Перейдите к https://github.com/Azure/dev-spaces и выберите **Clone or download** (Клонировать или скачать), чтобы скачать репозиторий GitHub. Код для этого раздела находится в папке `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Запуск *mywebapi*
1. Откройте папку `mywebapi` в *отдельном окне VS Code*.
1. Откройте **палитру команд** (с помощью меню **Вид | Палитра команд**), включите автоматическое завершение ввода и выберите эту команду: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Не следует путать эту команду с командой `azds prep`, которая позволяет настроить проект для развертывания.
1. Нажмите клавишу F5 и подождите, пока выполнится сборка и развертывание службы. Когда все будет готово, откроется панель отладки VS Code.
1. URL-адрес конечной точки будет выглядеть приблизительно так: http://localhost:\<portnumber\>. **Совет. В строке состояния VS Code отобразится URL-адрес, щелкнув по которому, можно перейти на соответствующий ресурс**. Кажется, что контейнер выполняется локально, но фактически он выполняется в нашей среде разработки в Azure. В адресе указано localhost, потому что для `mywebapi` не определены общедоступные конечные точки и доступ осуществляется только в пределах экземпляра Kubernetes. Для вашего удобства и упрощения взаимодействия с закрытой службой на локальном компьютере служба Azure Dev Spaces создает временный туннель SSH для контейнера, запущенного в Azure.
1. Когда `mywebapi` будет готово, откройте в браузере адрес localhost. Добавьте `/api/values` в URL-адрес, чтобы вызвать API GET по умолчанию для `ValuesController`. 
1. Если все шаги были успешными, вы должны увидеть ответ от службы `mywebapi`.

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Запрос из *webfrontend* к *mywebapi*
Давайте теперь напишем код в `webfrontend`, который отправляет запрос к `mywebapi`.
1. Перейдите в окно VS Code для `webfrontend`.
1. *Замените* код метода About:

    ```csharp
    public async Task<IActionResult> About()
    {
        ViewData["Message"] = "Hello from webfrontend";
        
        using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

        return View();
    }
    ```

В предыдущем примере код перенаправляет заголовок `azds-route-as` из входящего запроса в исходящий. Позже вы увидите, как это помогает повысить производительность при коллективной разработке.

### <a name="debug-across-multiple-services"></a>Отладка в нескольких службах
1. На этом этапе служба `mywebapi` по-прежнему должна выполняться с подключенным отладчиком. Если это не так, нажмите клавишу F5 в проекте `mywebapi`.
1. Установите точку останова в методе `Get(int id)`, который обрабатывает запросы GET `api/values/{id}`.
1. В проекте `webfrontend` установите точку останова до отправки запроса GET к `mywebapi/api/values`.
1. Нажмите клавишу F5 в проекте `webfrontend`.
1. Вызовите веб-приложение и пошагово выполните код в обеих службах.
1. В веб-приложении на странице About (Сведения) будет отображаться объединенное сообщение от двух служб: "Hello from webfrontend and Hello from mywebapi".


Все готово! Теперь у вас есть многоконтейнерное приложение, где каждый контейнер можно разрабатывать и развертывать отдельно.

## <a name="learn-about-team-development"></a>Сведения о коллективной разработке

[!INCLUDE [](../../includes/team-development-1.md)]

Давайте посмотрим на это в действии. Перейдите в окно VS Code для `mywebapi` и измените код в методе `string Get(int id)`, например:

```csharp
[HttpGet("{id}")]
public string Get(int id)
{
    return "mywebapi now says something new";
}
```


[!INCLUDE [](../../includes/team-development-2.md)]

### <a name="well-done"></a>Все готово!
Вы выполнили руководство по началу работы. Вы научились выполнять следующие задачи:

> [!div class="checklist"]
> * Настройка Azure Dev Spaces с помощью управляемого кластера Kubernetes в Azure.
> * итеративная разработка кода в контейнерах;
> * независимая разработка двух отдельных служб и вызов другой службы с помощью обнаружения службы DNS Kubernetes;
> * эффективная разработка и тестирование кода в среде командной работы.

Теперь, после ознакомления с Azure Dev Spaces, [предоставьте доступ к пространству разработки своему сотруднику](how-to/share-dev-spaces.md) и покажите ему, как просто работать вместе.

## <a name="clean-up"></a>Очистка
Чтобы полностью удалить экземпляр Azure Dev Spaces из кластера, включая все среды разработки и работающие службы, используйте команду `az aks remove-dev-spaces`. Не забывайте, что это действие необратимо. Позднее вы сможете снова добавить в кластер поддержку Azure Dev Spaces, но в этом случае вам придется начать работу с нуля. Прежние службы и среды не будут восстановлены.

С помощью приведенного ниже примера кода можно вывести список контроллеров Azure Dev Spaces в активной подписке, а затем удалить контроллер Azure Dev Spaces, связанный с кластером AKS myaks в группе ресурсов myaks-rg.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
