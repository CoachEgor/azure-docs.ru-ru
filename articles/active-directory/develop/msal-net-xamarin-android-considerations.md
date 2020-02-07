---
title: Замечания по Xamarin Android (MSAL.NET) | Службы
titleSuffix: Microsoft identity platform
description: Ознакомьтесь с конкретными соображениями при использовании Xamarin Android с библиотекой проверки подлинности Майкрософт для .NET (MSAL.NET).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 54df91d38541fbe17a28c9ae083ae0e7d0c9d88d
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77063668"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Вопросы, связанные с Xamarin Android, с MSAL.NET
В этой статье рассматриваются конкретные рекомендации при использовании Xamarin Android с библиотекой проверки подлинности Майкрософт для .NET (MSAL.NET).

## <a name="set-the-parent-activity"></a>Задание родительского действия

В Xamarin. Android необходимо задать родительское действие, чтобы токен был возвращен после взаимодействия.

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```
Его также можно задать на уровне Публикклиентаппликатион (в MSAL 4.2 +) с помощью обратного вызова.

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

Рекомендуется использовать Куррентактивитиплугин [здесь](https://github.com/jamesmontemagno/CurrentActivityPlugin).  Затем код Публикклиентаппликатион Builder будет выглядеть следующим образом:

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```


## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>Обеспечение возврата управления в MSAL после завершения интерактивной части потока проверки подлинности
В Android необходимо переопределить метод `OnActivityResult` `Activity` и вызвать метод Сетаусентикатионконтинуатионевентаргс класса MSAL Аусентикатионконтинуатионхелпер.

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```
Эта строка гарантирует, что элемент управления возвращается к MSAL после завершения интерактивной части потока проверки подлинности.

## <a name="update-the-android-manifest"></a>Обновление манифеста Android
`AndroidManifest.xml` должны содержать следующие значения:
```xml
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msauth"
              android:host="Enter_the_Package_Name"
              android:path="/Enter_the_Signature_Hash"/>
         </intent-filter>
</activity>
```
Замените имя пакета, зарегистрированного на портале Azure, значением `android:host=`. Замените хэш ключа, зарегистрированный на портале Azure, значением `android:path=`. **Недопустимо** применять к хэшу подписи кодирование по правилам для URL-адреса. Убедитесь, что в начале хэша подписи присутствует символ `/`.

Вы также можете [создать действие в коде](https://docs.microsoft.com/xamarin/android/platform/android-manifest#the-basics) и не изменять `AndroidManifest.xml`вручную. Для этого необходимо создать класс с атрибутом `Activity` и `IntentFilter`. Класс, представляющий те же значения приведенного выше XML-кода:

```csharp
  [Activity]
  [IntentFilter(new[] { Intent.ActionView },
        Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
        DataHost = "auth",
        DataScheme = "msal{client_id}")]
  public class MsalActivity : BrowserTabActivity
  {
  }
```

### <a name="xamarinforms-43x-manifest"></a>Манифест Ксамаринформс 4.3. X

Код, созданный Ксамаринформс 4.3. x, задает атрибут `package` для `com.companyname.{appName}` в `AndroidManifest.xml`. Если вы используете `DataScheme` как `msal{client_id}`, может потребоваться изменить значение так, чтобы оно совпадало с `MainActivity.cs` пространством имен.

## <a name="use-the-embedded-web-view-optional"></a>Использовать внедренное веб-представление (необязательно)

По умолчанию MSAL.NET использует системный веб-браузер, который позволяет получать единый вход с веб-приложениями и другими приложениями. В некоторых редких случаях может потребоваться указать, что вы хотите использовать встроенное веб-представление. Дополнительные сведения см. [в статье MSAL.NET. Использование веб-браузера](msal-net-web-browsers.md) и [браузера системы Android](msal-net-system-browser-android-considerations.md).

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>Устранение неполадок
Если вы создаете новое приложение Xamarin. Forms и добавляете ссылку на пакет NuGet MSAL.Net, это будет работать просто.
Однако если вы хотите обновить существующее приложение Xamarin. Forms до MSAL.NET Preview 1.1.2 или более поздней версии, могут возникнуть проблемы сборки.

Для устранения этих проблем следует выполнить следующие действия.
- Обновление существующего пакета NuGet MSAL.NET до MSAL.NET предварительной версии 1.1.2 или более поздней
- Проверка автоматического обновления Xamarin. Forms до версии 2.5.0.122203 (если нет, обновление до этой версии)
- Убедитесь, что Xamarin. Android. support. v4 автоматически обновлен до версии 25.4.0.2 (если нет, обновите до этой версии)
- Все пакеты Xamarin. Android. support должны быть нацелены на версию 25.4.0.2
- Очистка и перестроение
- Попробуйте задать для параметра max Parallel Project значение 1 в Visual Studio (параметры — > проекты и решения — > сборка и запуск — > Максимальное число параллельных сборок проектов).
- Кроме того, если вы выполняете сборку из командной строки, попробуйте удалить/m из команды, если вы используете ее.


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>Ошибка: имя "Аусентикатионконтинуатионхелпер" не существует в текущем контексте

Возможно, Visual Studio неправильно обновит файл Android. csproj *. Иногда **\<HintPath >** FilePath неправильно содержит netstandard13 вместо **monoandroid90**.

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения и примеры приведены в абзаце с описанием для [Android](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations) , приведенным в следующем примере файла readme.md:

| Образец | Платформа | Description |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP; | Простое приложение Xamarin Forms, в котором показано, как использовать MSAL для проверки подлинности MSA и Azure AD через конечную точку добавьте версии 2.0 и доступа к Microsoft Graph с полученным маркером. <br>![Топология](media/msal-net-xamarin-android-considerations/topology.png) |
