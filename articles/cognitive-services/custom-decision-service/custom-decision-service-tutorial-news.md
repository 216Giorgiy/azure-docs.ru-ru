---
title: Руководство. Персонализация статей — Пользовательская служба принятия решений
titlesuffix: Azure Cognitive Services
description: Руководство по персонализации статей для принятия решений на основе контекста.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: tutorial
ms.date: 05/08/2018
ms.author: slivkins
ms.openlocfilehash: d8ddafe20ff93e7ae4d51e2180bbd40447729234
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2019
ms.locfileid: "55983038"
---
# <a name="tutorial-article-personalization-for-contextual-decision-making"></a>Руководство. Персонализация статей для принятия решений на основе контекста

Это руководство посвящено персонализации выбранных статей на главной странице веб-сайта. Например, Пользовательская служба принятия решений влияет на *несколько* списков статей на главной странице. Возможно, страница — это новостной сайт, который охватывает только политику и спорт. Он будет показывать три ранжированные списка статей: политика, спорт и недавние события.

## <a name="applications-and-action-sets"></a>Приложения и наборы действий

Вот как подготовить сценарий для использования на платформе. Представим себе три приложения, по одному для каждого оптимизируемого списка: app-politics, app-sports и app-recent. Чтобы указать статьи-кандидаты для каждого приложения, есть два набора действий: один для app-politics и один для app-sports. Действие, установленное для app-recent, появляется автоматически в виде объединения двух наборов.

> [!TIP]
> Наборы действий можно разделить между приложениями в Пользовательской службе принятия решений.

## <a name="prepare-action-set-feeds"></a>Подготовка веб-каналов для набора действий

Служба пользовательских решений использует набор действий через веб-каналы RSS или Atom, предоставленные клиентом. Вы предоставляете два веб-канала: один для "политики" и один для "спорта". Предположим, что они обслуживаются с `http://www.domain.com/feeds/<feed-name>`.

Каждый канал предоставляет список статей. В RSS каждый из них определяется элементом `<item>` следующим образом:

```xml
<rss version="2.0"><channel>
   <item>
      <title><![CDATA[article title]]></title>
      <link>"article url"</link>
      <pubDate>publication date</pubDate>
    </item>
</channel></rss>
```

Порядок статей имеет значение. Он определяет ранжирование по умолчанию, которое представляет собой самый оптимальный способ упорядочивания статей. Ранжирование по умолчанию используется для сравнения производительности на панели мониторинга.

