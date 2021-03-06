---
title: Что такое проверки доступа Azure Active Directory | Документы Майкрософт
description: С помощью проверок доступа Azure Active Directory, можно управлять группы членства и доступом к приложениям в соответствии с управление, управление рисками и соответствию в вашей организации.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 01/18/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1563a023f397999deb5c6abd40843d6a376b0492
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576128"
---
# <a name="what-are-azure-ad-access-reviews"></a>Что такое доступ к Azure AD проверяет?

Проверки доступа Azure Active Directory (Azure AD) позволяют организациям эффективно управлять членством, доступ к корпоративным приложениям и назначения ролей. Доступ пользователей можно проверять на регулярной основе, чтобы только у авторизованных пользователей был постоянный доступ.

Вот видео, в котором представлен краткий обзор проверки доступа:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Почему важны проверки доступа

Azure AD предоставляет возможность совместной работы внутри вашей организации и с пользователями из внешних организаций, таких как партнерские. Пользователи могут присоединяться к группам, приглашать гостей, подключаться к облачным приложениям и удаленно работать со своих рабочих или личных устройств. Удобство и эффективность самообслуживания привели к потребности в более широких возможностях управления доступом.

- Как обеспечить новым сотрудникам нужный доступ, чтобы они работали максимально производительно?
- Люди перемещаются между командами и уходят из компании. Как обеспечить отмену доступа, особенно гостевого, когда он больше не требуется?
- Чрезмерные права доступа могут привести к неблагоприятным результатам аудита, так как укажут на отсутствие контроля доступа.
- Вам нужно заблаговременно обратиться к владельцам ресурсов и убедиться, что они регулярно проверяют, у кого есть доступ к их ресурсам.

## <a name="when-to-use-access-reviews"></a>Когда нужно проверять доступ

- **Когда слишком много пользователей обладают привилегированными ролями.** Он имеет смысл проверить, сколько пользователей есть административный доступ, сколько из них не Глобальные администраторы, и если существуют какие-либо приглашенных гостей или партнеров, которые не были удалены после назначения для выполнения задачи администрирования. Можно повторно сертифицировать назначение ролей пользователей в [ролей Azure AD](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) такие как глобальные Администраторы или [ролей ресурсов Azure](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) например администратор доступа пользователей в [привилегированными пользователями Azure AD IDENTITY Management (PIM)](../privileged-identity-management/pim-configure.md) столкнуться.
- **Когда автоматизация невозможна.** Предположим, вы создали правила для динамического членства в группах безопасности или группах Office 365. Но что делать, если данные отдела кадров не хранятся в Azure AD или если пользователям все равно требуется доступ после выхода из группы, чтобы обучать пришедших им на замену сотрудников? Можно создать проверку для этой группы, чтобы убедиться, что доступ есть у пользователей, которым он по-прежнему необходим.
- **Когда группа используется в новых целях.** Если у вас есть группа, которая будет синхронизироваться с Azure AD или если вы планируете включить приложение Salesforce для всех пользователей в группе продаж, будет полезно попросить владельца группы проверить членство в группе, прежде чем она будет использоваться для другого содержимого.
- **Доступ к данным, критически важным для бизнеса.** Для определенных ресурсов может потребоваться регулярная подпись сотрудников, не связанных с ИТ, и обоснование необходимости доступа для целей аудита.
- **Когда нужно поддерживать список исключений политики.** В идеальном мире все пользователи выполняют политики доступа для защиты доступа к ресурсам организации. Но иногда в интересах бизнеса приходится делать исключения. ИТ-администраторы могут управлять этой задачей, чтобы не просмотреть исключения политик и предоставить аудиторам доказательства регулярной проверки этих исключений.
- **Попросите владельцев группы подтвердить, что им все еще нужны гости в группах.** Доступом сотрудников может автоматизировать некоторые на локальном компьютере IAM, но не приглашенных гостей. Если группа предоставляет гостям доступ к конфиденциальному бизнес-содержимому, то владелец группы должен подтвердить, что гостям по-прежнему на законных основаниях требуется такой доступ для бизнеса.
- **Периодически повторяйте проверки.** Вы можете настроить повторяющиеся проверки доступа пользователей с заданной частотой (например, еженедельно, ежемесячно, ежеквартально или ежегодно). Перед каждой новой проверкой рецензентам будет отправляться соответствующее уведомление. Рецензенты могут подтверждать или отклонять доступ с помощью удобного интерфейса и смарт-рекомендаций.

## <a name="where-do-you-create-reviews"></a>Где создавать проверки

В зависимости от того, что вы хотите просмотреть, вы создадите проверки доступа в Azure AD доступ к обзоры, корпоративных приложений Azure AD (в предварительной версии) или Azure AD PIM.

| Права доступа пользователей | Типы рецензентов | Где создана проверка | Работа рецензентов |
| --- | --- | --- | --- |
| Члены группы безопасности</br>Члены группы Office | Указанные рецензенты</br>Владельцы группы</br>Самостоятельная проверка | Проверки доступа Azure AD</br>Группы Azure AD | Панель доступа |
| Назначенные для подключенного приложения | Указанные рецензенты</br>Самостоятельная проверка | Проверки доступа Azure AD</br>Корпоративные приложения Azure AD (в предварительной версии) | Панель доступа |
| Роль Azure AD | Указанные рецензенты</br>Самостоятельная проверка | Azure AD PIM | Портал Azure |
| Роль ресурса Azure | Указанные рецензенты</br>Самостоятельная проверка | Azure AD PIM | Портал Azure |

## <a name="prerequisites"></a>Технические условия

Чтобы использовать проверки доступа, нужно наличие одной из следующих лицензий:

- Azure AD Premium P2
- лицензия Enterprise Mobility + Security (EMS) E5.

Дополнительные сведения см. в статье [Регистрация для работы с выпусками Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) или на странице [пробной версии Enterprise Mobility + Security E5](https://aka.ms/emse5trial).

## <a name="get-started-with-access-reviews"></a>Начало проверок доступа

Дополнительные сведения о создании и выполнении проверок доступа см. в коротком ролике:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Если вы готовы к развертыванию проверок доступа в своей организации, выполните указанные в видео шаги для подключения, а также обучения администраторов, затем создайте первую проверку доступа!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="enable-access-reviews"></a>Включение проверок доступа

Чтобы включить проверки доступа, выполните указанные ниже шаги.

1. Как глобальный администратор или администратор пользователей, войдите [портала Azure](https://portal.azure.com) где вы хотите использовать доступ проверок.

1. Нажмите **Все службы** и найдите службу проверок доступа.

1. Нажмите кнопку **проверок доступа**.

    ![Все службы - проверки доступа](./media/access-reviews-overview/all-services-access-reviews.png)

1. В списке навигации выберите **Присоединить**, чтобы открыть страницу **Подключение проверок доступа**.

    ![Подключение проверок доступа](./media/access-reviews-overview/onboard-button.png)

1. Нажмите **Создать** для включения проверок доступа в текущем каталоге.

    ![Подключение проверок доступа](./media/access-reviews-overview/onboard-access-reviews.png)

    Проверки при следующем запуске доступа, станут доступны параметры проверки доступа.

    ![Проверки доступа включено](./media/access-reviews-overview/access-reviews-enabled.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Создание проверки доступа групп или приложений](create-access-review.md)
- [Создание проверки доступа для пользователей в роли администратора Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Проверка доступа к группам или приложениям](perform-access-review.md)
- [Завершение проверки доступа групп или приложений](complete-access-review.md)
