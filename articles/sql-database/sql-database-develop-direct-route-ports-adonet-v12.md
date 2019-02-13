---
title: Порты для базы данных SQL помимо 1433 | Документация Майкрософт
description: Взаимодействие с базой данных при клиентских подключениях из ADO.NET к Базе данных SQL Azure может происходить напрямую, без прокси-сервера, если база данных не использует порт 1433.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: b6fbb71a827c90abd1fac58d7975ab2f7b2a5674
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2019
ms.locfileid: "55560895"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Порты для ADO.NET 4.5, отличные от порта 1433

В этом разделе описывается поведение подключения к Базе данных SQL Azure клиентов, использующих ADO.NET 4.5 или более поздней версии. 

> [!IMPORTANT]
> Сведения об архитектуре подключения см. в статье [Azure SQL Database Connectivity Architecture](sql-database-connectivity-architecture.md) (Архитектура подключения базы данных SQL Azure).
>

## <a name="outside-vs-inside"></a>Снаружи или внутри

В случае подключения к Базе данных SQL Azure сначала нужно узнать, запускается ли клиентская программа *за пределами* или *в пределах* границ облака Azure. В подразделах рассматриваются два типичных сценария.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Внешняя программа*. Клиент выполняется на настольном компьютере

Порт 1433 — единственный порт, который должен быть открыт на компьютере, где размещено ваше клиентское приложение базы данных SQL.

#### <a name="inside-client-runs-on-azure"></a>*Внутренняя программа*. Клиент выполняется в Azure

При запуске внутри границ облака Azure для взаимодействия с сервером Базы данных SQL клиент использует то, что можно назвать *прямым маршрутом* . После установления подключения дальнейшее взаимодействие между клиентом и базой данных не включает в себя шлюз базы данных SQL Azure.

Последовательность выглядит так:

1. ADO.NET 4.5 (или более поздней версии) инициирует краткое взаимодействие с облаком Azure и получает динамически указанный номер порта.
   
   * Диапазон для динамического назначения номера порта: 11000–11999 или 14000–14999.
2. Затем ADO.NET подключается к серверу базы данных SQL напрямую, с без промежуточного слоя.
3. Запросы отправляются непосредственно в базу данных, а результаты возвращаются клиенту.

Убедитесь, что диапазоны портов 11000–11999 и 14000–14999 на клиентском компьютере Azure доступны для взаимодействия клиента ADO.NET 4.5 с Базой данных SQL.

* В частности, порты в этом диапазоне должны оставаться свободными от других исходящих ошибок.
* На виртуальной машине Azure параметрами порта управляет **брандмауэр Windows в режиме повышенной безопасности** .
  
  * С помощью [пользовательского интерфейса брандмауэра](https://msdn.microsoft.com/library/cc646023.aspx) можно добавить правило, для которого указывается протокол **TCP**, а также диапазон портов, используя следующий синтаксис: **11000–11999**.

## <a name="version-clarifications"></a>Уточнение версии
В этом разделе описываются псевдонимы, относящиеся к версиям продуктов. В нем также перечисляются некоторые пары версий продуктов.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0 поддерживает протокол TDS 7.3, но не 7.4.
* ADO.NET 4.5 и более поздних версий поддерживает протокол TDS 7.4.

#### <a name="odbc"></a>ODBC
* Microsoft SQL Server ODBC 11 или более поздней версии

#### <a name="jdbc"></a>JDBC
* Microsoft SQL Server JDBC 4.2 или более поздней версии (JDBC 4.0 поддерживает TDS 7.4, но не выполняет перенаправление)


## <a name="related-links"></a>Связанные ссылки
* 20 июля 2015 г. был выпущен ADO.NET 4.6. Объявление в блоге группы разработчиков для .NET доступно [здесь](https://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* 15 августа 2012 г. был выпущен ADO.NET 4.5. Объявление в блоге группы разработчиков для .NET доступно [здесь](https://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx). 
  * Запись блога об ADO.NET 4.5.1 доступна [здесь](https://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).

* Microsoft® ODBC Driver 17 for SQL Server® — Windows, Linux и macOS https://www.microsoft.com/download/details.aspx?id=56567

* Подключение к базе данных SQL Azure версии 12 с помощью перенаправления https://blogs.msdn.microsoft.com/sqlcat/2016/09/08/connect-to-azure-sql-database-v12-via-redirection/

* [Список версий протокола TDS](http://www.freetds.org/userguide/tdshistory.htm)
* [Общие сведения о разработке базы данных SQL](sql-database-develop-overview.md)
* [Брандмауэр базы данных SQL Azure](sql-database-firewall-configure.md)
* [How to: Настройка правил брандмауэра в Базе данных SQL](sql-database-configure-firewall-settings.md)


