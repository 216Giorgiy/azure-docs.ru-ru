---
title: "Обновление с DirSync и Azure AD Sync | Документация Майкрософт"
description: "Узнайте, как обновить DirSync и Azure AD Sync до Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 28b5da6098316f8fbe84966e0dac88f5b7d2cb1d
ms.openlocfilehash: 53a67a2bccb622bfd79eb082baf9688336760cf1


---
# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Обновление Windows Azure Active Directory Sync (DirSync) и Azure Active Directory Sync (Azure AD Sync)
Azure AD Connect — это лучший способ для подключения локального каталога к Azure AD и Office 365. Если вы пользуетесь Windows Azure Active Directory Sync (DirSync) или Azure AD Sync, настало время перейти на Azure AD Connect, поскольку эти продукты уже устарели, и с 13 апреля 2017 года их поддержка будет прекращена.

Одно из устаревших средств проверки подлинности удостоверений было предназначено работы с одним лесом (DirSync), а другое — для работы с несколькими лесами и для продвинутых пользователей (Azure AD Sync). На смену этим средствам пришло единое решение, подходящее для любой ситуации: Azure AD Connect. Оно включает новые возможности, усовершенствования функций и поддержку новых сценариев. Чтобы сохранить возможность синхронизации локальных данных удостоверений между Azure AD и Office 365, настоятельно рекомендуем перейти на Azure AD Connect.

Последний выпуск DirSync вышел в июле 2014 года, а последний выпуск Azure AD Sync — в мае 2015 года.

## <a name="what-is-azure-ad-connect"></a>Что такое Azure AD Connect?
Azure AD Connect является преемником DirSync и Azure AD Sync. В нем объединены все сценарии, которые поддерживались этими средствами. Дополнительные сведения см. в статье [Интеграция локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>График устаревания
| Дата | Комментарий |
| --- | --- |
| 13 апреля 2016 г. |Windows Azure Active Directory Sync (DirSync) и Microsoft Azure Active Directory Sync (Azure AD Sync) объявлены устаревшими. |
| 13 апреля 2017 г. |Прекращение поддержки. Пользователи не смогут обратиться в службу поддержки, пока не перейдут на Azure AD Connect. |

## <a name="how-to-transition-to-azure-ad-connect"></a>Переход на Azure AD Connect
Если вы пользуетесь DirSync, переход можно выполнить одним из двух способов: обновлением на месте и параллельным обновлением. Обновление на месте рекомендуется для большинства клиентов при наличии последней версии операционной системы и менее чем 50 000 объектов. В других случаях рекомендуется выполнять параллельное развертывание, при котором конфигурация DirSync переносится на новый сервер под управлением Azure AD Connect.

Для пользователей Azure AD Sync рекомендуется обновление на месте. При желании вы можете параллельно установить новый сервер Azure AD Connect и перенести данные с сервера Azure AD Sync в Azure AD Connect.

| Решение | Сценарий |
| --- | --- |
| [Обновление из DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Если есть уже работающий сервер DirSync.</li> |
| [Обновление из Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md) |<li>При переходе с Azure AD Sync.</li> |

Обновление на месте для перехода с DirSync на Azure AD Connect демонстрируется в следующем видеоролике Channel 9:

> [!ВИДЕО https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
> 
> 

## <a name="faq"></a>Часто задаваемые вопросы
**Вопрос. Мне прислали уведомление по электронной почте от команды Azure и (или) сообщение из центра сообщений Office 365, хотя я использую Connect.**  
Уведомление было также отправлено клиентам, использующим Azure AD Connect с номером сборки 1.0.\*.0 (версии, предшествующие выпуску 1.1). Корпорация Майкрософт рекомендует пользователям обновлять Azure AD Connect до последних выпусков. В версии 1.1 функция [автоматического обновления](active-directory-aadconnect-feature-automatic-upgrade.md) существенно упрощает эту задачу: благодаря ей у вас всегда будет установлена последняя версия Azure AD Connect.

**Вопрос. Перестанет ли DirSync или Azure AD Sync работать 13 апреля 2017 года?**  
 Нет. Дата прекращения обмена данными между этими средствами и Azure AD будет объявлена позднее. Как только они станут известны, соответствующие сведения будут опубликованы в этом разделе.

**Вопрос. Какие версии DirSync можно обновить?**  
 Поддерживается обновление любой версии DirSync, которая используется в настоящее время.

**Вопрос. Как насчет соединителя Azure AD для FIM/MIM?**  
Соединитель Azure AD для FIM или MIM устаревшим **не** объявлен. Проект **заморожен**, т. е. новые функции не добавляются, а ошибки не исправляются. Корпорация Майкрософт рекомендует использующим его клиентам перейти на Azure AD Connect. Настоятельно рекомендуем не запускать с его помощью никакие новые развертывания. В будущем соединитель будет объявлен устаревшим.

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Интеграция локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md)




<!--HONumber=Dec16_HO3-->


