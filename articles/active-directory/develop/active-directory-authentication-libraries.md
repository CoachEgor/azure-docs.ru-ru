---
title: Библиотеки проверки подлинности Azure Active Directory | Документация Майкрософт
description: Библиотека проверки подлинности Azure AD (ADAL) позволяет разработчикам клиентских приложений легко проверять подлинность пользователей в облачной или локальной Active Directory (AD) и получать маркеры доступа для защиты вызовов API.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 831ebe53e43d0fef5bd57a699f02d06f2dff5ddc
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835497"
---
# <a name="azure-active-directory-authentication-libraries"></a>Библиотеки проверки подлинности Azure Active Directory

Библиотека аутентификации Azure Active Directory (ADAL) версии 1.0 позволяет разработчикам приложений проверять подлинность пользователей в облачной или локальной среде Active Directory (AD) и получать маркеры для защиты вызовов API. ADAL упрощает для разработчиков работу с проверкой подлинности, предоставляя такие возможности, как:

- настраиваемый кэш маркеров, в котором хранятся маркеры доступа и маркеры обновления;
- автоматическое обновление маркеров после истечения срока их действия, если доступен маркер обновления;
- поддержка асинхронных вызовов методов;

> [!NOTE]
> Ищете библиотеки Azure AD версии 2.0 (MSAL)? Ознакомьтесь с [руководством по библиотекам MSAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

## <a name="microsoft-supported-client-libraries"></a>Клиентские библиотеки, поддерживаемые корпорацией Майкрософт

| Платформа | Библиотека | Загрузить | Исходный код | Пример | Ссылка
| --- | --- | --- | --- | --- | --- |
| Клиент .NET, Магазин Windows, UWP, Xamarin iOS и Android |ADAL .NET версии 3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Классическое приложение](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Справочные материалы](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory?view=azure-dotnet) |
| Клиент .NET, Магазин Windows, Windows Phone 8.1 |ADAL .NET версии 2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Классическое приложение](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | |
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Одностраничное приложение](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[Приложение iOS](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Справочные материалы](http://cocoadocs.org/docsets/ADAL/2.5.1/)|
| Android |ADAL |[Maven](https://search.maven.org/search?q=g:com.microsoft.aad+AND+a:adal&core=gav) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Приложение Android](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [Javadocs](https://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Веб-приложение Node.js](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)|[Справочные материалы](https://docs.microsoft.com/javascript/api/adal-node/?view=azure-node-latest) |
| Java |ADAL4J |[Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Веб-приложение Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) |[Справочные материалы](https://javadoc.io/doc/com.microsoft.azure/adal4j) |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Веб-приложение Python](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Справочные материалы](https://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Серверные библиотеки, поддерживаемые корпорацией Майкрософт

| Платформа | Библиотека | Загрузить | Исходный код | Пример | Ссылка
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN для AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.ActiveDirectory) |[Приложение MVC](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN для OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.OpenIdConnect) |[Веб-приложение](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |OWIN для WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.WsFederation) |[Веб-приложение MVC](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Расширения протокола удостоверения для .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |Обработчик JWT для .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Веб-интерфейс API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Сценарии

Ниже приведены три типичных сценария использования ADAL в клиенте, который обращается к удаленному ресурсу:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Проверка подлинности пользователей собственного клиентского приложения, выполняющегося на устройстве

В этом сценарии у разработчика есть мобильное клиентское или классическое приложение, которому требуется доступ к удаленному ресурсу, например к веб-API. Веб-API не допускает анонимные вызовы и должен вызываться в контексте пользователя, прошедшего проверку подлинности. Для веб-API предварительно настроено доверие для маркеров доступа, выданных конкретным клиентом Azure AD. Azure AD предварительно настроен для выдачи маркеров доступа для этого ресурса. Чтобы вызвать веб-API из клиента, разработчик использует библиотеку ADAL для упрощения проверки подлинности в Azure AD. Для обеспечения максимальной защиты необходимо использовать ADAL для отображения пользовательского интерфейса, в котором пользователь вводит учетные данные (отображается в окне браузера).

ADAL упрощает проверку подлинности пользователя, получение маркера доступа и маркера обновления от Azure AD и вызовы к веб-API с помощью маркера доступа.

Пример кода, который демонстрирует этот сценарий с использованием проверки подлинности Azure AD, см. в разделе [Собственное клиентское приложение WPF для веб-интерфейса API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Проверка подлинности конфиденциального клиентского приложения, выполняющегося на веб-сервере

В этом сценарии у разработчика есть запущенное на сервере приложение, которому требуется доступ к удаленному ресурсу, например, к веб-API. Веб-API не допускает анонимных вызовов, поэтому он должен быть вызван из службы, прошедшей проверку подлинности. Для веб-API предварительно настроено доверие для маркеров доступа, выданных конкретным клиентом Azure AD. Azure AD предварительно настроен для выдачи маркеров доступа для этого ресурса службе с учетными данными клиента (идентификатор клиента и секрет клиента). ADAL упрощает проверку подлинности службы в Azure AD, возвращая маркер доступа, который может использоваться для вызова веб-API. ADAL также управляет жизненным циклом маркера доступа, кэшируя его и возобновляя его действие при необходимости. Пример кода для такого сценария представлен в описании [консольной управляющей программы для веб-API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Проверка подлинности конфиденциального клиентского приложения, выполняющегося на веб-сервере, от имени пользователя

В этом сценарии у разработчика есть запущенное на сервере веб-приложение, которому требуется доступ к удаленному ресурсу, например к веб-API. Веб-API не допускает анонимных вызовов, поэтому он должен быть вызван из службы, прошедшей проверку подлинности, от имени пользователя, прошедшего проверку подлинности. Для веб-API предварительно настроено доверие для маркеров доступа, выданных конкретным клиентом Azure AD. Azure AD предварительно настроен для выдачи маркеров доступа для этого ресурса службе с учетными данными клиента. После того как пользователь прошел проверку подлинности для веб-приложения, оно может получить код проверки подлинности пользователя из Azure AD. Затем веб-приложение может использовать ADAL для получения маркера доступа и обновления маркера от имени пользователя с помощью кода проверки подлинности и учетных данных клиента, связанных с приложением в Azure AD. После получения маркера доступа приложение может обращаться к веб-интерфейсу API до истечения срока действия маркера. После истечения срока действия маркера веб-приложение может использовать ADAL для получения нового маркера доступа с помощью обновления маркера, полученного ранее. Пример кода для такого сценария представлен в описании [собственного клиента для веб-API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>См. также

- [Руководство разработчика по Azure Active Directory](v1-overview.md)
- [Сценарии проверки подлинности в Azure Active Directory](authentication-scenarios.md)
- [Примеры кода Azure Active Directory](sample-v1-code.md)
