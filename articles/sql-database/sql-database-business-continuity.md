---
title: Непрерывность облачных бизнес-процессов — восстановление базы данных — база данных SQL | Документация Майкрософт
description: Узнайте, как базы данных SQL Azure поддерживают непрерывное выполнение облачных бизнес-процессов и восстановление баз данных, а также помогают обеспечить выполнение важных облачных приложений.
keywords: непрерывность бизнес-процессов, непрерывность облачных бизнес-процессов, аварийное восстановление базы данных, восстановление базы данных
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 04/04/2019
ms.openlocfilehash: dfa5d4cb2d782f1466329300157a64fd17765460
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797699"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Обзор обеспечения непрерывности бизнес-процессов с помощью базы данных SQL Azure

**Непрерывность бизнес-процессов** в Базе данных SQL Azure относится к механизмам, политикам и процедурам, которые позволяют вашим бизнес-процессам продолжать работу при возникновении сбоев, особенно в вычислительной инфраструктуре. В большинстве случаев База данных SQL Azure будет обрабатывать аварийные события, которые могут произойти в облачной среде, и поддерживать выполнение приложений и бизнес-процессов. Однако существуют аварийные события, которые База данных SQL Azure обработать не может, например:

- Пользователь случайно удалил или обновил строку в таблице.
- Злоумышленник удалил данные или базу данных.
- Землетрясение привело к отключению электроэнергии и временному отключению центра обработки данных.

База данных SQL Azure не может контролировать данные случаи, поэтому вам нужно будет использовать функции обеспечения непрерывности бизнес-процессов, которые позволяют восстанавливать данные и поддерживать работу приложений.

В этом обзоре описываются возможности базы данных SQL Azure по обеспечению непрерывности бизнес-процессов и аварийного восстановления. Вы узнаете о параметрах, рекомендациях и руководствах по восстановлению после аварийных событий, которые могут приводить к потере данных или недоступности базы данных и приложений. Здесь также содержатся сведения о том, как действовать в ситуациях, когда ошибка пользователя или приложения нарушает целостность данных, происходит сбой региона Azure или приложение требует обслуживания.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Функции базы данных SQL, которые можно использовать для обеспечения непрерывности бизнес-процессов

С точки зрения базы данных существуют четыре основных сценария возможных нарушений:

- Локальные сбои оборудования или программного обеспечения, затрагивающие узел базы данных, например сбой диска.
- Повреждение или удаление данных, которое, как правило, возникает из-за ошибки приложения или пользователя. Такие сбои по своей сути относятся к конкретному приложению и, как правило, автоматически не обнаруживаются и не устраняются с помощью инфраструктуры.
- Отключение центра обработки данных, возможно, вызванное стихийным бедствием. Этот сценарий требует определенного уровня геоизбыточности с отработкой отказа приложений в альтернативном центре обработки данных.
- Ошибки обновления или обслуживания, непредвиденные проблемы, возникающие во время планового обновления или обслуживания приложения или базы данных. Они могут потребовать быстрого отката к предыдущему состоянию базы данных.

База данных SQL предоставляет несколько функций обеспечения непрерывности бизнес-процессов, включая автоматизированную архивацию и необязательную репликацию баз данных, которые могут справиться с этими сценариями. Во-первых, вам нужно понять, как [архитектура высокого уровня доступности](sql-database-high-availability.md) Базы данных SQL обеспечивает доступность на уровне 99,99 % и отказоустойчивость к определенным аварийным событиям, которые могут повлиять на бизнес-процесс.
Затем вы можете узнать о дополнительных механизмах, позволяющих выполнить восстановление после аварийных событий, которые невозможно обработать с помощью архитектуры высокой доступности Базы данных SQL, например:

