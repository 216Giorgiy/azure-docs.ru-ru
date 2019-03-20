---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: fcd71f74e0b00934958828024094773e42496b66
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2018
ms.locfileid: "58114366"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Чтобы зарегистрировать веб-API, используйте настройки, указанные в таблице.

![Примеры настроек регистрации для нового веб-API](./media/active-directory-b2c-register-web-api/b2c-new-web-api-settings.png)

| Параметр      | Образец значения  | ОПИСАНИЕ                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Имя** | Contoso B2C API | Введите **имя** приложения, которое будет служить описанием API для пользователей. | 
| **Включить веб-приложение или веб-интерфейс API** | Yes | Выберите **Да** для веб-API. |
| **Разрешить неявный поток** | Yes | Выберите **Да**, если приложение использует [вход через OpenID Connect](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md). |
| **URL-адрес ответа** | `https://localhost:44316/` | URL-адреса ответа — это конечные точки, куда Azure AD B2C возвращает все токены, запрашиваемые вашим приложением. В этом примере используется локальный веб-API, который ожидает передачу данных через порт 44316. |
| **URI кода приложения** | api | URI кода приложения — это идентификатор, используемый для веб-API. Полный URI код с доменом создается автоматически. |

Чтобы зарегистрировать приложение, щелкните **Создать** .

Только что зарегистрированное приложение отобразится в списке приложений для клиента B2C. Выберите веб-API в списке. Откроется панель свойств API-интерфейса.

![Свойства веб-API](./media/active-directory-b2c-register-web-api/b2c-web-api-properties.png)

Запишите глобальный уникальный **идентификатор для клиента приложения**. Этот идентификатор понадобится для кода приложения.