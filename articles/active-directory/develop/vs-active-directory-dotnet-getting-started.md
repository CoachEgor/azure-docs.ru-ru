---
title: Приступая к работе с Azure AD в проектах .NET MVC в Visual Studio
description: Как приступить к использованию Azure Active Directory в проектах .NET MVC после подключения или создания Azure AD с помощью подключенных служб Visual Studio
author: ghogen
manager: jillfra
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 806961dce41fbdf07572b3d0ab8b79bc9fb682da
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852020"
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>Приступая к работе с Azure Active Directory (проекты ASP.NET MVC)

> [!div class="op_single_selector"]
> - [Начало работы](vs-active-directory-dotnet-getting-started.md)
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

## <a name="next-steps"></a>Следующие шаги

- [Сценарии аутентификации в Azure Active Directory](authentication-scenarios.md)
- [Добавление возможности входа в веб-приложение ASP.NET с помощью учетной записи Майкрософт](quickstart-v1-aspnet-webapp.md)
