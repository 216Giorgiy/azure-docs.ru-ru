---
title: Создание и просмотр оповещений журнала, а также управление ими с помощью Azure Monitor
description: Использование Azure Monitor для авторизации, просмотра и управления правилами генерации оповещений журнала в Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: be86e961d04b600f112a173c041b60cbe50ea00d
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725068"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Создание и просмотр оповещений журнала, а также управление ими с помощью Azure Monitor  

## <a name="overview"></a>Обзор
В этой статье показано, как настроить оповещения журнала с помощью интерфейса оповещений на портале Azure. Определение правила оповещений состоит из трех частей:
- Цель: определенный ресурс Azure, который следует отслеживать.
- Критерии: определенное условие или логика в сигнале, которые должны вызывать действие.
- Действие: конкретный вызов, отправленный получателю уведомления — электронное сообщение, текстовое сообщение, веб-перехватчик и т. д.

Термин **оповещения журнала**, который используется для описания оповещений, где сигналом является пользовательский запрос, основан на [Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) или [Application Insights](../../azure-monitor/app/analytics.md). Дополнительные сведения о функциях, терминологии и типах см. в статье [Оповещения журнала в Azure Monitor. Интерфейс оповещений](../../azure-monitor/platform/alerts-unified-log.md).

> [!NOTE]
> Распространенные данные журнала из [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) теперь также доступны на платформе метрик в Azure Monitor. Более подробную информацию см. в статье [Create Metric Alerts for Logs in Azure Monitor](../../azure-monitor/platform/alerts-metric-logs.md) (Создание оповещений метрик для журналов в Azure Monitor).

## <a name="managing-log-alerts-from-the-azure-portal"></a>Управление оповещениями журнала на портале Azure

