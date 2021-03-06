---
title: Azure AD Connect Health. Диагностика ошибок синхронизации повторяющихся атрибутов | Документация Майкрософт
description: В этом документе описывается диагностика ошибок синхронизации повторяющихся атрибутов и возможное исправление сценариев потерянных объектов непосредственно с портала Azure.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: maheshu
editor: billmath
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbdeef7c591221756ad206bf2f3dd78ac3d26c4f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "57885323"
---
# <a name="diagnose-and-remediate-duplicated-attribute-sync-errors"></a>Диагностика и устранение ошибок синхронизации повторяющихся атрибутов

## <a name="overview"></a>Обзор
Расширяя возможности выделения ошибок синхронизации, Azure Active Directory (Azure AD) Connect Health предоставляет функцию самостоятельного устранения ошибок. С ее помощью можно диагностировать ошибки синхронизации повторяющихся атрибутов и исправлять потерянные объекты из Azure AD.
Функция диагностики имеет следующие преимущества:
- Она предоставляет диагностическую процедуру, которая позволяет сузить сведения об ошибках синхронизации повторяющихся атрибутов. И обеспечивает соответствующие исправления.
- Она применяет исправление для выделенных сценариев из Azure AD, позволяя устранить ошибку одним действием.
- для включения этой функции не требуется обновление или настройка
Дополнительные сведения об Azure AD см. в статье [Синхронизация удостоверений и устойчивость повторяющихся атрибутов](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="problems"></a>Проблемы
### <a name="a-common-scenario"></a>Стандартный сценарий
При возникновении ошибок синхронизации **QuarantinedAttributeValueMustBeUnique** и **AttributeValueMustBeUnique** в Azure AD обычно происходит конфликт значений **UserPrincipalName** или **адресов прокси-сервера**. Вы можете устранить ошибки синхронизации, обновив конфликтующий исходный объект в локальном расположении. Ошибка синхронизации будет устранена после следующей синхронизации. Например, на приведенном ниже рисунке показан конфликт значений **UserPrincipalName** двух пользователей. Оба являются **Joe.J\@contoso.com**. В Azure AD конфликтующие объекты помещаются в карантин.

![Распространенный сценарий диагностики ошибок синхронизации](./media/how-to-connect-health-diagnose-sync-errors/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Сценарий с потерянным объектом
Иногда можно обнаружить, что текущий пользователь теряет **привязку к источнику**. В локальном каталоге Active Directory произошло удаление исходного объекта. Однако изменение сигнала удаления не было синхронизировано с Azure AD. Это может произойти из-за проблем механизма синхронизации или из-за переноса домена. Когда тот же объект восстанавливается или повторно создается, по логике для текущего пользователя должна выполняться синхронизация из **привязки к источнику**. 

Если пользователь существует в виде облачного объекта, возможны также конфликты при синхронизации пользователей с Azure AD. Пользователя невозможно сопоставить при синхронизации с существующим объектом. Прямого способа переназначить **привязку к источнику** нет. Узнайте больше из [базы знаний](https://support.microsoft.com/help/2647098). 

Например, текущий объект в Azure AD содержит лицензию Joe. Недавняя синхронизация объекта с другой **привязкой к источнику** произошла при наличии повторяющегося атрибута в Azure AD. В Azure AD изменения для Николая в локальном каталоге Active Directory нельзя применить к первоначальному пользователю Joe (текущий объект).  

![Диагностика ошибок синхронизации в сценарии с потерянным объектом](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Шаги по диагностике и устранению неполадок в Connect Health 
Функция диагностики поддерживает объекты-пользователи со следующими повторяющимися атрибутами.

| Имя атрибута | Типы ошибок синхронизации|
| ------------------ | -----------------|
| UserPrincipalName | QuarantinedAttributeValueMustBeUnique или AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique или AttributeValueMustBeUnique | 
| SipProxyAddress | AttributeValueMustBeUnique | 
| OnPremiseSecurityIdentifier |  AttributeValueMustBeUnique |

>[!IMPORTANT]
> Для доступа к этой функции в параметрах RBAC требуется разрешение **глобального администратора** или **участника**.
>

Следуя инструкциям на портале Azure, вы сможете уточнить сведения об ошибках синхронизации и найти более конкретные решения.

![Процедура диагностики ошибок синхронизации](./media/how-to-connect-health-diagnose-sync-errors/IIdFixSteps.png)

На портале Azure вам нужно выполнить несколько шагов, чтобы определить конкретные сценарии для исправления.  
1.  Просмотрите столбец **Diagnose status** (Состояние диагностики). В нем указывается, существует ли способ устранить ошибку синхронизации непосредственно из Azure Active Directory. Другими словами, существует ли процедура устранения неполадок, позволяющая уточнить ошибку и потенциально устранить ее.

| Status | Что это означает? |
| ------------------ | -----------------|
| Не запущено | Вы не рассмотрели этот процесс диагностики. В зависимости от результата диагностики, может существовать возможность исправить ошибку синхронизации напрямую с портала. |
| Требуется исправление вручную | Ошибка не подпадает под критерии доступного исправления с портала. Возможно, конфликтующие типы объектов не являются пользователями или вы уже выполнили диагностику, а на портале нет доступных исправлений. В последнем случае одним из решений является исправление из локальной среды. [Дополнительные сведения об исправлении в локальной среде](https://support.microsoft.com/help/2647098). | 
| Ожидается синхронизация | Исправление было применено. Портал ожидает следующего цикла синхронизации для устранения ошибки. |

  >[!IMPORTANT]
  > Данные в столбце состояния диагностики будут сбрасываться после каждого цикла синхронизации. 
  >

1. Нажмите кнопку **Диагностика** под сведениями об ошибке. Можно ответить на несколько вопросов и получить сведения об ошибке синхронизации. Ответы на вопросы помогают идентифицировать сценарий с потерянным объектом.

1. Если после окончания диагностики появляется кнопка **Закрыть**, это означает, что на основе данных ответов на портале нет способов быстрого устранения неполадок. Выберите решение, показанное на последнем шаге. По-прежнему доступно устранение из локальной среды. Нажмите кнопку **Закрыть**. Состояние текущей ошибки синхронизации изменится на **Требуется исправление вручную**. Это состояние сохранится на протяжении текущего цикла синхронизации.

1. После определения сценария с потерянным объектом вы сможете исправить ошибки синхронизации повторяющихся атрибутов напрямую с портала. Нажмите кнопку **Применить исправление**, чтобы активировать процесс. Состояние текущей ошибки синхронизации обновится и изменится на **Ожидается синхронизация**.

1. После следующего цикла синхронизации ошибка будет удалена из списка.

## <a name="how-to-answer-the-diagnosis-questions"></a>Как ответить на вопросы по диагностике 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>Есть ли в локальном каталоге Active Directory пользователь?

Этот вопрос позволяет идентифицировать исходный объект текущего пользователя из локального каталога Active Directory.  
1. Проверьте, содержит ли Active Directory объект с предоставленным значением **UserPrincipalName**. Если это не так, ответьте **Нет**.
2. Если это так, убедитесь, что объект все еще находится в области для синхронизации.  
   - Выполните поиск в пространстве соединителя Azure AD с помощью различающегося имени.
   - Если состояние найденного объекта **Отложенное добавление**, ответьте **Нет**. Azure AD Connect не может подключить объект к соответствующему объекту Azure AD.
   - Если объект не найден, ответьте **Да**.

В этих примерах вопрос пытается определить, существует ли пользователь **Joe Jackson** в локальном каталоге Active Directory.
Для**распространенного сценария** оба пользователя — **Joe Johnson** и **Joe Jackson** будут присутствовать в локальном каталоге Active Directory. Объектами, помещенными в карантин, являются два разных пользователя.

![Распространенный сценарий диагностики ошибок синхронизации](./media/how-to-connect-health-diagnose-sync-errors/IIdFixCommonCase.png)

Для **сценария с потерянным объектом** только один пользователь (**Joe Johnson**) будет существовать в локальном каталоге Active Directory.

![Диагностика ошибок синхронизации в сценарии с потерянным объектом при существовании пользователя](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)

### <a name="do-both-of-these-accounts-belong-to-the-same-user"></a>Обе эти учетные записи принадлежат одному пользователю?
Этот вопрос позволяет проверить входящего конфликтующего пользователя и текущий объект пользователя в Azure AD, чтобы узнать, принадлежат ли они одному пользователю.  
1. Конфликтующий объект вновь синхронизируется с Azure Active Directory. Сравните атрибуты объектов.  
   - Отображаемое имя
   - Имя участника-пользователя
   - Object ID
2. Если службе Azure AD не удается их сравнить, проверьте, есть ли в Active Directory объекты с указанными значениями **UserPrincipalName**. Ответьте **Нет**, если найдете оба объекта.

В приведенном ниже примере два объекта принадлежат одному и тому же пользователю, **Joe Johnson**.

![Диагностика ошибок синхронизации в сценарии с потерянным объектом, если владельцем является один пользователь](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)


## <a name="what-happens-after-the-fix-is-applied-in-the-orphaned-object-scenario"></a>Что происходит после того, как исправление применяется для сценария с потерянным объектом
Ответив на вопросы выше, вы увидите кнопку **Применить исправление**, если в Azure AD доступно исправление. В этом случае локальный объект синхронизируется с непредвиденным объектом Azure AD. Два объекта сопоставляются с помощью **привязки к источнику**. При нажатии кнопки **Применить исправление** выполняются приведенные ниже или схожие действия:
1. обновляется **привязка к источнику** для правильного объекта в Azure AD;
2. удаляется конфликтующий объект в Azure AD (при его наличии).

![Диагностика ошибок синхронизации после исправления](./media/how-to-connect-health-diagnose-sync-errors/IIdFixAfterFix.png)

>[!IMPORTANT]
> Действие **Применить исправление** применимо только к сценариям с потерянными объектами.
>

Выполнив описанные выше действия, пользователь может получить доступ к исходному ресурсу, который представляет собой ссылку на существующий объект. Значение **Diagnose status** (Состояние диагностики) в представлении списка будет изменено на **Ожидается синхронизация**. Ошибка синхронизации будет устранена после следующей синхронизации. Connect Health больше не будет отображать в представлении списка исправленную ошибку синхронизации.

## <a name="failures-and-error-messages"></a>Ошибки и сообщения об ошибках
**User with conflicting attribute is soft deleted in the Azure Active Directory. Ensure the user is hard deleted before retry.** (Пользователь с конфликтующим атрибутом обратимо удален из Azure Active Directory. Перед повторной попыткой убедитесь, что пользователь удален необратимо.)  
Пользователь с конфликтующим атрибутом в Azure AD должен быть удален перед попыткой устранить ошибку. Узнайте, [как навсегда удалить пользователя из Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-restore), прежде чем повторить попытку исправления. Пользователь также будет автоматически необратимо удален через 30 дней после того, как он был обратимо удален. 

**Updating source anchor to cloud-based user in your tenant is not supported.** (Обновление привязки к источнику в клиенте для облачного пользователя не поддерживается.)  
У облачного пользователя в Azure AD не должно быть привязки к источнику. В этом случае обновление привязки к источнику не поддерживается. Требуется исправление вручную из локальной среды. 

## <a name="faq"></a>Часто задаваемые вопросы
**Вопрос.** Что происходит при сбое применения действия **Применить исправление**?  
**Ответ.** Если произошел сбой применения, возможно, в Azure AD Connect возникла ошибка экспорта. Обновите страницу портала и повторите попытку после следующей синхронизации. Цикл синхронизации по умолчанию составляет 30 минут. 


**Вопрос.** Что если **текущий объект** является объектом, который нужно удалить?  
**Ответ.** Если следует удалить **существующий объект**, этот процесс не требует изменения **привязки к источнику**. Как правило, исправить проблему можно из локального каталога Active Directory. 


**Вопрос.** Какие разрешения нужны пользователю для применения исправления?  
**Ответ.** Разрешения **глобального администратора** или **участника** в параметрах RBAC позволяют получить доступ к диагностике и устранению неполадок.


**Вопрос.** Нужно ли мне настраивать AAD Connect или обновлять агент Azure AD Connect Health для использования этой функции?  
**Ответ.** Нет, диагностика — это полностью облачная функция.


**Вопрос.** При обратимом удалении текущего объекта можно ли с помощью процесса диагностики восстановить его?  
**Ответ.** Нет, при исправлении атрибут объекта не обновляется, если это не **привязка к источнику**.
