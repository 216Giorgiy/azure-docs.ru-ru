---
title: Установка Azure AD Connect с использованием делегированных разрешений администратора SQL | Документация Майкрософт
description: В этой статье описывается обновление для Azure AD Connect, которое позволяет использовать при установке учетную запись, которая имеет только разрешения владельца базы данных SQL.
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: jparsons
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38ad75d22d21a141d48e9664ae580dfb5577a389
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56184930"
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>Установка Azure AD Connect с использованием делегированных разрешений администратора SQL
До последней сборки Azure AD Connect административное делегирование при развертывании конфигураций, для которых требовался SQL, не поддерживалось.  Пользователям, которые хотят установить Azure AD Connect, необходимо иметь разрешения администратора сервера на сервере SQL.

В последнем выпуске Azure AD Connect внешнюю подготовку базы данных может выполнять администратор SQL, а установку — администратор Azure AD Connect с правами владельца базы данных.

## <a name="before-you-begin"></a>Перед началом работы
Чтобы использовать эту функцию, необходимо понимать, что имеется несколько перемещаемых частей, для каждой из которых требуется отдельный администратор в вашей организации.  В указанной ниже таблице приведены отдельные роли и их соответствующие обязанности при развертывании Azure AD Connect с помощью этой функции.

|Роль|ОПИСАНИЕ|
|-----|-----|
|Администратор домена или леса AD|Создает учетную запись службы на уровне домена, используемую Azure AD Connect для запуска службы синхронизации.  Дополнительные сведения об учетных записях службы см. в статье [Azure AD Connect: учетные записи и разрешения](reference-connect-accounts-permissions.md).
|Администратор SQL|Создает базу данных ADSync и предоставляет доступ для входа и доступ владельца базы данных администратору Azure AD Connect и учетной записи службы, созданной администратором домена или леса.|
Администратор Azure AD Connect|Устанавливает Azure AD Connect и указывает учетную запись службы во время выборочной установки.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>Инструкции по установке Azure AD Connect с использованием делегированных разрешений SQL
Выполните указанные ниже действия, чтобы подготовить внешнюю базу данных и установить Azure AD Connect с разрешениями владельца базы данных.

>[!NOTE]
>Хотя это не обязательно, **настоятельно рекомендуется** выбрать параметры сортировки Latin1_General_CI_AS при создании базы данных.


1.  При помощи администратора SQL создайте базу данных ADSync с последовательностью параметров сортировки без учета регистра **(Latin1_General_CI_AS)**.  Имя базы данных должно быть **ADSync**.  Модель восстановления, уровень совместимости и тип вложения обновляются до правильных значений при установке Azure AD Connect.  Однако администратор SQL должен правильно настроить последовательность параметров сортировки. В противном случае Azure AD Connect заблокирует установку.  Чтобы восстановить установку, администратор сервера должен удалить и повторно создать базу данных.</br>
![Параметры сортировки](./media/how-to-connect-install-sql-delegation/sql4.png)
2.  Предоставьте администратору Azure AD Connect и учетной записи службы домена следующие разрешения:
    - Имя для входа в SQL 
    - Права **владельца базы данных (dbo)**.  </br>
![Разрешения](./media/how-to-connect-install-sql-delegation/sql3a.png)
3.  Отправьте электронное сообщение администратору Azure AD Connect, указав сервер и имя экземпляра SQL, которые должны использоваться при установке Azure AD Connect.

## <a name="additional-information"></a>Дополнительная информация
После подготовки базы данных администратор Azure AD Connect может для удобства установить и настроить службу локальной синхронизации.  

Дополнительные сведения об установке Azure AD Connect с помощью имеющейся базы данных ADSync см. в [этой статье](how-to-connect-install-existing-database.md).

## <a name="next-steps"></a>Дополнительная информация
- [Приступая к работе с Azure AD Connect с использованием стандартных параметров](how-to-connect-install-express.md)
- [Выборочная установка Azure AD Connect](how-to-connect-install-custom.md)
- [Установка Azure AD Connect с помощью имеющейся базы данных ADSync](how-to-connect-install-existing-database.md)  