Ниже приводится пошаговое руководство по использованию оповещений журнала с помощью интерфейса портала Azure.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Создание правила генерации оповещений журнала с помощью портала Azure
1. На [портале](https://portal.azure.com/) выберите **Мониторинг**, а затем в разделе "Мониторинг" выберите **Оповещения**.  
    ![Мониторинг](media/alerts-log/AlertsPreviewMenu.png)

1. Нажмите кнопку **Новое правило генерации оповещений**, чтобы создать оповещение в Azure.
    ![Добавление оповещения](media/alerts-log/AlertsPreviewOption.png)

1. Появится раздел "Создать оповещение" с тремя элементами: *Определение условия оповещения*, *Определение сведений об оповещении* и *Определение группы действий*.

    ![Создание правила](media/alerts-log/AlertsPreviewAdd.png)

1.  Определите условие оповещения, для этого используйте ссылку **Выбор ресурса** и укажите цель путем выбора ресурса. Отфильтруйте, выбрав _Подписку_, _Тип ресурса_, и нужный _Ресурс_. 

    >[!NOTE]

    > Прежде чем продолжить, убедитесь, что сигнал **журнала** доступен для выбранного ресурса, чтобы создать оповещения журнала.
    ![Выбор ресурса](media/alerts-log/Alert-SelectResourceLog.png)

 
1. *Оповещения журналов.* В качестве **типа ресурса** выберите источник данных аналитики, например *Log Analytics* или *Application Insights*, а в качестве типа сигнала – **Журнал**. Выбрав соответствующий **ресурс**, нажмите кнопку *Готово*. Затем нажмите кнопку **Добавить условие**, чтобы просмотреть список доступных параметров сигнала для ресурса. В списке сигналов выберите **Custom log search** (Пользовательский поиск по журналам) для выбранной службы мониторинга журналов, например *Log Analytics* или *Application Insights*.

   ![Выбор ресурса: пользовательский поиск по журналам](media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Списки интерфейса оповещений могут импортировать запрос аналитических данных как тип сигнала — **Log (Saved Query)** (Журнал (сохраненный запрос)), как показано на приведенном выше изображении. Таким образом, пользователи могут усовершенствовать ваш запрос в Analytics, а затем сохранить его для дальнейшего использования в оповещениях. Дополнительные сведения о сохранении запросов см. в статьях об [использовании поиска по журналам в Log Analytics](../../azure-monitor/log-query/log-query-overview.md) или [совместном использовании запроса аналитических данных Application Insights](../../azure-monitor/log-query/log-query-overview.md). 

1.  *Оповещения журналов.* После выбора запрос для оповещения можно указать в поле **Search Query** (Поисковой запрос). Если синтаксис запроса неверен, в поле отображается ошибка, выделенная красным цветом. Если синтаксис запроса верен, появятся исторические данные указанного запроса для справки в виде графика с возможностью настройки временного интервала от последних шести часов до последней недели.

 ![Настройка правил оповещений](media/alerts-log/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Визуализация исторических данных может отображаться, только если в результатах запроса есть данные о времени. Если при выполнении запроса появляются обобщенные данные или значения конкретных столбцов, то они отображаются как отдельный график.

    >  Для оповещений журнала типа "Измерения метрик" с использованием Application Insights вы можете указать конкретную переменную для группирования данных, используя параметр **Агрегация по**, как показано ниже:

    ![параметр "Агрегация по"](media/alerts-log/aggregate-on.png)

1.  *Оповещения журналов.* После настройки визуализации можно выбрать **логику оповещений** из показанных параметров "Условие", "Агрегирование" и "Пороговое значение". В логике укажите время для оценки заданного условия с помощью параметра **Period** (Период). Кроме того, укажите частоту запуска оповещений, выбрав параметр **Frequency** (Частота).
**Оповещения журналов** могут создаваться на основе таких данных:
   - *Число записей.* Оповещение создается, если количество записей, возвращаемых запросом, больше или меньше указанного вами значения.
   - *Измерение метрик.* Оповещение создается, если каждое *статистическое значение* в результатах превышает указанное пороговое значение, которое *группируется* в соответствии с выбранным значением. Количество нарушений для оповещения — это количество превышений порогового значения в течение выбранного периода времени. Можно указать значение свойства Всего брешей, чтобы учитывать любое сочетание нарушений в наборе результатов, или значение свойства Последовательные бреши, чтобы учитывать только нарушения, следующие подряд. Дополнительные сведения см. в статье [Log alerts in Azure Monitor — Alerts (Preview)](../../azure-monitor/platform/alerts-unified-log.md) (Журнал оповещений в Azure Monitor ("Оповещения (предварительная версия)")).


1. В качестве второго шага в параметрах задайте имя оповещения в поле **Имя правила генерации оповещений**, а также введите подробные сведения в поле **Описание** и **Severity** (Серьезность). Эти данные повторно используются во всех оповещениях по электронной почте, уведомлениях или push-уведомлениях Azure Monitor. Кроме того, пользователь может сразу же активировать правило оповещения при создании, переключив параметр **Включить правило при создании**.

    Для параметра **Оповещения журналов** доступны только некоторые дополнительные функции в разделе "Детали оповещения":

    - **Отключение оповещений.** При включении подавления для правила генерации оповещений действия для этого правила отключаются на заданный промежуток времени после создания оповещения. Правило по прежнему выполняется и создает записи оповещений при соблюдении критериев. Это дает вам время устранить проблему без запуска повторяющихся действий.

        ![Отключение оповещений в оповещениях журналов](media/alerts-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Укажите значение предупреждения, превышающее частоту предупреждения, чтобы гарантировать, что уведомления прекращаются без перекрытия

1. В качестве третьего и последнего шага укажите, нужно ли запускать **группу действий** для правила оповещения, если выполняется условие. Вы можете выбрать любую имеющуюся группу действий с оповещением или создать новую. Согласно выбранной группе действий при срабатывании оповещения Azure отправит электронное письмо, текстовое сообщение, вызовет веб-перехватчик, устранит проблему с помощью модуля Runbook Azure, отправит push-уведомление в инструмент ITSM и т. д. Дополнительные сведения о группах действий см. в статье [Create and manage action groups in the Azure portal](action-groups.md) (Создание групп действий и управление ими на портале Azure).

    > [!NOTE]
    > Дополнительные сведения об ограничениях на полезные данные модуля Runbook, запускаемые для оповещений журнала через группы действий Azure, см. в статье [Подписка Azure, границы, квоты и ограничения службы](../../azure-subscription-service-limits.md).

    Для **оповещений журналов** доступны дополнительные функции для переопределения действий по умолчанию:

    - **Уведомление по электронной почте.** Переопределяет *тему в письме*, отправленном через группу действия, если в указанной группе существует одно или несколько действий электронного сообщения. Основной текст сообщения нельзя изменить, и в этом поле вы **не можете** указать адрес электронной почты.
    - **Включить настраиваемые полезные данные JSON.** Переопределяет JSON веб-перехватчика, используемый группами действий, если в указанной группе существует одно или несколько действий веб-перехватчика. Вы можете задать формат JSON, который будет использоваться для всех веб-перехватчиков в связанной группе действий. Дополнительные сведения о форматах веб-перехватчиков см. в статье [Действия веб-перехватчика для правил оповещений журнала](../../azure-monitor/platform/alerts-log-webhook.md). Параметр просмотр веб-перехватчика предоставляется для проверки формата с помощью примера данных JSON.

        ![Переопределение действий оповещений журналов](media/alerts-log/AlertsPreviewOverrideLog.png)


1. Если все поля действительны и выделены зеленым цветом, нажмите кнопку **Создать правило оповещения**, чтобы создать оповещение в Azure Monitor ("Оповещения "). Все оповещения можно просмотреть с помощью панели мониторинга службы "Оповещения".

    ![Создание правила](media/alerts-log/AlertsPreviewCreate.png)

    Через несколько минут оповещение включится и будет активироваться, как было описано выше.

Пользователи также могут завершить свой запрос аналитики на [странице Logs Analytics на портале Azure](../../azure-monitor/log-query/portals.md#log-analytics-page
) и отправить его, чтобы создать оповещение с помощью кнопки «Настроить оповещение», а затем следовать инструкциям, начиная с шага 6, в приведенном выше руководстве.

 ![Настройка оповещения в Log Analytics](media/alerts-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Просмотр оповещений журнала, и управление ими на портале Azure

1. На [портале](https://portal.azure.com/) выберите **Мониторинг**, а затем в разделе "Мониторинг" выберите **Оповещения**.  

1. Отображается **Панель мониторинга оповещений**,в которой все оповещения Azure (включая оповещения журнала) отображаются в единственной панели мониторинга; включая каждый момент запуска правил генерации оповещений журнала. Дополнительные сведения см. в статье [Управление оповещениями](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Правила генерации оповещений журнала включают пользовательскую логику на основе запросов, предоставляемых пользователями, и, как следствие, не имеют разрешенного состояния. Из-за чего каждый раз,когда выполняются условия, указанные в правиле генерации оповещений журнала, создается оповещение. 


1. Нажмите кнопку **​​Управление правилами** на верхней панели, чтобы перейти к разделу управления правилами, в котором перечислены все созданные правила оповещений, включая отключенные оповещения.
    ![Управление правилами генерации оповещений](media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Управление оповещениями журнала с помощью шаблона ресурсов Azure
В настоящее время оповещения журнала можно создавать с помощью двух разных шаблонов ресурсов, на основе которых оповещение платформы аналитики должно основываться на Log Analytics или Application Insights.

Поэтому в следующем разделе приводятся сведения об использовании шаблона ресурсов для оповещений журнала для каждой платформы аналитики.

### <a name="azure-resource-template-for-log-analytics"></a>Шаблон ресурсов Azure для Log Analytics
Оповещения журнала для Log Analytics создаются правилами генерации оповещений, которые выполняют сохраненный поиск через равные промежутки времени. Если результаты запроса соответствуют указанным условиям, то создается запись оповещения и выполняются одно или несколько действий. 

Шаблон ресурсов для сохраненного поиска Log Analytics и оповещений Log Analytics доступен в разделе документации по Log Analytics. Дополнительные сведения см. в статье [Добавление сохраненных поисковых запросов и оповещений Log Analytics в решение по управлению (предварительная версия)](../../azure-monitor/insights/solutions-resources-searches-alerts.md), которая содержит демонстрационные примеры и сведения о схеме.

### <a name="azure-resource-template-for-application-insights"></a>Шаблон ресурсов Azure для Application Insights
Оповещение журнала для ресурсов Application Insights имеет тип `Microsoft.Insights/scheduledQueryRules/`. Дополнительные сведения о данном типе ресурса приведены в разделе [Scheduled Query Rules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) (Правила запланированных запросов).

Ниже приведена структура для [создания правил запланированных запросов](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) на основе шаблона ресурсов, содержащего пример набора данных в качестве переменных.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
    "variables": {
    "alertLocation": "southcentralus",
    "alertName": "samplelogalert",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
    "alertDesription": "Sample log search alert",
    "alertStatus": "true",
    "alertSource":{
        "Query":"requests",
        "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
        "Type":"ResultCount"
         },
     "alertSchedule":{
         "Frequency": 15,
         "Time": 60
         },
     "alertActions":{
         "SeverityLevel": "4"
         },
      "alertTrigger":{
        "Operator":"GreaterThan",
        "Threshold":"1"
         },
       "actionGrp":{
        "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
        "Subject": "Customized Email Header",
        "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"           
         }
  },
  "resources":[ {
    "name":"[variables('alertName')]",
    "type":"Microsoft.Insights/scheduledQueryRules",
    "apiVersion": "2018-04-16",
    "location": "[variables('alertLocation')]",
    "tags":{"[variables('alertTag')]": "Resource"},
    "properties":{
       "description": "[variables('alertDesription')]",
       "enabled": "[variables('alertStatus')]",
       "source": {
           "query": "[variables('alertSource').Query]",
           "dataSourceId": "[variables('alertSource').SourceId]",
           "queryType":"[variables('alertSource').Type]"
       },
      "schedule":{
           "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
           "timeWindowInMinutes": "[variables('alertSchedule').Time]"    
       },
      "action":{
           "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
           "severity":"[variables('alertActions').SeverityLevel]",
           "aznsAction":{
               "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
               "emailSubject":"[variables('actionGrp').Subject]",
               "customWebhookPayload":"[variables('actionGrp').Webhook]"
           },
       "trigger":{
               "thresholdOperator":"[variables('alertTrigger').Operator]",
               "threshold":"[variables('alertTrigger').Threshold]"
           }
       }
     }
   }
 ]
}
```
> [!IMPORTANT]
> Поле тега со скрытой ссылкой на целевой ресурс является обязательным при вызове API [правил запланированных запросов](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) или использовании шаблона ресурсов. 

В приведенном выше примере JSON можно сохранить как sampleScheduledQueryRule.json, чтобы использовать его в данном пошаговом руководстве. Его можно развернуть с помощью [Azure Resource Manager на портале Azure](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).


## <a name="managing-log-alerts-using-powershell-cli-or-api"></a>Управление оповещениями журнала с помощью PowerShell, CLI или API
В настоящее время оповещения журнала можно создавать с помощью двух разных интерфейсов API совместимых с Resource Manager, на основе которых оповещение платформы аналитики должно основываться на Log Analytics или Application Insights.

Таким образом в следующем разделе приведены сведения об использовании API с помощью Powershell или CLI для оповещений журнала для каждой платформы аналитики.

### <a name="powershell-cli-or-api-for-log-analytics"></a>PowerShell, CLI или API для Log Analytics
В службе Log Analytics используется REST API, поддерживающий RESTful. К нему можно получить доступ с помощью REST API Azure Resource Manager. Следовательно, к этому API можно обратиться из командной строки PowerShell, и он будет выдавать результаты поиска в формате JSON, позволяя программно использовать эти результаты различными способами.

Ознакомьтесь с разделом [Создание правил генерации оповещений и управление ими в Log Analytics с помощью REST API](../../azure-monitor/platform/api-alerts.md), включая примеры доступа к API из PowerShell.

### <a name="powershell-cli-or-api-for-application-insights"></a>PowerShell, CLI или API для Application Insights
[Scheduled Query Rules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) (Правила запланированных запросов) — это интерфейс REST API, который полностью совместим с REST API Azure Resource Manager. Поэтому он может использоваться в PowerShell с помощью командлета Resource Manager, а также в Azure CLI.

Ниже показано использование командлета PowerShell для Azure Resource Manager в примере шаблона ресурсов, приведенного выше (sampleScheduledQueryRule.json) в [разделе шаблонов ресурсов](#azure-resource-template-for-application-insights).
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Ниже показано использование команды Azure CLI для Azure Resource Manager в примере шаблона ресурсов, приведенного выше (sampleScheduledQueryRule.json) в [разделе шаблонов ресурсов](#azure-resource-template-for-application-insights).

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
После успешного выполнения возвращается код 201, который означает, что новое правило создано. Если было изменено существующее правило генерации оповещений, то возвращается код 200.


  
## <a name="next-steps"></a>Дополнительная информация

* Дополнительные сведения см. в статье [Оповещения журнала в Azure Monitor. Интерфейс оповещений](../../azure-monitor/platform/alerts-unified-log.md).
* Общие сведения о [действиях веб-перехватчиков для оповещений журнала](../../azure-monitor/platform/alerts-log-webhook.md)
* Дополнительные сведения об [Application Insights](../../azure-monitor/app/analytics.md)
* Дополнительные сведения о [Log Analytics](../../azure-monitor/log-query/log-query-overview.md). 

