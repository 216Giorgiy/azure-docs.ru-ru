---
title: 'Azure AD Connect выполняет следующие функции: параметры устройств | Документация Майкрософт'
description: В этом документе описаны параметры устройств, доступные в Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/13/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8f397401581cc1bc947f1b19ce248c17fba26f4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56160728"
---
# <a name="azure-ad-connect-device-options"></a>Azure AD Connect выполняет следующие функции: Параметры устройств

В этом документе содержится сведения о различных параметрах устройств, доступных в Azure AD Connect. С помощью Azure AD Connect можно настроить две следующие операции: 
* **Гибридное присоединение к Azure AD**. Если в вашей среде действует локальная служба AD и вы хотите воспользоваться преимуществами Azure AD, можно применить гибридные устройства, присоединенные к Azure AD. Это устройства, которые присоединены и к локальной среде Active Directory, и к Azure Active Directory.
* **Обратная запись устройств**. Обратная запись устройств используется для включения условного доступа для устройств в AD FS (2012 R2 или более поздней версии).

## <a name="configure-device-options-in-azure-ad-connect"></a>Настройка параметров устройств в Azure AD Connect

1.  Запустите Azure AD Connect. На странице **Дополнительные задачи** выберите **Настройка параметров устройств**.  Щелкните **Далее**.
    ![Настройка параметров устройств](./media/how-to-connect-device-options/deviceoptions.png) 

    На странице **Обзор** приводятся подробные сведения.
    ![Обзор](./media/how-to-connect-device-options/deviceoverview.png)

    >[!NOTE]
    > Новая функция настройки параметров устройств доступна только в версии 1.1.819.0 и более поздних.

2.  Указав учетные данные для Azure AD, на странице "Параметры устройств" можно выбрать операцию для выполнения.
    ![Операции с устройствами](./media/how-to-connect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Дополнительная информация

* [Настройка гибридных устройств, присоединенных к Azure Active Directory](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Azure AD Connect: включение обратной записи устройств](how-to-connect-device-writeback.md)

