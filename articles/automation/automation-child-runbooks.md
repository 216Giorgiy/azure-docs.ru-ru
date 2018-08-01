---
title: Дочерние модули Runbook в службе автоматизации Azure
description: Описывает различные методы запуска Runbook в службе автоматизации Azure из другого Runbook и обмена информацией между ними.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 582513e7e556859e70c1af9c4f6179e1d60e0139
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216747"
---
# <a name="child-runbooks-in-azure-automation"></a>Дочерние модули Runbook в службе автоматизации Azure

Рекомендуется создавать в службе автоматизации Azure повторно используемые модульные Runbook с отдельной функцией, которую могут использовать другие Runbook. Родительский Runbook часто будет вызывать один или несколько дочерних Runbook для выполнения требуемых функций. Существует два способа вызвать дочерний Runbook, и у каждого из них есть определенные отличия, которые следует понимать, чтобы определить, какой способ наилучшим образом подходит для тех или иных сценариев.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Вызов дочернего Runbook с использованием встроенного выполнения

Чтобы вызвать Runbook изнутри другого Runbook, используйте имя Runbook и укажите значения его параметров точно так же, как при использовании действия или командлета.  Так все Runbook в одной учетной записи службы автоматизации доступны для использования всем остальным. Родительский Runbook ожидает завершения работы дочернего Runbook, прежде чем перейти к следующей строке, и любые выходные данные возвращаются непосредственно в родительский Runbook.

При встроенном вызове дочерний Runbook выполняется в той же задаче, что и родительский Runbook. В журнале заданий дочернего Runbook не будет записей о его выполнении. Любые исключения и любой потоковый вывод дочернего Runbook будут связаны с родительским Runbook. Это уменьшает число заданий и упрощает отслеживание и устранение неполадок, так как все исключения, порождаемые дочерним Runbook, и любой его потоковый вывод связан с родительским заданием.

При публикации Runbook все дочерние Runbook, которые он вызывает, уже должны быть опубликованы. Это связано с тем, что служба автоматизации Azure создает связь с любыми дочерними Runbook при компиляции Runbook. В противном случае родительский компонент Runbook будет отображаться как доступный для публикации, но будет выдавать исключение при запуске. В этом случае можно повторно опубликовать родительский Runbook, чтобы исправить ссылки на дочерние Runbook. Не надо повторно публиковать родительский Runbook, если были изменены все дочерние Runbook, так как связь уже будет создана.

