---
title: Приступая к работе с мобильными приложениями Azure для приложений Xamarin.Android | Microsoft Azure
description: Этот учебник поможет приступить к использованию мобильных приложений Azure для разработки приложений Xamarin Android
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: crdun
ms.openlocfilehash: 29efa963a254913e3d4744ade1d161c5c8ce42e4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127902"
---
# <a name="create-a-xamarinandroid-app"></a>Создание приложения Xamarin.Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Обзор
В этом руководстве показано, как добавить облачную серверную службу в приложение Xamarin.Android. Дополнительные сведения см. в статье [Что такое мобильные приложения?](app-service-mobile-value-prop.md).

Снимок экрана завершенного приложения приведен ниже:

![][0]

Завершение изучения этого учебника является необходимым условием для работы со всеми другими учебниками, посвященными мобильным приложениям для приложений Xamarin.Android.

## <a name="prerequisites"></a>Технические условия
Для работы с данным руководством вам потребуется:

* Активная учетная запись Azure. Если у вас еще нет учетной записи, зарегистрируйтесь для использования пробной версии Azure и получите до 10 бесплатных мобильных приложений. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio с Xamarin. Инструкции см. в статье об [установке и настройке Visual Studio и Xamarin](/visualstudio/cross-platform/setup-and-install).

## <a name="create-an-azure-mobile-app-backend"></a>Создание серверной части мобильного приложения Azure
Чтобы создать серверную часть мобильного приложения, выполните указанные ниже действия.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Итак, вы подготовили серверную часть мобильного Azure, которая может использоваться мобильными клиентскими приложениями. Теперь скачайте серверный проект со списком простых задач и опубликуйте его в Azure.

## <a name="configure-the-server-project"></a>Настройка серверного проекта
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Скачивание и запуск приложения Xamarin.Android
1. В разделе **Download and run your Xamarin.Android project** (Загрузка и запуск проекта Xamarin.Android) нажмите кнопку **Download** (Загрузить).

      Сохраните сжатый файл проекта на локальном компьютере и запомните путь к нему.
2. Нажмите клавишу **F5** , чтобы выполнить сборку проекта, после чего запустите приложение.
3. В приложении введите содержательный текст, например *Работа с руководством*, и нажмите кнопку **Добавить**.

    ![][10]

    Данные из запроса вставляются в таблицу TodoItem. Элементы, сохраненные в таблице, возвращаются серверной частью мобильного приложения, а данные отображаются в списке.

   > [!NOTE]
   > Вы можете просмотреть код, который получает доступ к внутреннему серверу мобильного приложения для запроса и вставки данных, которые находятся в файле C# ToDoActivity.cs.
   >
   >

## <a name="troubleshooting"></a>Устранение неполадок
При проблемах со сборкой проекта запустите диспетчер пакетов NuGet и выполните обновление пакетов поддержки `Xamarin.Android`. Проекты быстрого запуска не всегда включают самые свежие версии.

Обратите внимание, что все пакеты поддержки, на которые содержатся ссылки в проекте Android, должны быть одной версии. [Пакет NuGet для мобильных приложений Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) имеет зависимость `Xamarin.Android.Support.CustomTabs` для платформы Android, поэтому если в проекте используются пакеты поддержки более новой версии, во избежание конфликтов необходимо установить требуемую версию пакета.

## <a name="next-steps"></a>Дальнейшие действия
* [Включение автономной синхронизации для мобильного приложения Xamarin.Android](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Добавление аутентификации в приложение](app-service-mobile-xamarin-android-get-started-users.md)
* [Добавление push-уведомлений в приложение Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md)
* [Использование управляемого клиента для мобильных приложений Azure](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
