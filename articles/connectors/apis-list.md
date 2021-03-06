---
title: Соединители для Azure Logic Apps
description: Автоматизация рабочих процессов с помощью соединителей для Azure Logic Apps, включая встроенную, управляемую, локальную, корпоративную учетную запись интеграции и корпоративные соединители
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: 59c8effb4c5feae99755b7937f4796e8f11fde46
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "58895889"
---
# <a name="connectors-for-azure-logic-apps"></a>Соединители для Azure Logic Apps

Соединители обеспечивают быстрый доступ из Azure Logic Apps для событий, данных и действия для других приложений, систем, протоколы и службы платформ. С помощью соединителей в logic apps позволяет расширить возможности для облачных и локальных приложений для выполнения задач с данными, уже есть.

Хотя система Logic Apps предлагает [свыше 200 соединителей](https://docs.microsoft.com/connectors), в этой статье описаны популярные соединители, чаще всего используемые во многих приложениях и выполнениях для обработки данных и информации. Чтобы получить полный список соединителей и каждый соединитель справочные сведения, такие как триггеры, действия и ограничения, проверьте соединитель ссылается на страницы в разделе [Общие сведения о соединителях](https://docs.microsoft.com/connectors). Кроме того, Дополнительные сведения о [триггеры и действия](#triggers-actions).

> [!NOTE]
> Для интеграции со службой или API, который не имеет соединителя, можно непосредственно вызывать службу с помощью протокола, например HTTP или создать [Настраиваемый соединитель](#custom).

Соединители доступны как встроенные триггеры и действия или управляемые соединители.

* [**Встроенные объекты**](#built-ins): Эти встроенные действия и триггеры являются «native» для Azure Logic Apps и помогут создать приложения логики, которые работают на пользовательские расписания, взаимодействовать с другими конечными точками, получать и реагировать на запросы и вызова функции Azure, приложения API Azure (веб-приложения), собственные интерфейсы API управляемые, так и опубликованные с помощью управления API Azure и вложенных приложений логики, которые могут получать запросы. Кроме того, с помощью встроенных действий можно организовать и контролировать рабочий процесс приложения логики, а также работать с данными.

* **Управляемые соединители.** Развертываются и управляются корпорацией Майкрософт, эти соединители предоставляют триггеры и действия для доступа к других служб и систем, таких как Office 365, хранилище BLOB-объектов Azure, SQL Server, Salesforce и многое другое. Некоторые соединители требуется сначала создать подключения, которые управляются Azure Logic Apps. Управляемые соединители разделены на следующие группы:

  |   |   |
  |---|---|
  | [**Соединители управляемого API**](#managed-api-connectors) | Создавайте приложения логики, которые используют такие службы, как хранилище BLOB-объектов, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online и многие другие. | 
  | [**Локальные соединители**](#on-premises-connectors) | После установки и настройки [локального шлюза данных][gateway-doc] эти соединители помогают приложениям логики получать доступ к локальным системам, таким как SQL Server, SharePoint Server, Oracle DB, общие файловые ресурсы и другие. | 
  | [**Соединители для учетной записи интеграции**](#integration-account-connectors) | Доступные при создании и оплате учетной записи интеграции. Эти соединители преобразуют и проверяют XML, кодируют и декодируют неструктурированные файлы, а также обрабатывают сообщения типа "бизнес-бизнес" (B2B) с помощью протоколов AS2, EDIFACT и X12. | 
  | [**Корпоративные соединители**](#enterprise-connectors) | Предоставьте доступ к корпоративным системам, таким как SAP и IBM MQ, за дополнительную плату. |
  ||| 

  Например, при использовании Microsoft BizTalk Server приложения логики можно подключить к BizTalk Server с помощью [соединителя BizTalk Server](#on-premises-connectors) и обеспечить взаимодействие приложений с BizTalk Server с помощью этого соединителя. 
  Затем вы можете расширить или выполнить операции, подобные BizTalk, в своих приложениях логики с помощью [соединителей для учетной записи интеграции](#integration-account-connectors). 

> [!NOTE]
> Полный список соединителей и справочную информацию о каждом, такую как действия и триггеры, определяемые описанием Swagger, и любые ограничения можно найти в статье об [общих сведениях о соединителях](/connectors/). Информацию о ценах см. в статьях о [тарифах Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) и [модели ценообразования Logic Apps](../logic-apps/logic-apps-pricing.md). 

<a name="built-ins"></a>

## <a name="built-ins"></a>Встроенные элементы

Система Logic Apps предоставляет встроенные триггеры и действия, поэтому вы можете создавать рабочие процессы на основе расписания, помогать приложениям логики взаимодействовать с другими приложениями и службами, контролировать рабочий процесс с помощью приложений логики, а также управлять данными и обрабатывать их. 

|   |   |   |   | 
|---|---|---|---| 
| [![Значок API][schedule-icon]<br/>**Расписание**][recurrence-doc] | — Запускайте приложение логики с учетом указанного расписания (начиная с простого и закачивая сложным повтором) с помощью триггера **Повторения**. <p>— Приостанавливайте работу приложения логики на определенное время с помощью действия **Задержка**. <p>— Приостанавливайте работу приложения логики до указанной даты и времени с помощью действия **Задержка до**. | [![Значок API][http-icon]<br/>**HTTP**][http-doc] | Взаимодействуйте с любой конечной точкой через протокол HTTP с помощью триггеров и действий для HTTP, HTTP + Swagger и HTTP + Webhook. | 
| [![Значок API][http-request-icon]<br/>**Запрос**][http-request-doc] | — Сделайте свое приложение логики доступным для вызовов от других приложений или служб, активируйте события ресурса Event Grid или ответ на предупреждения Azure Security Center с помощью триггера **Запрос**. <p>— Отправляйте ответы в приложение или службу с помощью действия **Ответ**. | [![Значок API][batch-icon]<br/>**Пакет**][batch-doc] | — Обрабатывайте сообщения в пакетах с помощью триггера **Пакетные сообщения**. <p>— Вызывайте приложения логики, имеющие пакеты триггеров, с помощью действия **Send messages to batch** (Отправка сообщений в пакет). | 
| [![Значок API][azure-functions-icon]<br/>**Функции Azure**][azure-functions-doc] | Вызывайте функции Azure, которые запускают настраиваемые фрагменты кода (C# или Node.js) из ваших приложений логики. | [![Значок API][azure-api-management-icon]</br>**Управление API Azure**][azure-api-management-doc] | Вызывайте триггеры и действия, определенные API-интерфейсами, которые вы публикуете и контролируете с помощью управления API Azure. | 
| [![Значок API][azure-app-services-icon]<br/>**Служба приложений Azure**][azure-app-services-doc] | Вызывайте приложения API Azure или веб-приложения, размещенные в Службе приложений Azure. Если включен Swagger, триггеры и действия, определенные этими приложениями, отображаются как любые другие триггеры и действия первого класса. | [![Значок API][azure-logic-apps-icon]<br/>**Azure<br/>Logic Apps** ][nested-logic-app-doc] | Вызывайте другие приложения логики, которые запускаются с помощью триггера "Запуск". | 
||||| 

### <a name="control-workflow"></a>Рабочий процесс управления

Ниже приведены встроенные действия для структурирования и управления действиями в рабочем процессе приложения логики:

|   |   |   |   | 
|---|---|---|---| 
| [![Значок встроенного действия][condition-icon]<br/>**Условие**][condition-doc] | Оценка условия и выполнение различных действий на основе значения условия: True или False. | [![Значок встроенного действия][for-each-icon]</br>**For each**][for-each-doc] | Выполнение одинаковых действий для каждого элемента в массиве. | 
| [![Значок встроенного действия][scope-icon]<br/>**Область**][scope-doc] | Группировка действий в *области*, которые получают свое состояние после завершения действий в области. | [![Значок встроенного действия][switch-icon]</br>**Переключить**][switch-doc] | Группируйте действия в *варианты*, которым присваиваются уникальные значения, за исключением варианта по умолчанию. Запускайте только тот вариант, присвоенное значение которого соответствует результату выражения, объекта или токена. Если совпадений не существует, запустите вариант по умолчанию. | 
| [![Значок встроенного действия][terminate-icon]<br/>**Завершение**][terminate-doc] | Остановка выполнения активного рабочего процесса приложения логики. | [![Значок встроенного действия][until-icon]<br/>**До**][until-doc] | Повторяйте действия до тех пор, пока значение указанного условия не будет равно True или какое-нибудь из состояний не изменится. | 
||||| 

### <a name="manage-or-manipulate-data"></a>Управление данными или их обработка

Ниже приведены встроенные действия для работы с выводами данных и их форматами:  

|   |   | 
|---|---| 
| [![Значок встроенного действия][data-operations-icon]<br/>**Операции с данными**][data-operations-doc] | Выполнение операций с данными: <p>- **Compose**: Создайте единый выход из нескольких наборов входных данных с различными типами. <br>- **Создать таблицу CSV**: Создайте таблицу разделителями запятыми (CSV) из массива с объектами JSON. <br>- **Создание таблицы HTML**: Создание таблицы HTML из массива с объектами JSON. <br>- **Фильтрация массива**: Создайте массив из элементов в другой массив, соответствующих вашим критериям. <br>- **Присоединяйтесь к**: Создание строки из всех элементов в массиве и разделения этих элементов с указанным разделителем. <br>- **Анализ JSON**: Создайте понятные токены из свойств и их значения в JSON-содержимое, чтобы эти свойства можно использовать в рабочем процессе. <br>- **Выберите**: Создание массива объектов JSON путем преобразования элементов или значений в другой массив и сопоставление этих элементов с заданными свойствами. | 
| ![Значок встроенного действия][date-time-icon]<br/>**Дата и время** | Выполнение операций с метками времени: <p>- **Добавляет к времени**: Добавите указанное количество единиц с меткой времени. <br>- **Преобразование часового пояса**: Преобразовывает метку времени из исходного часового пояса в целевой. <br>- **Текущее время**: Возвращает текущую метку времени в виде строки. <br>- **Получение будущего времени**: Возвращает текущую метку времени, а также указанные единицы времени. <br>- **Получение времени в прошлом**: Возвращает текущую метку времени, вычитая указанные единицы времени. <br>- **Вычесть из времени**: Вычитает количество единиц времени из метки времени. |
| [![Значок встроенного действия][variables-icon]<br/>**Переменные**][variables-doc] | Выполнение операций с переменными: <p>- **Добавление в переменную массива**: Вставьте значение как последний элемент в массиве, хранящиеся в переменной. <br>- **Добавление в переменную строки**: Вставьте значение в качестве последнего символа в строке, хранящиеся в переменной. <br>- **Переменная декремента**: Уменьшение переменной на значение константы. <br>- **Увеличение переменной**: Увеличьте переменную, значение константы. <br>- **Инициализируйте переменную**: Создайте переменную и объявить его тип данных и начальное значение. <br>- **Задание переменной**: Назначьте другое значение для существующей переменной. |
|  |  | 

<a name="managed-api-connectors"></a>

## <a name="managed-api-connectors"></a>Соединители управляемого API

Ниже приведены более популярные соединители для автоматизации задач, процессов и рабочих процессов с помощью этих служб или систем:

|   |   |   |   | 
|---|---|---|---| 
| [![Значок API][azure-service-bus-icon]<br/>**Служебная шина Azure**][azure-service-bus-doc] | Управление асинхронными сообщениями, сеансами и подписками на темы с помощью наиболее часто используемого соединителя в системе Logic Apps. | [![Значок API][sql-server-icon]<br/>**SQL Server**][sql-server-doc] | Подключение к SQL Server локально или в Базе данных Azure SQL в облаке для управления записями, запуска хранимых процедур или выполнения запросов. | 
| [![Значок API][office-365-outlook-icon]<br/>**Office 365<br/>Outlook**][office-365-outlook-doc] | Подключение к вашей учетной записи электронной почты Office 365 для создания и управления электронной почтой, задачами, событиями и встречами календаря, контактами, запросами и т.д. | [![Значок API][azure-blob-storage-icon]<br/>**Хранилище<br/>BLOB-объектов Azure**][azure-blob-storage-doc] | Подключение к учетной записи хранения для создания содержимого большого двоичного объекта и управления им. | 
| [![Значок API][sftp-icon]<br/>**SFTP**][sftp-doc] | Подключение к SFTP-серверам, к которым вы можете получить доступ из Интернета для работы со своими файлами и папками. | [![Значок API][sharepoint-online-icon]<br/>**SharePoint<br/>Online**][sharepoint-online-doc] | Подключение к SharePoint Online для управления файлами, вложениями, папками и т.д. | 
| [![Значок API][dynamics-365-icon]<br/>**Dynamics 365<br/>CRM Online**][dynamics-365-doc] | Подключение к учетной записи Dynamics 365 для создания записей, элементов и т.д., а также управления ими. | [![Значок API][ftp-icon]<br/>**FTP**][ftp-doc] | Подключение к FTP-серверам, к которым вы можете получить доступ из Интернета для работы со своими файлами и папками. | 
| [![Значок API][salesforce-icon]<br/>**Salesforce**][salesforce-doc] | Подключение к учетной записи Salesforce для создания таких элементов, как записи, задания, объекты и т.д., а также управления ими. | [![Значок API][twitter-icon]<br/>**Twitter**][twitter-doc] | Подключение к учетной записи Twitter для управления твитами, подписчиками, временной шкалой и т.д. Хранение твитов в SQL, Excel или SharePoint. | 
| [![Значок API][azure-event-hubs-icon]<br/>**Центры событий Azure**][azure-event-hubs-doc] | Использование событий и их публикация в концентраторе событий. Например, получите выходные данные из приложения логики с помощью соединителя Центров событий, а затем отправьте эти сведения любому поставщику аналитики в реальном времени. | [![Значок API][azure-event-grid-icon]<br/>**Сетка событий**</br>**Azure**][azure-event-grid-doc] | Мониторинг событий, публикуемых службой "Сетка событий Azure", например при изменении ресурсов Azure или сторонних ресурсов. | 
||||| 

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Локальные соединители 

Ниже приведены некоторые часто используемые соединители, обеспечивающие доступ к данным и ресурсам в локальных системах. Прежде чем создавать подключение к локальной системе, необходимо [загрузить, установить и настроить локальный шлюз данных ][gateway-doc]. Этот шлюз предоставляет безопасный коммуникационный канал без необходимости настройки обязательной сетевой инфраструктуры. 

|   |   |   |   |   | 
|---|---|---|---|---| 
| ![Значок API][biztalk-server-icon]<br/>**BizTalk**</br> **Сервер** | [![Значок API][file-system-icon]<br/>**Файловая</br> система**][file-system-doc] | [![Значок API][ibm-db2-icon]<br/>**IBM DB2**][ibm-db2-doc] | [![Значок API][ibm-informix-icon]<br/>**IBM**</br>**Informix**][ibm-informix-doc] | ![Значок API][mysql-icon]<br/>**MySQL** | 
| [![Значок API][oracle-db-icon]<br/>**Oracle Database**][oracle-db-doc] | ![Значок API][postgre-sql-icon]<br/>**PostgreSQL** | [![Значок API][sharepoint-server-icon]<br/>**SharePoint</br> Server**][sharepoint-server-doc] | [![Значок API][sql-server-icon]<br/>**SQL</br> Server**][sql-server-doc] | ![Значок API][teradata-icon]<br/>**Teradata** | 
||||| 

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Соединители для учетной записи интеграции 

Ниже приведены соединители для создания решения типа "бизнес-бизнес" (B2B) с помощью приложений логики при создании и оплате [учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), которая доступна через пакет интеграции Enterprise (EIP) в Azure. С помощью этой учетной записи вы можете создавать и хранить артефакты B2B, такие как деловые партнеры, соглашения, карты, схемы, сертификаты и т.д. Чтобы использовать эти артефакты, свяжите свои приложения логики с учетной записью интеграции. Если в настоящее время вы используете BizTalk Server, эти соединители могут быть вам знакомы.

|   |   |   |   | 
|---|---|---|---| 
| [![Значок API][as2-icon]<br/>**Декодирование</br>AS2**][as2-decode-doc] | [![Значок API][as2-icon]<br/>**Кодирование</br>AS2**][as2-encode-doc] | [![Значок API][edifact-icon]<br/>**Декодирование</br>EDIFACT**][edifact-decode-doc] | [![Значок API][edifact-icon]<br/>**Кодирование</br>EDIFACT**][edifact-encode-doc] | 
| [![Значок API][flat-file-decode-icon]<br/>**Декодирование</br>неструктурированного файла**][flat-file-decode-doc] | [![Значок API][flat-file-encode-icon]<br/>**Кодирование</br>неструктурированного файла**][flat-file-encode-doc] | [![Значок API][integration-account-icon]<br/>**Учетная запись<br/>интеграции**][integration-account-doc] | [![Значок API][liquid-icon]<br/>**Преобразование**</br>**Liquid**][json-liquid-transform-doc] | 
| [![Значок API][x12-icon]<br/>**Декодирование</br>Х12**][x12-decode-doc] | [![Значок API][x12-icon]<br/>**Кодирование</br>Х12**][x12-encode-doc] | [![Значок API][xml-transform-icon]<br/>**Преобразование**</br>**XML**][xml-transform-doc] | [![Значок API][xml-validate-icon]<br/>**Проверка<br/>XML**][xml-validate-doc] |  
||||| 

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Корпоративные соединители

Приложениям логики может быть предоставлен доступ к корпоративным системам, таким как SAP и IBM MQ:

|   |   | 
|---|---| 
| [![Значок API][ibm-mq-icon]<br/>**IBM MQ**][ibm-mq-doc] | [![Значок API][sap-icon]<br/>**SAP**][sap-connector-doc] |
||| 

<a name="triggers-actions"></a>

## <a name="triggers-and-actions---more-info"></a>Триггеры и действия — Дополнительные сведения

Соединители можно предоставить *триггеры*, *действия*, или оба. Объект *триггера* является первым шагом в любом приложении логики, обычно указание событие, которое активирует триггер и запускает выполнение приложения логики. Например соединитель FTP имеет триггер, который запускает приложение логики «при добавлении или изменении файла». Некоторые триггеры Регулярная проверка для указанного события или данные, а затем обработать при обнаружении указанного события или данные. Другие триггеры ожидания, но срабатывают мгновенной когда происходит определенное событие или когда доступны новые данные. Триггеры также передавать необходимые данные для приложения логики. Приложение логики можно читать и использовать эти данные через рабочий процесс.
Например, соединитель Twitter предоставляет триггера «При публикации нового твита», который передает твита содержимого в рабочий процесс приложения логики. 

После срабатывания триггера Azure Logic Apps создает экземпляр приложения логики и запускает выполнение *действия* в рабочем процессе приложения логики. Действия — действия, выполните триггера и выполнения задач в рабочем процессе приложения логики. Например можно создать приложение логики, которое получает данные клиентов из базы данных SQL и обработки этих данных в последующих действиях. 

Ниже приведены общие виды триггеры, которые предоставляет Azure Logic Apps.

* *Триггер повторения*: Этот триггер выполняется по указанному расписанию и не тесно связана с конкретной службы или системы.

* *Опрашивающий триггер*: Этот триггер регулярно опрашивает определенной службы или системы, на основе заданного расписания, проверка наличия новых данных или ли определенное событие произошло. Если новые данные или произошло определенное событие, триггер создает и запускает новый экземпляр приложения логики, который теперь можно использовать данные, передается в качестве входных данных.

* *Извещающий триггер*: Этот триггер ожидает и ожидает передачи данных для новых данных или же планируемое событие. Когда доступны новые данные, или когда происходит событие, триггер создает и запускает новый экземпляр приложения логики, который теперь можно использовать данные, передается в качестве входных данных.

<a name="custom"></a>

## <a name="connector-configuration"></a>Настройка соединителя

Триггеры и действия для каждого соединителя предоставляют свои собственные свойства для настройки. Множество соединителей также требуется, необходимо сначала создать *подключения* для целевой службы или системы и укажите учетные данные проверки подлинности или другие данные конфигурации, прежде чем триггер или действие, можно использовать в приложении логики. Например необходимо авторизовать подключение к учетной записи Twitter для доступа к данным или к публикации от вашего имени. 

Для соединителей, использующих OAuth создав подключение значит войти в службу, например Office 365, Salesforce или GitHub, где маркер доступа шифруется и безопасно хранится в секретном хранилище Azure. Других соединителей, таких как FTP и SQL, требуется подключение, которое содержит сведения о конфигурации, такие как адрес сервера, имя пользователя и пароль. Это сведения о конфигурации подключения также шифруются и хранятся безопасно. 

Подключения можно получить доступ к целевой службы или системы для до тех пор, пока этой службы или системы позволяет. Для служб, использующих подключений OAuth Azure Active Directory (AD), например Office 365 и Dynamics Azure Logic Apps обновляет маркеры доступа бессрочно. Другие службы могут ограничивать продолжительность Azure Logic Apps может применять маркер без обновления. Как правило некоторые действия недействительными все маркеры доступа, такие как смена пароля.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Пользовательские API и соединители

Чтобы вызывать API-интерфейсы, которые запускают пользовательский код или недоступны в качестве соединителей, можно расширить платформу Logic Apps, [создавая настраиваемые приложения API](../logic-apps/logic-apps-create-api-app.md). Кроме того, вы можете [создавать пользовательские соединители](../logic-apps/custom-connector-overview.md) для *любых* API на основе REST или SOAP, которые делают эти API доступными для любого приложения логики в вашей подписке Azure.
Чтобы пользовательские приложения API или соединители были доступными для любого пользователя в Azure, вы можете [отправить соединители для сертификации Майкрософт](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Получение поддержки

* Если у вас возникли вопросы, то посетите [форум Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Проголосовать за идеи улучшения Azure Logic Apps и соединителей или отправить собственные можно на [сайте обратной связи Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Дальнейшие действия

* Найдите [полный список соединителей](https://docs.microsoft.com/connectors).
* [Создание приложения логики](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Обзор пользовательских соединителей](https://docs.microsoft.com/connectors/custom-connectors/)
* [Создание настраиваемых API для приложений логики](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Подключение к локальным источникам данных из приложений логики с помощью локального шлюза данных"

<!--Built-ins doc links-->
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Интеграция приложений логики с Функциями Azure"
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Создание экземпляра службы управления API Azure для управления и публикации API-интерфейсов"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Интеграция приложений логики с приложениями API службы приложений."
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Отправка сообщений из очередей и разделов служебной шины, а также получение сообщений из очередей и подписок служебной шины"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Обработка сообщений в группах или в качестве пакетов"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Оценка условия и выполнение различных действий на основе условия (True или False)"
[delay-doc]: ./connectors-native-delay.md "Выполнение отложенных действий"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Выполнение одинаковых действий для каждого элемента в массиве"
[http-doc]: ./connectors-native-http.md "Вызовы HTTP с помощью соединителя HTTP"
[http-request-doc]: ./connectors-native-reqres.md "Добавление действий для HTTP-запросов и ответов в приложения логики"
[http-response-doc]: ./connectors-native-reqres.md "Добавление действий для HTTP-запросов и ответов в приложения логики"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Вызовы HTTP с помощью соединителя HTTP + Swagger"
[http-webook-doc]: ./connectors-native-webhook.md "Добавление действий и триггеров веб-перехватчика HTTP в приложения логики"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Интеграция приложений логики с вложенным рабочим процессом."
[query-doc]: ./connectors-native-query.md "Выбор и фильтрации массивов с помощью действия "Запрос""
[recurrence-doc]:  ./connectors-native-recurrence.md "Активация повторяющихся действий для приложений логики"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Упорядочение действий в группы, которые получают свой статус после окончания выполнения действий в группе" 
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Упорядочение действий в варианты, которым присваиваются уникальные значения. Выполняйте лишь тот вариант, значение которого соответствует результату выражения, объекта или токена. Если совпадений не существует, выполните вариант по умолчанию"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Остановка или отмена активного рабочего процесса для приложения логики"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Повтор действий до тех пор, пока состояние указанного условия не будет равно True или какое-нибудь из состояний не изменится"
[data-operations-doc]: ../logic-apps/logic-apps-perform-data-operations.md "Выполнение операций с данными, таких как фильтрация массивов или создание таблиц CSV и HTML"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Выполнение операций с переменными, такими как инициализация, набор, увеличение, декремент и добавление к строке или переменной массива"

<!--Managed API doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Управление файлами в контейнере больших двоичных объектов с помощью соединителя хранилища BLOB-объектов Azure"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Мониторинг событий, публикуемых службою "Сетка событий Azure", например, при изменении ресурсов Azure или сторонних ресурсов"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Подключение к Центрам событий Azure. Receive and send events between logic apps and Event Hubs" (Подключение к Центрам событий Azure. Получение и отправка событий между приложением логики и Центрами событий)
[box-doc]: ./connectors-create-api-box.md "Подключение к Box. Отправка, получение, удаление файлов, отображение их списка и другие действия"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Подключение к Dropbox. Отправка, получение, удаление файлов, отображение их списка и другие действия"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Подключение к Dynamics CRM Online для работы с данными CRM Online"
[facebook-doc]: ./connectors-create-api-facebook.md "Подключение к Facebook. Публикация на стене, получение канала страниц и другие действия"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Подключение к локальной файловой системе"
[ftp-doc]: ./connectors-create-api-ftp.md "Подключение к FTP и FTPS-серверу для выполнения разных FTP-задач, включая отправку, получение, удаление файлов и другие действия."
[github-doc]: ./connectors-create-api-github.md "Подключение к GitHub и отслеживание проблем"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Подключается к Google Календарь и может управлять календарем."
[google-drive-doc]: ./connectors-create-api-googledrive.md "Подключение к Google Диску для работы с данными"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Подключение к Таблицам Google для редактирования таблиц"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Подключение к Задачам Google для управления задачами"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Подключение к IBM DB2 в облаке или локальной среде. Обновление строки, таблицы и другие действия"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Подключение к Informix в облаке или локальной среде. Чтение строк, отображение таблиц и другие действия"
[ibm-mq-doc]: ./connectors-create-api-mq.md "Подключение к IBM MQ в локальной среде или Azure для отправки и получения сообщений"
[instagram-doc]: ./connectors-create-api-instagram.md "Подключение к Instagram. Активация событий и реакция на них"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Подключения к учетной записи MailChimp. Управление сообщениями электронной почты и их автоматизация"
[mandrill-doc]: ./connectors-create-api-mandrill.md "Подключение к Mandrill для обмена данными"
[microsoft-translator-doc]: ./connectors-create-api-microsofttranslator.md "Подключение к Microsoft Translator. Перевод текста, обнаружение языков и другие действия" 
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Подключение к учетной записи Office 365. Отправка и получение сообщений, управление календарем и контактами, а также другие действия"
[office-365-users-doc]: ./connectors-create-api-office365-users.md 
[office-365-video-doc]: ./connectors-create-api-office365-video.md "Получение сведений о видео, списков и каналов видео, а также URL-адресов воспроизведения для видео Office 365"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Подключение к личному Microsoft OneDrive. Отправка, удаление, отображение файлов и другие действия"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Подключение к рабочему Microsoft OneDrive. Отправка, удаление, отображение файлов и другие действия"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Connect to an Oracle database to add, insert, delete rows, and more" (Подключение к базе данных Oracle для добавления, вставки, удаления строк и других действий)
[outlook.com-doc]: ./connectors-create-api-outlook.md "Подключение к почтовому ящику Outlook. Управление электронной почтой, календарями, контактами и другие действия"
[project-online-doc]: ./connectors-create-api-projectonline.md "Подключение к Microsoft Project Online. Управление проектами, задачами, ресурсами и другие действия"
[rss-doc]: ./connectors-create-api-rss.md "Публикация и получение элементов канала, активация операций при публикации нового элемента в RSS-канале."
[salesforce-doc]: ./connectors-create-api-salesforce.md "Подключение к учетной записи Salesforce. Управление учетными записями, потенциальными клиентами, возможностями и другие действия"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Подключение к локальному серверу SAP"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Подключение к SendGrid. Отправка электронных сообщений и управление списками получателей"
[sftp-doc]: ./connectors-create-api-sftp.md "Подключение к учетной записи SFTP. Отправка, получение, удаление файлов и другие действия"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Подключение к локальному серверу SharePoint Управление документами, элементами списка и другие действия"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Подключение к SharePoint Online. Управление документами, элементами списка и другие действия"
[slack-doc]: ./connectors-create-api-slack.md "Подключение к Slack и публикация сообщений в каналах Slack"
[smtp-doc]: ./connectors-create-api-smtp.md "Подключение к SMTP-серверу и отправка электронных сообщений с вложениями"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Подключение к SparkPost для обмена данными"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Подключение к Базе данных SQL Azure или серверу SQL. Создание, обновление, получение и удаление записей в таблице базы данных SQL."
[trello-doc]: ./connectors-create-api-trello.md "Подключение к Trello. Управление проектами и организация всего и всех"
[twilio-doc]: ./connectors-create-api-twilio.md "Подключение к Twilio. Отправка и получение сообщений, получение доступных номеров, управление входящими телефонными номерами и выполнение других задач"
[twitter-doc]: ./connectors-create-api-twitter.md "Подключение к Twitter. Получение обновлений, публикация твитов и выполнение других задач"
[wunderlist-doc]: ./connectors-create-api-wunderlist.md "Подключение к Wunderlist. Управление задачами и списками дел и их синхронизация, а также другие действия"
[yammer-doc]: ./connectors-create-api-yammer.md "Подключение к Yammer. Публикация сообщений, получение новых сообщений и другие действия"
[youtube-doc]: ./connectors-create-api-youtube.md "Подключение к YouTube. Управление видео и каналами"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Дополнительные сведения об AS2 в рамках корпоративной интеграции."
[as2-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Дополнительные сведения о декодировании AS2 в рамках корпоративной интеграции."
[as2-encode-doc]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Дополнительные сведения о кодировке AS2 в рамках корпоративной интеграции."
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Дополнительные сведения о декодировании EDIFACT в рамках корпоративной интеграции."
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Дополнительные сведения о кодировке EDIFACT в рамках корпоративной интеграции."
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Дополнительные сведения о неструктурированном файле в рамках корпоративной интеграции."
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Дополнительные сведения о неструктурированном файле в рамках корпоративной интеграции."
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Поиск схем, карт, партнеров и другого в вашей учетной записи интеграции"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Дополнительные сведения о преобразованиях JSON с помощью Liquid"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Дополнительные сведения об X12 в рамках корпоративной интеграции."
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Дополнительные сведения о декодировании X12 в рамках корпоративной интеграции."
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Дополнительные сведения о кодировке X12 в рамках корпоративной интеграции."
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Дополнительные сведения о преобразованиях в рамках корпоративной интеграции."
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Дополнительные сведения о проверке XML в рамках корпоративной интеграции."

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed API icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[microsoft-translator-icon]: ./media/apis-list/microsoft-translator.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[office-365-video-icon]: ./media/apis-list/office-365-video.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[wunderlist-icon]: ./media/apis-list/wunderlist.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png
