---
title: Приступая к работе с Azure AD в проектах .NET MVC в Visual Studio | Документация Майкрософт
description: Как приступить к использованию Azure Active Directory в проектах .NET MVC после подключения или создания Azure AD с помощью подключенных служб Visual Studio
services: active-directory
documentationcenter: ''
author: kraigb
manager: ghogen
editor: ''
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 07fa4655d9e7ad74cae33391d55c7c9be1d446a6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>Приступая к работе с Azure Active Directory (проекты ASP.NET MVC)

> [!div class="op_single_selector"]
> - [Приступая к работе](vs-active-directory-dotnet-getting-started.md)
> - [Что произошло?](vs-active-directory-dotnet-what-happened.md)

В этой статье приведены дополнительные рекомендации, которые потребуются вам после того, как вы добавите Active Directory в проект ASP.NET MVC, щелкнув **Проект > Подключенные службы** в Visual Studio. Если вы еще не добавили службу в проект, это можно сделать в любое время.

Изменения, которые вносятся в проект при добавлении подключенной службы, описаны в статье [Что произошло с моим проектом MVC в подключенной службе Visual Studio Azure Active Directory?](vs-active-directory-dotnet-what-happened.md)

## <a name="requiring-authentication-to-access-controllers"></a>Требование проверки подлинности для доступа к контроллерам

Ко всем контроллерам в проекте добавлен атрибут `[Authorize]`. Этот атрибут обеспечивает аутентификацию пользователей перед их доступом к контроллерам. Для анонимного доступа к контроллеру удалить с него этот атрибут. Если необходимо задать разрешения на более детальном уровне, примените атрибут к каждому методу, требующему проверки подлинности, а не к классу контроллера.

## <a name="adding-signin--signout-controls"></a>Добавление элементов управления SignIn и SignOut

Чтобы добавить элементы управления SignIn и SignOut, используйте частичное представление `_LoginPartial.cshtml` для добавления функций к одному из представлений. Вот пример добавления такой функциональности в стандартное представление `_Layout.cshtml`. (обратите внимание на последний элемент в теге div с классом navbar-collapse):

```html
<!DOCTYPE html>
 <html>
 <head>
     <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title - My ASP.NET Application</title>
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
</head>
<body>
    <div class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>
        </div>
    </div>
    <div class="container body-content">
        @RenderBody() 
        <hr />
        <footer>
            <p>&copy; @DateTime.Now.Year - My ASP.NET Application</p>
        </footer>
    </div>
    @Scripts.Render("~/bundles/jquery")
    @Scripts.Render("~/bundles/bootstrap")
    @RenderSection("scripts", required: false)
</body>
</html>
```

## <a name="next-steps"></a>Дополнительная информация

- [Сценарии аутентификации в Azure Active Directory](active-directory-authentication-scenarios.md)
- [Добавление возможности входа в веб-приложение ASP.NET с помощью учетной записи Майкрософт](guidedsetups/active-directory-aspnetwebapp-v1.md)
