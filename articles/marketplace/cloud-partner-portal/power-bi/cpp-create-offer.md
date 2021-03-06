---
title: Создание предложения приложения Power BI в Azure Marketplace | Документация Майкрософт
description: Создание предложения приложения Power BI для Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: pbutlerm
ms.openlocfilehash: 6a4f7daa337618278c3652fad3053c20557a9e28
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414695"
---
# <a name="create-a-power-bi-app-offer"></a>Создание предложения приложения Power BI

В этой статье приведены шаги по созданию нового предложения приложения Power BI для [AppSource](https://appsource.microsoft.com). Каждое предложение отображается в виде отдельной сущности в AppSource. При создании нового предложения на [Портале Cloud Partner](https://cloudpartner.azure.com/) необходимо предоставить четыре группы ресурсов для предложения.

Группы основных ресурсов описаны в таблице ниже.

|   Группа ресурсов      | ОПИСАНИЕ                                                                         |
| ----------------   | ----------------                                                                    |
| Параметры предложения     | Первичные идентифицирующие данные и название предложения.                                      |
| Технические сведения     | URL-адрес установщика, используемый для установки приложения в рабочей области Power BI клиента. Дополнительные сведения о создании этого URL-адреса см. в [документации по приложениям Power BI](https://go.microsoft.com/fwlink/?linkid=2028636). |
| Подробные сведения об онлайн-магазинах | Содержит маркетинговые, юридические ресурсы и ресурсы управления потенциальными клиентами. Маркетинговые ресурсы включают описание и логотипы предложения, а юридические — политику конфиденциальности, условия использования и другие юридические документы. Политика управления потенциальными клиентами позволяет указать способ обработки потенциальных клиентов пользовательского портала AppSource. |
| Контакты           | Содержит сведения о поддержке контактных данных и политики.                                     |

## <a name="new-offer-form"></a>Форма нового предложения

Войдите на Портал Cloud Partner, а затем в строке меню слева выберите **Новое предложение**. Далее, чтобы отобразить форму "Новое предложение" и начать процесс определения ресурсов для нового предложения, выберите **Power BI Apps** (Приложения Power BI).

![Пункт меню предложения Power BI](./media/new-offer-menu.png)

> [!NOTE] 
> Если параметр **Power BI Apps** (Приложения Power BI) не отображается или не включен, учетная запись не имеет разрешения на создание этого типа предложения. Убедитесь, что соблюдены все [предварительные требования](./cpp-prerequisites.md) для этого типа предложения, включая регистрацию для учетной записи разработчика.


## <a name="next-steps"></a>Дополнительная информация

В следующих статьях описаны вкладки на странице **Новое предложение** для типа предложения приложения Power BI. Каждая статья описывает группы ресурсов и вспомогательные службы для нового предложения приложения Power BI.

-  [Вкладка с параметрами предложения](./cpp-offer-settings-tab.md)
-  [Вкладка с техническими сведениями](./cpp-technical-info-tab.md)
-  [Вкладка со сведениями об онлайн-магазине](./cpp-storefront-details-tab.md)
-  [Вкладка "Контакты"](./cpp-contacts-tab.md)