В параметрах дочернего модуля runbook со встроенным вызовом может использоваться любой тип данных, включая составные объекты. Кроме того, отсутствует [сериализация JSON](automation-starting-a-runbook.md#runbook-parameters), как при запуске runbook с помощью портала Azure или командлета Start-AzureRmAutomationRunbook.

### <a name="runbook-types"></a>Типы Runbook

Какие типы могут вызывать друг друга?

* [Runbook PowerShell](automation-runbook-types.md#powershell-runbooks)и [графические модули Runbook](automation-runbook-types.md#graphical-runbooks) могут вызывать друг друга с помощью встроенного вызова (на основе PowerShell).
* [Runbook рабочего процесса PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) и графические модули Runbook рабочего процесса PowerShell могут вызывать друг друга с помощью встроенного вызова (на основе рабочих процессов PowerShell).
* Типы PowerShell и типы рабочего процесса PowerShell не могут вызывать друг друга с помощью встроенного вызова и должны использовать командлет Start-AzureRmAutomationRunbook.

Когда имеет значение порядок публикации?

* Порядок публикации модулей Runbook имеет значение только для модулей Runbook рабочего процесса PowerShell и графических модулей Runbook рабочего процесса PowerShell.

При вызове графического Runbook и Runbook рабочего процесса PowerShell с помощью встроенного выполнения просто используйте имя необходимого Runbook.  При вызове дочернего модуля runbook PowerShell необходимо поставить перед его именем символ *.\\*, чтобы указать, что скрипт находится в локальном каталоге. 

### <a name="example"></a>Пример

В следующем примере вызывается тестовый дочерний Runbook, который принимает три параметра: сложный объект, целое число и логическое значение. Выходные данные дочернего Runbook присваиваются переменной.  В этом случае дочерний модуль runbook является модулем runbook рабочего процесса PowerShell.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

Ниже приведен тот же пример с использованием Runbook PowerShell в качестве дочернего элемента.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-cmdlet"></a>Запуск дочернего Runbook с помощью командлета

Чтобы запустить модуль Runbook, можно воспользоваться командлетом [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook), как описано в разделе [Запуск модуля Runbook с помощью Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Этот командлет можно использовать в двух режимах.  В одном режиме командлет возвращает идентификатор задания при создании дочернего задания для дочернего модуля Runbook.  В другом режиме, который можно включить, указав параметр **-wait** , командлет ожидает завершения дочернего задания и возвращает выходные данные из дочернего модуля Runbook.

Задание дочернего Runbook, запущенного с помощью командлета, будет выполняться отдельно от задания родительского Runbook. Это порождает больше заданий, чем при встроенном вызове компонента runbook, и усложняет их отслеживание. Родительский модуль Runbook может запустить несколько дочерних модулей Runbook асинхронно, не ожидая завершения каждого из них. Для такого параллельного выполнения дочерних Runbook с помощью встроенного вызова в родительском Runbook потребуется использовать [ключевое слово parallel](automation-powershell-workflow.md#parallel-processing).

Выходные данные дочерних модулей Runbook не всегда возвращаются в родительский модуль из-за расписания выполнения. Кроме того, определенные переменные, такие как $VerbosePreference, $WarningPreference и другие, могут не распространяться на дочерние модули Runbook. Чтобы избежать этих проблем, можно вызвать дочерние модули Runbook как отдельные задания службы автоматизации с помощью командлета `Start-AzureRmAutomationRunbook` с параметром `-Wait`. В результате этого выполнение родительского модуля Runbook будет заблокировано до завершения дочернего модуля.

Если вы не хотите блокировать родительский модуль Runbook во время ожидания, можно вызвать дочерний модуль Runbook с помощью командлета `Start-AzureRmAutomationRunbook` без параметра `-Wait`. Затем вам придется использовать `Get-AzureRmAutomationJob` для ожидания завершения задания, а также `Get-AzureRmAutomationJobOutput` и `Get-AzureRmAutomationJobOutputRecord` для получения результатов.

Параметры дочернего Runbook, запускаемого с помощью командлета, предоставляются в виде хэш-таблицы, как описано в статье [Параметры Runbook](automation-starting-a-runbook.md#runbook-parameters). Можно использовать только простые типы данных. Если у Runbook есть параметр со сложным типом данных, то для него необходимо использовать встроенный вызов.

### <a name="example"></a>Пример

В следующем примере с помощью командлета Start-AzureRmAutomationRunbook с параметром -wait запускается дочерний модуль Runbook с параметрами и ожидается его завершение. После завершения его выходные данные собираются из дочернего модуля Runbook. Для использования `Start-AzureRmAutomationRunbook` необходимо пройти проверку подлинности в подписке Azure.

```azurepowershell-interactive
# Connect to Azure with RunAs account
$conn = Get-AutomationConnection -Name "AzureRunAsConnection"

$null = Add-AzureRmAccount `
  -ServicePrincipal `
  -TenantId $conn.TenantId `
  -ApplicationId $conn.ApplicationId `
  -CertificateThumbprint $conn.CertificateThumbprint

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}
$joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResourceGroupName "LabRG" –Parameters $params –wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Сравнение методов вызова дочернего Runbook
В следующей таблице перечислены различия между двумя методами вызова Runbook из другого Runbook.

|  | Встроенный | Командлет |
|:--- |:--- |:--- |
| Задание |Дочерние Runbook выполняются в том же задании, что и родительский. |Для дочернего Runbook создается отдельное задание. |
| Выполнение |Родительский Runbook ожидает завершения дочернего Runbook, прежде чем продолжить выполнение. |Родительский модуль Runbook продолжает работу сразу после запуска дочернего модуля Runbook *или* ожидает завершения дочернего задания. |
| Выходные данные |Родительский Runbook может получить выходные данные непосредственно из дочернего Runbook. |Родительский модуль Runbook должен получить выходные данные из задания дочернего модуля Runbook *или* может получить выходные данные непосредственно из дочернего модуля Runbook. |
| Параметры |Значения параметров дочернего Runbook указываются отдельно и могут иметь любой тип данных. |Значения параметров дочернего Runbook должны быть объединены в одну хэш-таблицу и могут содержать только простые, регулярные и объектные типы данных, использующие сериализацию JSON. |
| Учетная запись службы автоматизации |Родительский Runbook может использовать только дочерний Runbook, находящийся в той же учетной записи автоматизации. |Родительский модуль Runbook может использовать дочерний Runbook из любой учетной записи автоматизации в той же подписке Azure и даже в другой подписке, если к ней есть подключение. |
| Публикация |Перед публикацией родительского Runbook должен быть опубликован дочерний Runbook. |Дочерний Runbook должен быть опубликован в любое время до запуска родительского Runbook. |

## <a name="next-steps"></a>Дополнительная информация

* [Запуск модуля Runbook в службе автоматизации Azure](automation-starting-a-runbook.md)
* [Выходные данные и сообщения Runbook в службе автоматизации Azure](automation-runbook-output-and-messages.md)
