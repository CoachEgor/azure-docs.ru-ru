---
title: Создание универсальной платформы Windows (UWP) для использования мобильных приложений Azure | Документация Майкрософт
description: Следуйте указаниям этого руководства, чтобы начать работу с серверной частью мобильных приложений Azure для разработки приложений универсальной платформы Windows (UWP) на C#, Visual Basic или JavaScript.
services: app-service\mobile
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: crdun
ms.openlocfilehash: 959c1ff8b199320105f650a7eb62a04bedb03b3b
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412786"
---
# <a name="create-a-windows-app-with-an-azure-backend"></a>Создание приложения Windows с серверной частью Azure

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Обзор

В этом руководстве показано, как добавить облачную серверную службу в универсальную платформу Windows (UWP). Дополнительные сведения см. в статье [Общие сведения о мобильных приложениях](app-service-mobile-value-prop.md). Ниже приведены снимки экрана готового приложения.

![Готовое классическое приложение](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)

Изучение этого руководства является необходимым условием для работы со всеми остальными руководствами, посвященными мобильным приложениям UWP.

## <a name="prerequisites"></a>Технические условия

Для работы с этим учебником требуется:

* Активная учетная запись Azure. Если у вас нет учетной записи, можно зарегистрироваться для получения бесплатной пробной версии Azure и получить до 10 бесплатных мобильных приложений, которые можно использовать и после окончания пробного периода. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/).
* Windows 10;
* [Visual Studio Community].
* Знакомство с разработкой приложений UWP. Ознакомьтесь с [документацией по UWP](https://docs.microsoft.com/windows/uwp/), чтобы получить инструкции по [настройке](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) для создания приложений UWP.

## <a name="create-a-new-azure-mobile-app-backend"></a>Создание серверной части мобильного приложения Azure

Чтобы создать серверную часть мобильного приложения, выполните указанные ниже действия.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Итак, вы подготовили серверную часть мобильного Azure, которая может использоваться мобильными клиентскими приложениями. Теперь скачайте серверный проект со списком простых задач и опубликуйте его в Azure.

## <a name="configure-the-server-project"></a>Настройка серверного проекта

[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-client-project"></a>Скачивание и выполнение клиентского проекта

Настроив серверную часть мобильного приложения, можно создать новое клиентское приложение или изменить существующее приложение, чтобы подключиться к Azure. В этом разделе вы загрузите приме проекта приложения UWP, который настроен для подключения к серверной части мобильного приложения.

1. Вернитесь в колонку **Быстрый запуск** серверной части мобильного приложения и последовательно выберите элементы **Создать новое приложение** > **Загрузить**. Затем извлеките сжатые файлы проекта на локальный компьютер.

    ![Загрузка проекта быстрого запуска Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

2. Откройте проект UWP и нажмите клавишу F5, чтобы развернуть и запустить приложение.
3. В приложении в поле **Insert a TodoItem** (Вставить TodoItem) введите содержательный текст, например *Работа с руководством*, и нажмите кнопку **Сохранить**.

    ![Полный быстрый запуск Windows — классическая версия](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    В результате будет отправлен запрос POST к серверной части нового мобильного приложения, размещенного в Azure.

> [!TIP]
> При использовании серверной части .NET можно добавить проект приложения UWP в то же решение, в которое добавлен серверный проект. Это упрощает отладку и тестирование приложения и серверной части в одном решении Visual Studio. Чтобы добавить проект приложения UWP для серверной части решения, необходимо использовать Visual Studio 2017 или более поздней версии.

## <a name="next-steps"></a>Дальнейшие действия

* [Добавление аутентификации в приложение](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Узнайте больше о проверке подлинности пользователей приложения с помощью поставщика удостоверений.
* [Добавление push-уведомлений в приложение](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Узнайте, как добавить поддержку push-уведомлений в мобильное приложение и настроить в его серверной части использование концентраторов уведомлений Azure для отправки push-уведомлений.
* [Включение автономной синхронизации для приложения](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Узнайте, как добавить в приложение поддержку автономной работы с помощью серверной части мобильного приложения. Автономная синхронизация позволяет пользователям взаимодействовать с мобильным приложением &mdash; просматривать, добавлять или изменять данные &mdash; даже при отсутствии подключения к сети.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: https://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community]: https://go.microsoft.com/fwLink/p/?LinkID=534203
