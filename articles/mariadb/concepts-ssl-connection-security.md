---
title: Возможность подключения SSL в базе данных Azure для MariaDB
description: Сведения о настройке базы данных Azure для MariaDB и связанных приложений для правильного использования SSL-соединений
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 15bb28846b3409dd31bcdf8d42990facc94fd06d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539608"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>Возможность подключения SSL в базе данных Azure для MariaDB
База данных Azure для MariaDB поддерживает подключение сервера базы данных к клиентским приложениям с помощью SSL (Secure Sockets Layer). Применение SSL-соединений между сервером базы данных и клиентскими приложениями обеспечивает защиту от атак "злоумышленник в середине" за счет шифрования потока данных между сервером и приложением.

## <a name="default-settings"></a>Параметры по умолчанию
По умолчанию в службе базы данных должно быть настроено обязательное использование SSL-соединений при подключении к MariaDB.  Мы рекомендуем не отключать параметр SSL без необходимости.

При подготовке нового сервера базы данных Azure для MariaDB с помощью портала Azure и интерфейса командной строки применение SSL-соединений включается по умолчанию.

Строки подключения для различных языков программирования отображаются на портале Azure. Они включают необходимые параметры SSL для подключения к базе данных. На портале Azure выберите свой сервер. В разделе **Параметры** выберите **Строки подключения**. Значение параметра SSL зависит от соединителя. Например, может использоваться "ssl=true", "sslmode=require", "sslmode=required" или другой вариант.

Чтобы узнать, как включить или отключить SSL-соединение при разработке приложения, ознакомьтесь со [статьей, посвященной настройке SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Дополнительная информация
- Дополнительные сведения о [правилах брандмауэра сервера](concepts-firewall-rules.md)
- Дополнительные сведения о [настройке SSL](howto-configure-ssl.md)
