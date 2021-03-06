---
title: Служба мобильных веб-приложения сервера Azure MFA — Azure Active Directory
description: Настройка сервера Многофакторной идентификации для отправки push-уведомлений пользователям с помощью приложения Microsoft Authenticator.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac59b68f1ddda695fba8f1a4ceacb90bfa4102a2
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313708"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Включение аутентификации мобильных приложений с помощью сервера Многофакторной идентификации Azure

Приложение Microsoft Authenticator предлагает возможность дополнительной внешней проверки. Вместо автоматического телефонного вызова или отправки SMS пользователю при входе в систему служба Многофакторной идентификации Azure отправляет push-уведомление в приложение Microsoft Authenticator, которое установлено на смартфоне или планшете пользователя. Чтобы выполнить вход, пользователю в приложении нужно просто коснуться элемента **Проверить** (или ввести ПИН-код и коснуться элемента "Проверить подлинность").

Использование мобильного приложения для двухфакторной проверки используется является предпочтительным, когда телефонная связь ненадежна. При использовании такого приложения, как генератор OATH-токенов, не требуется подключение к сети или Интернету.

> [!IMPORTANT]
> Если вы установили сервер Многофакторной идентификации Microsoft Azure 8.x или более поздней версии, большинство приведенных ниже действий не требуются. Аутентификацию мобильного приложения можно настроить, следуя инструкциям в разделе [Настройка мобильного приложения](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server).

## <a name="requirements"></a>Требования

Для использования приложения Microsoft Authenticator необходим сервер Многофакторной идентификации Microsoft Azure 8.x или более поздней версии.

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Настроить параметры мобильного приложения на сервере Многофакторной идентификации Azure.

1. В консоли сервера Многофакторной идентификации щелкните значок пользовательского портала. Если пользователям разрешено управлять своими методами аутентификации, на вкладке "Параметры" в группе **Разрешить пользователям выбирать метод** установите флажок **Мобильное приложение**. Если не включить эту функцию, пользователям придется обращаться в службу поддержки для завершения активации мобильного приложения.
2. Установите флажок **Разрешить пользователям активировать мобильное приложение**.
3. Установите флажок **Разрешить регистрацию пользователей**.
4. Щелкните значок **мобильного приложения**.
5. В поле **Имя учетной записи** введите название компании или организации, которое будет отображаться в мобильном приложений для этой учетной записи.
   ![Настройка сервера MFA, параметры для мобильного приложения](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Расширенные сценарии с использованием Многофакторной идентификации Azure и VPN сторонних поставщиков](howto-mfaserver-nps-vpn.md).
