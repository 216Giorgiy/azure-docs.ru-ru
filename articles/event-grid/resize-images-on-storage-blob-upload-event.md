---
title: Использование службы "Сетка событий Azure" для автоматического изменения размера переданных изображений | Документация Майкрософт
description: Служба "Сетка событий Azure" может быть активирована при передаче больших двоичных объектов в службу хранилища Azure. Это можно использовать для отправки файлов изображений, которые передаются в службу хранилища Azure, в другие службы, например службу "Функции Azure", для изменения размера и других улучшений.
services: event-grid, functions
author: ggailey777
manager: jpconnoc
editor: ''
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/29/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: f08de2398174363604576874627026dcc6199ac5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2018
ms.locfileid: "53104668"
---
# <a name="tutorial-automate-resizing-uploaded-images-using-event-grid"></a>Руководство. Автоматическое изменение размера переданных изображений с помощью службы "Сетка событий"

[Сетка событий Azure](overview.md) — это служба обработки событий для облака. Служба "Сетка событий" дает возможность создавать подписки на события, порождаемые службами Azure или сторонними ресурсами.  

Это руководство — вторая часть из цикла руководств по службе хранилища. Оно дополняет [предыдущее руководство по службе хранилища][previous-tutorial] и содержит сведения о том, как добавить автоматическое бессерверное создание эскизов с помощью служб "Функции Azure" и "Сетка событий Azure". Служба "Сетка событий" позволяет службе [Функции Azure](../azure-functions/functions-overview.md) реагировать на события [хранилища BLOB-объектов Azure](../storage/blobs/storage-blobs-introduction.md) и создавать эскизы передаваемых изображений. Подписка на событие создается для события создания хранилища BLOB-объектов. При добавлении большого двоичного объекта в конкретный контейнер хранилища BLOB-объектов вызывается конечная точка функции. Данные, передаваемые посредством привязки функции из службы "Сетка событий", используются для доступа к большому двоичному объекту и создания эскизов.

Для добавления возможностей изменения размера в существующее приложение для передачи изображений можно использовать Azure CLI и портал Azure.

