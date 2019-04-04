---
title: Устранение неполадок в службах BizTalk с помощью журналов операций | Документация Майкрософт
description: Устранение неполадок в службах BizTalk с помощью журналов операций. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 35c30ec97d4c033a687204b926d2f686c52be069
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916031"
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>Службы BizTalk. устранение неполадок с помощью журналов операций

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="what-are-the-operation-logs"></a>Что такое журналы операций
Журналы операций — это компонент служб управления, который позволяет просматривать журналы операций, выполняемых в службах Azure, в том числе службах BizTalk. Это позволяет просматривать данные журнала, относящиеся к операциям управления, которые выполнены для подписки на службу BizTalk 180 дней назад или позднее.

> [!NOTE]
> Эта функция собирает информацию журналов только для операций управления в службах BizTalk, например, время запуска службы, создания резервной копии и т. д. Такие операции отслеживаются с помощью [интерфейсов REST API службы BizTalk](/previous-versions/azure/reference/dn232347(v=azure.100)). Полный список операций, которые отслеживаются с помощью служб управления, см. в разделе [Операции, отслеживаемые с помощью служб управления Azure](#bizops).<br/><br/>
> Этот компонент не ведет журналы для действий, связанных со средой выполнения службы BizTalk (например, при обработке сообщения мостами и т. д.). Чтобы просмотреть эти журналы, используйте представление отслеживания на портале служб BizTalk. Дополнительные сведения см. в статье [Отслеживание сообщений](/previous-versions/azure/hh949805(v=azure.100)).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>Просмотр журналов операций служб BizTalk
1. На портале Azure выберите **Службы управления**, а затем щелкните вкладку **Журналы операций**.
2. Можно фильтровать журналы по различным параметрам, таким как подписки, диапазон дат, тип службы (например, службы BizTalk), имя службы или состояние операции (например, "Успешно", "Ошибка").
3. Щелкните флажок, чтобы просмотреть отфильтрованный список. На следующем изображении показаны действия, связанные с testbiztalkservice:  ![Просмотр журналов операций][ViewLogs] 
4. Чтобы узнать больше о конкретной операции, выберите строку и щелкните **Подробности** на панели задач в нижней части страницы.

## <a name="bizops"></a>Операции, отслеживаемые с помощью служб управления Azure
В следующей таблице перечислены операции, которые отслеживаются с помощью служб управления Azure.

| Имя операции | Задача |
| --- | --- |
| CreateBizTalkService |Операция создания новой службы BizTalk |
| DeleteBizTalkService |Операция удаления службы BizTalk |
| RestartBizTalkService |Операция перезапуска службы BizTalk |
| StartBizTalkService |Операция запуска службы BizTalk |
| StopBizTalkService |Операция остановки службы BizTalk |
| DisableBizTalkService |Операция отключения службы BizTalk |
| EnableBizTalkService |Операция включения службы BizTalk |
| BackupBizTalkService |Операция резервного копирования службы BizTalk |
| RestoreBizTalkService |Операция восстановления службы BizTalk из указанной резервной копии |
| SuspendBizTalkService |Операция приостановки службы BizTalk |
| ResumeBizTalkService |Операция возобновления службы BizTalk |
| ScaleBizTalkService |Операция масштабирования службы BizTalk вверх или вниз |
| ConfigUpdateBizTalkService |Операция обновления конфигурации службы BizTalk |
| ServiceUpdateBizTalkService |Операция обновления или возврата к предыдущей версии службы BizTalk |
| PurgeBackupBizTalkService |Операция удаления резервных копий службы BizTalk по истечении срока хранения |

## <a name="see-also"></a>См. также
* [Резервное копирование службы BizTalk](https://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Восстановление службы BizTalk из резервной копии](https://go.microsoft.com/fwlink/p/?LinkID=325582)
* [Службы BizTalk. Developer, Basic, Standard и Premium диаграмма выпусков](https://go.microsoft.com/fwlink/p/?LinkID=302279)
* [Службы BizTalk. Подготовка](https://go.microsoft.com/fwlink/p/?LinkID=302280)
* [Службы BizTalk. Схема состояний подготовки](https://go.microsoft.com/fwlink/p/?LinkID=329870)
* [Службы BizTalk. Вкладки панели мониторинга, мониторинг и масштабирование](https://go.microsoft.com/fwlink/p/?LinkID=302281)
* [Службы BizTalk. Регулирование](https://go.microsoft.com/fwlink/p/?LinkID=302282)
* [Службы BizTalk. Имя издателя и ключ издателя](https://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Инструкции запуска с помощью пакета SDK служб BizTalk Azure](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

