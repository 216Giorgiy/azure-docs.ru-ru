---
title: Отключение проверки адреса электронной почты во время регистрации потребителя в Azure Active Directory B2C | Документация Майкрософт
description: В этой статье демонстрируется, как отключить проверку адреса электронной почты во время регистрации потребителя в Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: e139f40e9724840f3dad4dc9f0b4d5317a75ebd4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55172431"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Отключение проверки адреса электронной почты во время регистрации потребителя в Azure Active Directory B2C 
Когда этот параметр включен, служба Azure Active Directory (Azure AD) B2C предоставляет потребителю возможность зарегистрироваться для получения доступа к приложениям, предоставив адрес электронной почты и создав локальную учетную запись. Чтобы убедиться, что адреса электронной почты являются действительными, Azure AD B2C требует от потребителей выполнить их проверку в процессе регистрации. Также предотвращаются вредоносные автоматизированные процессы, создающие для приложений фальшивые учетные записи.

Некоторые разработчики приложений предпочитают пропустить проверку адреса электронной почты в процессе регистрации. При таком сценарии потребители должны будут проверить адрес позже. Для этого в Azure AD B2C можно отключить проверку адреса электронной почты. Это позволяет упростить процедуру регистрации, а также дает разработчикам возможность разделить потребителей на прошедших проверку адреса и на тех, кто такую проверку еще не прошел.

По умолчанию в потоках пользователя для регистрации определено, что проверка адреса включена. Чтобы ее выключить, следуйте приведенным ниже инструкциям.

1. Щелкните **Потоки пользователей**.
2. Откройте свой поток пользователя (например B2C_1_SiUp), щелкнув его. 
3. Выберите **Макеты страниц**.
4. Щелкните **страницу регистрации локальной учетной записи**.
5. В разделе **Атрибуты пользователя** в колонке **Имя** щелкните **Адрес электронной почты**.
6. В меню **Требуется проверка** выберите **Нет**.
7. Щелкните **Сохранить** в верхней части колонки. Готово!

> [!NOTE]
> Отключение проверки адреса электронной почты в процессе регистрации может привести к получению спама. Если вы отключаете эту проверку по умолчанию, то рекомендуется добавить собственную систему проверки.
> 
>