![Опубликованное веб-приложение в браузере Microsoft Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * создание общей учетной записи хранения Azure;
> * развертывание бессерверного кода с помощью службы "Функции Azure";
> * создание подписки на событие хранилища BLOB-объектов в службе "Сетка событий".

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством:

Необходимо изучить руководство по использованию хранилища BLOB-объектов [Передача данных изображений в облако с помощью службы хранилища Azure][previous-tutorial].

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Зарегистрируйте поставщик ресурсов службы "Сетка событий" в подписке, если вы еще не сделали это.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этим руководством вам понадобится Azure CLI 2.0.14 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Если вы не используете Cloud Shell, сначала выполните вход с помощью `az login`.

## <a name="create-an-azure-storage-account"></a>Создание учетной записи хранения Azure

Для службы "Функции Azure" требуется общая учетная запись хранения. Создайте отдельную общую учетную запись хранения в группе ресурсов с помощью команды [az storage account create](/cli/azure/storage/account#az-storage-account-create).

Имя учетной записи хранения должно содержать от 3 до 24 символов и состоять только из цифр и строчных букв. 

В следующей команде замените `<general_storage_account>` глобально уникальным именем общей учетной записи хранения везде, где встречается этот заполнитель. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Создание приложения-функции  

Для выполнения функций вам понадобится приложение-функция, предоставляющее среду для выполнения кода функции без сервера. Создайте приложение-функцию с помощью команды [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

В следующей команде замените `<function_app>` уникальным именем своего приложения-функции везде, где встречается этот заполнитель. Имя приложения-функции используется по умолчанию в качестве его домена DNS. Поэтому оно должно быть уникальным среди всех приложений в Azure. Для `<general_storage_account>` замените имя созданной общей учетной записи хранения.

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Теперь необходимо настроить приложение функцию для подключения к учетной записи хранения, созданной в предыдущем [руководстве][previous-tutorial].

## <a name="configure-the-function-app"></a>Настройка приложения-функции

Чтобы подключиться к учетной записи хранилища BLOB-объектов, функции требуется строка подключения. Код функции, который вы развернули в Azure, ищет строку подключения в параметрах приложения myblobstorage_STORAGE, а также имя контейнера эскизов в параметрах приложения myContainerName. Получите строку подключения, выполнив команду [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Задайте параметры приложения с помощью команды [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set).

В следующих командах CLI `<blob_storage_account>` — это имя учетной записи хранилища BLOB-объектов, созданной при изучении предыдущего руководства.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbnails FUNCTIONS_EXTENSION_VERSION=~2
```

Параметр `FUNCTIONS_EXTENSION_VERSION=~2` позволяет запустить приложение-функцию в версии 2 среды выполнения решения "Функции Azure".

Теперь можно развернуть проект кода функции в этом приложении-функции.

## <a name="deploy-the-function-code"></a>Развертывание кода функции 

# <a name="nettabnet"></a>[\.NET](#tab/net)

Пример скрипта C# (CSX) для изменения размера можно найти в [GitHub](https://github.com/Azure-Samples/function-image-upload-resize). Разверните этот проект кода функции в приложение-функцию с помощью команды [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config). 

В следующей команде `<function_app>` — это имя приложения-функции, созданной ранее.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
Пример функции изменения размера на Node.js можно найти в [GitHub](https://github.com/Azure-Samples/storage-blob-resize-function-node). Разверните этот проект кода функции в приложение-функцию с помощью команды [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config).

В следующей команде `<function_app>` — это имя приложения-функции, созданной ранее.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-resize-function-node
```
---

Функция изменения размера образа активируется HTTP-запросом, отправленным из службы "Сетка событий". Вы указываете этой службе, что хотите получать эти уведомления по URL-адресу функции, создав подписку на события. В этом руководстве вы подписываетесь на события, созданные большим двоичным объектом.

Данные, передаваемые в функцию из уведомления службы "Сетка событий", содержат URL-адрес большого двоичного объекта. Этот URL-адрес передается входной привязке, что позволяет получить отправленный образ из хранилища BLOB-объектов. Функция создает эскиз и записывает результирующий поток в отдельный контейнер в хранилище BLOB-объектов. 

Этот проект использует `EventGridTrigger` как тип триггера. Рекомендуется использовать триггер Сетки событий, а не универсальные триггеры HTTP. Сетка событий автоматически проверяет триггеры функций Сетки событий. При использовании универсальных триггеров HTTP вам нужно реализовать [ответ проверки](security-authentication.md#webhook-event-delivery).

Дополнительные сведения об этой функции см. в [файлах function.json и run.csx](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
Код проекта функция развертывается непосредственно из общедоступного примера репозитория. Чтобы узнать больше о параметрах развертывания службы "Функции Azure", ознакомьтесь с разделом [Непрерывное развертывание для Функций Azure](../azure-functions/functions-continuous-deployment.md).

## <a name="create-an-event-subscription"></a>Создание подписки на событие

Подписка на событие определяет, какие создаваемые поставщиком события необходимо отправлять в конкретную конечную точку. В данном случае конечная точка предоставляется функцией. Чтобы создать подписку на событие, которая отправляет уведомления в функцию, выполните следующие действия на портале Azure: 

1. На [портале Azure](https://portal.azure.com) щелкните стрелку в нижнем левом углу, чтобы развернуть все службы, введите *функции* в поле **Фильтр**, а затем щелкните **Приложения-функции**. 

    ![Переход к приложениям-функциям на портале Azure](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Разверните приложение-функцию, выберите функцию **imageresizerfunc**, а затем выберите **Добавить подписку сетки событий**.

    ![Переход к приложениям-функциям на портале Azure](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Задайте значения параметров подписки на событие, указанные в таблице.
    
    ![Создание подписки на событие из функции на портале Azure](./media/resize-images-on-storage-blob-upload-event/event-subscription-create.png)

    | Параметр      | Рекомендуемое значение  | ОПИСАНИЕ                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Тип раздела** |  учетные записи хранения; | Выберите поставщик событий учетной записи хранения. | 
    | **Подписка** | Ваша подписка Azure. | По умолчанию должна быть выбрана ваша текущая подписка Azure.   |
    | **Группа ресурсов** | myResourceGroup | Щелкните **Использовать существующую** и выберите группу ресурсов, используемую в этом руководстве.  |
    | **Ресурс** |  Учетная запись хранения больших двоичных объектов |  Выберите учетную запись хранения BLOB-объектов, созданную вами. |
    | **Типы событий** | Blob created | Снимите флажки всех типов, кроме **Blob created** (Большой двоичный объект создан). Только события типа `Microsoft.Storage.BlobCreated` будут передаваться в функцию.| 
    | **Тип подписчика** |  autogenerated |  Предварительно определен как веб-перехватчик. |
    | **Конечная точка подписчика** | autogenerated | Используйте URL-адрес конечной точки, созданный автоматически. | 
    | **Имя** | imageresizersub | Имя, которое идентифицирует новую подписку на событие. | 

4. Нажмите кнопку **Создать**, чтобы добавить подписку на событие. Создается подписка на событие, которая вызывает функцию `imageresizerfunc` при добавлении большого двоичного объекта в контейнер *images*. Эта функция позволяет изменять размер образов и добавлять их в контейнер *эскизов*.

Теперь, когда внутренние службы настроены, следует проверить функциональные возможности изменения размера изображений в примере веб-приложения. 

## <a name="test-the-sample-app"></a>Поверка примера приложения

Чтобы проверить изменение размера изображений в веб-приложении, перейдите на URL-адрес опубликованного приложения. URL-адрес приложения по умолчанию: `https://<web_app>.azurewebsites.net`.

Щелкните область **Upload photos** (Передача фотографий), чтобы выбрать и передать файл. Можно также перетащить фотографию в эту область. 

Обратите внимание, что после того, как переданное изображение исчезнет, его копия появится в карусели **Generated thumbnails** (Созданные эскизы). С помощью функции размер этого образа был изменен, после чего он был добавлен в контейнер *эскизов* и скачан веб-клиентом.

![Опубликованное веб-приложение в браузере Microsoft Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * создание общей учетной записи хранения Azure;
> * развертывание бессерверного кода с помощью службы "Функции Azure";
> * создание подписки на событие хранилища BLOB-объектов в службе "Сетка событий".

Перейдите к третьей части цикла руководств по службе хранилища, чтобы узнать, как защитить доступ к учетной записи хранения.

> [!div class="nextstepaction"]
> [Безопасный доступ к данным приложения в облаке](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

+ Чтобы узнать больше о службе "Сетка событий", ознакомьтесь с разделом [Общие сведения о службе "Сетка событий Azure"](overview.md). 
+ Чтобы ознакомиться с еще одним руководством по возможностям службы "Функции Azure", прочитайте в статью [Создание функции, интегрируемой с Azure Logic Apps](../azure-functions/functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md