Дополнительные сведения о формате веб-канала см. в разделе [API набора действий (предоставляемый клиентом)](custom-decision-service-api-reference.md#action-set-api-customer-provided).

## <a name="register-a-new-app"></a>Регистрация нового приложения

1. Войдите с помощью [учетной записи Майкрософт](https://portal.ds.microsoft.com/). На ленте щелкните **My Portal** (Мой портал).

2. Чтобы зарегистрировать приложение, нажмите кнопку **Новое приложение**.

    ![Портал Пользовательской службы принятия решений](./media/custom-decision-service-tutorial/portal.png)

3. Введите уникальное имя для вашего приложения в текстовое поле **Идентификатор приложения**. Если это имя уже используется другим клиентом, система попросит вас выбрать другой идентификатор приложения. Установите флажок **Дополнительно** и введите [строку подключения](../../storage/common/storage-configure-connection-string.md) для учетной записи хранения Azure. Как правило, используется одна учетная запись хранения для всех приложений.

    ![Диалоговое окно нового приложения](./media/custom-decision-service-tutorial/new-app-dialog.png)

    После регистрации всех трех приложений они перечислены в приведенном выше сценарии:

    ![Список приложений](./media/custom-decision-service-tutorial/apps.png)

    Вы можете вернуться к этому списку, нажав кнопку **Приложения**.

4. В диалоговом окне **Новое приложение** укажите веб-канал действия. Веб-каналы действий можно также задать, нажав кнопку **Веб-каналы**, затем щелкнув **Новый веб-канал**. Введите **имя** нового веб-канала и **URL-адрес**, с которого он будет обслуживаться, затем введите **время обновления**. Время обновления указывает, как часто Пользовательская служба принятия решений должна обновлять веб-канал.

    ![Диалоговое окно нового веб-канала](./media/custom-decision-service-tutorial/new-feed-dialog.png)

    Веб-каналы действий могут использоваться любым приложением, независимо от того, где они указаны. После указания обоих каналов действий в сценарии они перечислены:

    ![Список веб-каналов](./media/custom-decision-service-tutorial/feeds.png)

    Вы можете вернуться к этому списку, нажав кнопку **Веб-каналы**.

## <a name="use-the-apis"></a>Использование API-интерфейсов

Пользовательская служба принятия решений ранжирует статьи через API-ранжирования. Чтобы использовать этот API, вставьте следующий код в заголовке HTML главной страницы:

```html
<!-- Define the "callback function" to render UI -->
<script> function callback(data) { … } </script>

<!--  call Ranking API after callback() is defined, separately for each app -->
<script src="https://ds.microsoft.com/api/v2/app-politics/rank/feed-politics" async></script>
<script src="https://ds.microsoft.com/api/v2/app-sports/rank/feed-sports" async></script>
<script src="https://ds.microsoft.com/api/v2/app-recent/rank/feed-politics/feed-sports" async></script>
<!-- NB: action feeds for 'app-recent' are listed one after another. -->
```

Ответ HTTP от API ранжирования — это строка в формате JSONP. Например, для app-politics строка выглядит следующим образом:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"app-politics",
   "actionSets":[{"id":"feed-politics","lastRefresh":"date"}] });
```

Затем браузер выполняет эту строку как вызов функции `callback()`. Аргумент `data` в функции `callback()` содержит идентификатор приложения и рейтинг URL-адресов для просмотра. В частности, `callback()` следует использовать `data.appId`, чтобы различать три приложения. `eventId` используется внутри Пользовательской службы принятия решений для сопоставления предоставленного ранжирования со щелчком, если такой имеется.

> [!TIP]
> `callback()` проверит каждый веб-канал действия на актуальность с помощью поля `lastRefresh`. Если данный веб-канал недостаточно актуален, `callback()` может игнорировать предоставленное ранжирование, вызвать этот веб-канал напрямую и использовать ранжирование по умолчанию, обслуживаемое веб-каналом.

Дополнительные сведения о технических характеристиках и дополнительных параметрах, предоставляемых API ранжирования, см. в статье [API](custom-decision-service-api-reference.md).

Выбранные пользователем лучшие статьи возвращаются при вызове API награждения. При получении выбранной лучшей статьи на первой странице следует вызвать следующий код:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/<eventId>',
    contentType: "application/json" })
```

Использование `appId` и `eventId` в коде обработки выбранных элементов требует некоторой осторожности. Например, можно реализовать функцию `callback()` следующим образом:

```javascript
function callback(data) {
    $.map(data.ranking, function (element) {
        //custom rendering function given content info
        render({appId:data.appId,
                article:element,
                onClick: element.id != data.rewardAction ? null :
                   function() {
                       $.ajax({
                       type: "POST",
                       url: '//ds.microsoft.com/api/v2/' + data.appId + '/reward/' + data.eventId,
                       contentType: "application/json" })}
                });
}}
```

В этом примере реализуется функция `render()` для преобразования статьи для просмотра в приложении. Эта функция вводит идентификатор приложения и статью (в формате из API ранжирования). Параметр `onClick` — это функция, которая должна вызываться из `render()` для обработки щелчка. Она проверяет, был ли выполнен щелчок в верхнем слоте. Затем она вызывает API награждения с соответствующим идентификатором приложения и идентификатором события.

## <a name="next-steps"></a>Дополнительная информация

* Обратитесь к [справочнику по API](custom-decision-service-api-reference.md), чтобы получить дополнительные сведения о предоставляемых возможностях.