- [Временные таблицы](sql-database-temporal-tables.md) позволяют восстанавливать версии строк из любой точки во времени.
- [Встроенные автоматически создаваемые резервные копии](sql-database-automated-backups.md) и [восстановление до точки во времени](sql-database-recovery-using-backups.md#point-in-time-restore) позволяют восстановить полную базу данных до определенной точки во времени в течение последних 35 дней.
- Вы можете [восстановить удаленную базу](sql-database-recovery-using-backups.md#deleted-database-restore) данных до точки, когда она была удалена, если **сервер Базы данных SQL не был удален**.
- [Долгосрочное хранение архивных копий](sql-database-long-term-retention.md) позволяет хранить резервные копии до 10 лет.
- [Активная георепликация](sql-database-active-geo-replication.md) позволяет создавать читаемые реплики и вручную переходить на другой ресурс реплики в случае сбоя центра обработки данных или обновления приложения.
- [Группа автоматической отработки отказа](sql-database-auto-failover-group.md#auto-failover-group-terminology-and-capabilities) позволяет приложению автоматически восстановиться в случае сбоя центра обработки данных.

У каждой из них разные параметры оценки времени восстановления (ERT) и возможной потери данных последних транзакций. Поняв, на что влияют эти параметры, вы сможете выбрать те, которые вам подходят, и, в большинстве случаев, использовать их вместе в различных сценариях. При разработке плана обеспечения непрерывности бизнес-процессов нужно понимать, какое максимальное время до полного восстановления приложения после аварийного события допустимо. Время, необходимое приложению для полного восстановления, называется целевым временем восстановления (RTO). Необходимо также понимать, потерю какого максимального периода последних обновлений данных (т. е. за какой интервал времени) приложение может допустить при восстановлении после аварийного события. Этот период обновлений является целевой точкой восстановления (RPO).

В следующей таблице сравниваются ERT и RPO для каждого уровня обслуживания для наиболее распространенных сценариев.

| Функция | базовая; | Стандартная | Премиум | Общего назначения | Критически важный для бизнеса
| --- | --- | --- | --- |--- |--- |
| Восстановление до точки во времени из резервной копии |Все точки восстановления в течение семи дней |Все точки восстановления в течение 35 дней |Все точки восстановления в течение 35 дней |Все точки восстановления за указанный период (до 35 дней)|Все точки восстановления за указанный период (до 35 дней)|
| Геовосстановление из геореплицированных резервных копий |ERT < 12 ч<br> RPO < 1 ч |ERT < 12 ч<br>RPO < 1 ч |ERT < 12 ч<br>RPO < 1 ч |ERT < 12 ч<br>RPO < 1 ч|ERT < 12 ч<br>RPO < 1 ч|
| Группы автоматической отработки отказа |RTO = 1 ч<br>RPO < 5 с |RTO = 1 ч<br>RPO < 5 с |RTO = 1 ч<br>RPO < 5 с |RTO = 1 ч<br>RPO < 5 с|RTO = 1 ч<br>RPO < 5 с|
| Отработка отказа базы данных вручную |ERT = 30 s<br>RPO < 5 с |ERT = 30 s<br>RPO < 5 с |ERT = 30 s<br>RPO < 5 с |ERT = 30 s<br>RPO < 5 с|ERT = 30 s<br>RPO < 5 с|

> [!NOTE]
> *Отработка отказа базы данных вручную* ссылается на другой ресурс для отдельной базы данных ее георепликацией — дополнительный с помощью [незапланированной режим](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities).

## <a name="recover-a-database-to-the-existing-server"></a>Восстановление базы данных на имеющемся сервере

Служба "База данных SQL" автоматически создает полные резервные копии (раз в неделю) и разностные резервные копии базы данных (в основном, каждые 12 часов), а также резервные копии журнала транзакций (раз в 5–10 минут) для защиты вашей компании от потери данных. Резервные копии хранятся в хранилище RA-GRS в течение 35 дней для всех уровней обслуживания, за исключением базовых с единицами DTU, где резервные копии хранятся в течение 7 дней. Дополнительные сведения см. в статье [Подробнее о резервном копировании базы данных SQL](sql-database-automated-backups.md). Вы можете восстановить существующую базу данных из автоматически создаваемых резервных копий до предшествующей точки во времени как новую базу данных на том же сервере Базы данных SQL с помощью портала Azure, PowerShell или REST API. Дополнительные сведения см. в разделе [Восстановление до точки во времени](sql-database-recovery-using-backups.md#point-in-time-restore).

Если для вашего приложения недостаточно максимального поддерживаемого срока хранения для восстановления до точки во времени (PITR), его можно увеличить, настроив для баз данных долгосрочную политику хранения (LTR). Дополнительные сведения см. в разделе [Долгосрочное хранение резервных копий](sql-database-long-term-retention.md).

Эти создаваемые автоматически резервные копии можно использовать для восстановления базы данных после различных аварийных событий как в своем, так и в другом центре обработки данных. Обычно время восстановления составляет менее 12 часов. Большая или активная база данных может восстанавливаться дольше. При использовании создаваемых автоматически резервных копий базы данных предполагаемое время восстановления будет зависеть от нескольких факторов, включая общее количество баз данных, восстанавливаемых в том же регионе и в то же время, размера базы данных, размера журнала транзакций и пропускной способности сети. Дополнительные сведения см. в разделе [Время восстановления](sql-database-recovery-using-backups.md#recovery-time). При восстановлении в другой регион потенциальные потери данных ограничены 1 часом благодаря геоизбыточным резервным копиям.

Используйте автоматически создаваемые резервные копии и [восстановление до точки во времени](sql-database-recovery-using-backups.md#point-in-time-restore) для обеспечения непрерывности бизнес-процессов и восстановления, если ваше приложение:

- не является критически важным;
- на него не распространяется действие соглашений об уровне обслуживания, то есть перерыв в работе на 24 часа и более не повлечет финансовой ответственности;
- имеет низкую частоту изменения данных (количество транзакций в час), и потеря до часа изменений данных считается допустимой;
- имеет ограниченный бюджет.

Если вам требуется более быстрое восстановление, используйте [активную георепликацию](sql-database-active-geo-replication.md) или [группы автоматической отработки отказа](sql-database-auto-failover-group.md). Если вам нужна возможность восстанавливать данные за период больше 35 дней, используйте [долгосрочное хранение резервных копий](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region"></a>Восстановление базы данных в другом регионе

В редких случаях возможен сбой центра обработки данных Azure. Такой сбой вызывает нарушение работы компании, которое может длиться от считанных минут до нескольких часов.

- Один из вариантов — дождаться возобновления работы базы данных после того, как сбой центра обработки данных будет устранен. Это подходит для приложений, которые допускают отключение базы данных от сети. Например, вам не нужно непрерывно работать с проектом по разработке или бесплатной пробной версией. После того как произошел сбой центра обработки данных, неизвестно, как долго будет длиться простой. Поэтому этот вариант используется, только если база данных некоторое время не нужна.
- Другим вариантом является восстановление базы данных на любом сервере в любом регионе Azure с использованием [резервных копий геоизбыточных баз данных](sql-database-recovery-using-backups.md#geo-restore) (геовосстановление). В качестве источника геовосстановление использует геоизбыточную резервную копию для восстановления базы данных, даже если она или центр обработки данных недоступны из-за сбоя.
- И, наконец, можно быстро восстановить после сбоя, если вы настроили получатель геореплицируемых данных с помощью [активной георепликации](sql-database-active-geo-replication.md) или [группы автоматической отработки отказа](sql-database-auto-failover-group.md) для базы данных или баз данных. В зависимости от выбора технологии можно использовать переход на другой ресурс автоматически или вручную. Хотя отработка отказа занимает всего несколько секунд, службе понадобится как минимум 1 час, чтобы активировать ее. Нужно удостовериться, что отработка отказа соответствует масштабу сбоя. Кроме того, при отработке отказа возможна небольшая потеря данных из-за характера асинхронной репликации. Для дополнительной информации об автоматической отработке отказа RTO и RPO см. таблицу, приведенную выше.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>
> [!IMPORTANT]
> Для использования активной георепликации и групп автоматической отработки отказа необходимо быть владельцем подписки или иметь разрешения администратора в SQL Server. Для настройки и отработки отказа можно использовать портал Azure, PowerShell или REST API, имея разрешения для подписки Azure, или Transact-SQL, имея разрешения для SQL Server.

Используйте группы автоматической отработки отказа, если приложение соответствует любому из следующих критериев:

- оно является критически важным;
- его условия Соглашения об уровне обслуживания (SLA) не допускают более 12 часов простоя;
- его простой может повлечь финансовую ответственность;
- имеет высокую частоту изменения данных, и потеря данных за час не допускается;
- дополнительные затраты на активную георепликацию ниже, чем потенциальная финансовая ответственность и связанная с ней утрата деловых возможностей.

Сколько времени займет восстановление и каковы будут потери данных зависит от того, как вы решите использовать в приложении возможности обеспечения непрерывности бизнес-процессов. Разумеется, вы можете использовать сочетание резервных копий базы данных и активной георепликации, в зависимости от требований приложения. Вопросы разработки приложений для автономных баз данных и эластичных пулов, использующих эти возможности обеспечения непрерывности бизнес-процессов, рассматриваются в статьях [Разработка глобально доступных служб с помощью Базы данных SQL Azure](sql-database-designing-cloud-solutions-for-disaster-recovery.md) и [Стратегии аварийного восстановления для приложений с использованием пула эластичных баз данных SQL](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).


В следующих разделах описываются инструкции по восстановлению с использованием резервных копий базы данных или активной георепликации. Подробные инструкции, включающие в себя планирование требований, действия после восстановления и сведения о том, как имитировать сбой для отработки аварийного восстановления, см. в статье [Восстановление базы данных SQL Azure или переход на базу данных-получатель при отказе](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Подготовка к сбою

Независимо от выбранной функции обеспечения непрерывности бизнес-процессов необходимо выполнить следующее.

- Определите и подготовьте целевой сервер, включая правила брандмауэра протокола IP уровня сервера, имена для входа и разрешения уровня базы данных master.
- Определите, как клиенты и клиентские приложения будут перенаправляться на новый сервер.
- Задокументируйте прочие зависимости, такие как параметры аудита и оповещения.

Без соответствующей подготовки возобновление работы приложений после отработки отказа или восстановления базы данных занимает больше времени и, вероятно, также требует устранения неполадок во время стрессовой нагрузки, а это весьма неудачное сочетание.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Отработка отказа в геореплицируемую базу данных-получатель

При использовании активной георепликации и групп автоматической отработки отказа в качестве механизма восстановления, можно настроить политику автоматической отработки отказа или использовать [вручную внеплановой отработки отказа](sql-database-active-geo-replication-portal.md#initiate-a-failover). После запуска отработка отказа приводит к повышению базы данных-получателя до новой базы данных-источника и делает ее доступной для записи новых транзакций и реагирования на запросы. При этом происходят минимальные потери данных, которые еще не реплицированы. Сведения о разработке процесса отработки отказа см. в статье [Проектирование приложения для облачного аварийного восстановления с использованием активной георепликации в базе данных SQL](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> После возобновления работы центра обработки данных старые базы данных-источники автоматически переподключаются к новым базам данных-источникам и становятся базами данных-получателями. Если базу данных-источник необходимо повторно переместить в исходный регион, вы можете вручную запустить плановую отработку отказа (восстановление размещения).

### <a name="perform-a-geo-restore"></a>Процедура геовосстановления

При использовании автоматически создаваемых резервных копий и геоизбыточного хранилища (включено по умолчанию) базу данных можно восстановить с помощью функции [геовосстановление](sql-database-disaster-recovery.md#recover-using-geo-restore). Обычно восстановление выполняется в пределах 12 часов с потерей данных за период до одного часа, что зависит от времени создания и репликации последней резервной копии журналов. До завершения восстановления база данных не может записывать какие-либо транзакции или отвечать на запросы. Обратите внимание, что функция геовосстановления восстанавливает базу данных до последней доступной точки во времени.

> [!NOTE]
> Если работа центра обработки данных возобновится до переключения приложения на восстановленную базу данных, то можно будет отменить восстановление.

### <a name="perform-post-failover--recovery-tasks"></a>Выполнение задач после восстановления размещения и восстановления

После восстановления посредством любого из механизмов восстановления необходимо выполнить следующие дополнительные задачи, прежде чем данные пользователей и приложений будут заархивированы, а они смогут возобновить работу.

- Перенаправьте клиенты и клиентские приложения на новый сервер и восстановленную базу данных.
- Убедитесь, что заданы соответствующие правила брандмауэра протокола IP уровня сервера, позволяющие пользователям подключаться к [брандмауэрам уровня базы данных](sql-database-firewall-configure.md#manage-server-level-ip-firewall-rules-using-the-azure-portal) или использовать их, чтобы включить соответствующие правила.
- Убедитесь, что заданы соответствующие имена для входа и разрешения уровня базы данных master (или используйте [автономных пользователей](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable)).
- Настройте аудит соответствующим образом.
- Настройте оповещения соответствующим образом.

> [!NOTE]
> Если вы используете группу отработки отказа и подключаетесь к базе данных, используя прослушиватель для чтения и записи, после отработки отказа произойдет автоматическое и прозрачное перенаправление в приложение.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Обновление приложения с минимальным простоем

Иногда приложение нужно перевести в автономный режим для планового обслуживания, например, чтобы обновить его. В статье [Управление последовательными обновлениями для облачных приложений с помощью активной георепликации базы данных SQL](sql-database-manage-application-rolling-upgrade.md) описывается, как с помощью активной георепликации обеспечить последовательное обновление облачного приложения, чтобы свести к минимуму простой при обновлениях и обеспечить путь восстановления, если случится сбой.

## <a name="next-steps"></a>Дальнейшие действия

Вопросы разработки приложений для автономных баз данных и эластичных пулов рассматриваются в статьях [Разработка глобально доступных служб с помощью Базы данных SQL Azure](sql-database-designing-cloud-solutions-for-disaster-recovery.md) и [Стратегии аварийного восстановления для приложений с использованием пула эластичных баз данных SQL](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
