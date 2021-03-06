---
title: 'Azure AD Connect выполняет следующие функции: Сквозная аутентификация Azure AD Connect — принцип работы | Документация Майкрософт'
description: В этой статье описывается процедура сквозной аутентификации Azure Active Directory.
services: active-directory
keywords: сквозная проверка подлинности azure ad connect, установка active directory, необходимые компоненты для azure ad, единый вход
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/19/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 59cd52dbdf6c13900cde592aeb52d8bf9abf850f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56174033"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Сквозная проверка подлинности Azure Active Directory Подробное техническое руководство
Эта статья содержит обзор принципов работы сквозной аутентификации Azure Active Directory (Azure AD). Подробные технические сведения и информацию о безопасности см. в [руководстве по безопасности](how-to-connect-pta-security-deep-dive.md).

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Как работает сквозная аутентификация Azure Active Directory?

>[!NOTE]
>В качестве предварительного требования для сквозной проверки подлинности пользователей необходимо подготовить для Azure AD в локальной службе Active Directory с помощью Azure AD Connect. Сквозная проверка подлинности не применяется к исключительно облачным пользователям.

Когда пользователь пытается войти в приложение, защищенное с помощью Azure AD, и на клиенте включена сквозная аутентификация, выполняются следующие действия.

1. Пользователь пытается получить доступ к приложению, например [Outlook Web App](https://outlook.office365.com/owa/).
2. Если пользователь еще не выполнил вход, он перенаправляется на страницу **Вход пользователя** Azure AD.
3. Пользователь вводит имя пользователя на странице входа Azure AD и нажимает кнопку **Далее**.
4. Пользователь вводит пароль на странице входа Azure AD и нажимает кнопку **Войти**.
5. Служба Azure AD, получив запрос на вход, помещает имя пользователя и пароль (зашифрованный с помощью открытого ключа или агентов аутентификации) в очередь.
6. Локальный агент проверки подлинности получает имя пользователя и зашифрованный пароль из очереди. Обратите внимание, что агент не часто опрашивает запросы из очереди, но извлекает запросы через готовое постоянное подключение.
7. Агент расшифровывает пароль, используя свой закрытый ключ.
8. Затем агент проверяет имя пользователя и пароль в Active Directory, используя стандартные интерфейсы API Windows (механизм, похожий на используемый службами федерации Active Directory (AD FS)). Именем пользователя может быть либо локальное имя пользователя по умолчанию (обычно это `userPrincipalName`), либо другой атрибут (известный как `Alternate ID`), настроенный в Azure AD Connect.
9. Локальный контроллер домена Active Directory анализирует запрос и возвращает агенту соответствующий ответ ("успешно", "ошибка", "срок действия пароля истек" или "пользователь заблокирован").
10. Агент проверки подлинности, в свою очередь, возвращает этот ответ в Azure AD.
11. Azure AD оценивает этот ответ и соответствующим образом отвечает пользователю. Например, Azure AD либо сразу же выполняет вход пользователя, либо запрашивает Многофакторную идентификацию Azure.
12. После успешного входа в систему пользователь может получить доступ к приложению.

На схеме ниже показаны все соответствующие компоненты и шаги.

![Сквозная проверка подлинности](./media/how-to-connect-pta-how-it-works/pta2.png)

## <a name="next-steps"></a>Дополнительная информация
- [Текущие ограничения](how-to-connect-pta-current-limitations.md). Узнайте, какие сценарии поддерживаются, а какие нет.
- [Краткое руководство](how-to-connect-pta-quick-start.md). Настройка и подготовка к работе сквозной аутентификации Azure Active Directory.
- [Migrate from AD FS to Pass-through Authentication](https://aka.ms/adfstoPTADP) (Переход с AD FS на сквозную проверку подлинности). Подробное руководство по переходу с AD FS (или других технологии федерации) на сквозную проверку подлинности.
- [Смарт-блокировка](../authentication/howto-password-smart-lockout.md). Настройка функций смарт-блокировки на клиенте для защиты учетных записей пользователей.
- [Часто задаваемые вопросы](how-to-connect-pta-faq.md). Ответы на часто задаваемые вопросы.
- [Устранение неполадок](tshoot-connect-pass-through-authentication.md). Способы устранения распространенных неполадок со сквозной аутентификацией.
- [Руководство по безопасности](how-to-connect-pta-security-deep-dive.md). Получите детальные технические сведения о компоненте сквозной проверки подлинности.
- [Простой единый вход Azure AD](how-to-connect-sso.md). Сведения об этой дополнительной функции.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect). Оставить запрос на новые функции можно на форуме по Azure Active Directory.

