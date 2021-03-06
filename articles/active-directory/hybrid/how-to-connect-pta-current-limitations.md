---
title: 'Azure AD Connect выполняет следующие функции: Текущие ограничения сквозной аутентификации | Документация Майкрософт'
description: Эта статья содержит сведения о текущих ограничениях сквозной аутентификации Azure Active Directory (Azure AD).
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
ms.date: 09/04/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97dc67d46b08bf5765c59806b45edd82f38720cd
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58011969"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Сквозная проверка подлинности Azure Active Directory Текущие ограничения

>[!IMPORTANT]
>Функция сквозной аутентификации Azure Active Directory (Azure AD) является бесплатной, и для ее использования не требуются платные выпуски Azure AD. Сквозная аутентификация доступна только в доступном по всему миру экземпляре Azure AD, но не в [облаке Microsoft Azure — Германия](https://www.microsoft.de/cloud-deutschland) или [облаке Microsoft Azure для государственных организаций](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Поддерживаемые сценарии использования.

Следующие сценарии поддерживаются:

- Вход пользователей в браузерные приложения.
- Вход пользователей в клиенты Outlook с помощью устаревших протоколов, таких как Exchange ActiveSync, EAS, SMTP, POP и IMAP.
- Вход пользователей в устаревшие клиентские приложения Office и приложения Office, поддерживающие [современную аутентификацию](https://www.microsoft.com/en-us/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview): Office 2013 и 2016 версий.
- Вход пользователей в приложения на основе устаревших протоколов, такие как PowerShell версии 1.0 и пр.
- Присоединение устройств Windows 10 к Azure AD.
- Добавление паролей для Многофакторной идентификации.

## <a name="unsupported-scenarios"></a>Неподдерживаемые сценарии

Следующие сценарии _не_ поддерживаются:

- Определение пользователей с [утерянными учетными данными](../reports-monitoring/concept-risk-events.md#leaked-credentials).
- Для использования доменных служб Azure AD в клиенте нужно включить синхронизацию хэшей паролей. Поэтому клиенты, использующие _только_ сквозную аутентификацию, не поддерживаются для сценариев, в которых требуются доменные службы Azure AD.
- Сквозная аутентификация не интегрирована с [Azure AD Connect Health](whatis-hybrid-identity-health.md).

> [!IMPORTANT]
> _Исключительно_ в качестве временного решения для неподдерживаемых сценариев (кроме интеграции Azure AD Connect Health) включите синхронизацию хэшей паролей на странице [Дополнительные возможности](how-to-connect-install-custom.md#optional-features) в мастере Azure AD Connect.
> 
> [!NOTE]
> Включение синхронизации хэшей паролей позволяет выполнять отработку отказа для аутентификации при полном сбое локальной инфраструктуры. Отработка отказа сквозной аутентификации с переходом на синхронизацию хэшей паролей не выполняется автоматически. Необходимо вручную переключить метод входа с помощью Azure AD Connect. Если сервер под управлением Azure AD Connect вышел из строя, потребуется помощь службы поддержки Майкрософт, чтобы отключить сквозную аутентификацию.

## <a name="next-steps"></a>Дальнейшие действия
- [Быстрый запуск](how-to-connect-pta-quick-start.md). Настройка и подготовка к работе с помощью сквозной аутентификации Azure Active Directory.
- [Migrate from AD FS to Pass-through Authentication](https://aka.ms/ADFSTOPTADPDownload) (Переход с AD FS на сквозную проверку подлинности). Подробное руководство по переходу с AD FS (или других технологии федерации) на сквозную проверку подлинности.
- [Интеллектуальная блокировка](../authentication/howto-password-smart-lockout.md). Узнайте, как настроить возможность интеллектуальной блокировки в клиенте для защиты учетных записей пользователей.
- [Техническое руководство](how-to-connect-pta-how-it-works.md). Узнайте, как работает функция сквозной аутентификации.
- [Часто задаваемые вопросы](how-to-connect-pta-faq.md). Найдите ответы на часто задаваемые вопросы о сквозной аутентификации.
- [Устранение неполадок](tshoot-connect-pass-through-authentication.md). Узнайте, как устранять распространенные неполадки со сквозной аутентификацией.
- [Руководство по безопасности](how-to-connect-pta-security-deep-dive.md). Получите детальные технические сведения о компоненте сквозной проверки подлинности.
- [Простой единый вход Azure AD](how-to-connect-sso.md). Сведения об этой дополнительной функции.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect). Оставить запрос на новые функции можно на форуме по Azure Active Directory.
