---
title: Включить мультитенантные приложения - Azure Digital Twins Документы Майкрософт
description: Как настроить мультитенантные приложения Azure Active Directory для Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 6e1321e01d8d12974a2704f4478b02a26c14142f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264939"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Включение мультитенантных приложений с помощью Azure Digital Twins

Разработчикам решений, которые выполняют сборку в Azure Digital Twins, может понадобиться реализовать несколько пользователей в рамках одной службы или решения. На самом деле, *мультитенантные* приложения являются наиболее распространенными конфигурациями Azure Digital Twins.

В этом документе описано, как настроить в приложении Azure Digital Twins поддержку нескольких клиентов и пользователей Azure Active Directory.

## <a name="multitenancy"></a>Мультитенантный режим

*Мультитенантный* ресурс — это единый подготовленный экземпляр, который поддерживает нескольких пользователей. У каждого пользователя есть собственные независимые данные и привилегии. Интерфейсы пользователей изолированы друг от друга, поэтому их представление приложения отличается.

Дополнительные сведения о мультитенантности см. в статье [Мультитенантные приложения Azure](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications).

## <a name="problem-scenario"></a>Сценарий проблемы

В этом случае есть **разработчик**, создающий решение Azure Digital Twins, и **пользователь**, который использует это решение:

- У **разработчика** есть подписка Azure с клиентом Azure Active Directory.
- **Разработчик** развертывает экземпляр Azure Digital Twins в свою подписку Azure. Azure Active Directory автоматически создает субъект-службу в клиенте Azure Active Directory **разработчика**.
- Пользователи в клиенте Azure Active Directory **разработчика** затем могут [получить маркеры OAuth 2.0](./security-authenticating-apis.md) от службы Azure Digital Twins.
- **Разработчик** теперь создает мобильное приложение, которое напрямую интегрируется с API управления Azure Digital Twins.
- **Разработчик** позволяет **пользователю** использовать мобильное приложение.
- **Пользователь** должен быть авторизован для использования API управления Azure Digital Twins в приложении **разработчика**.

Проблема:

- Когда **пользователь** входит в приложение **разработчика**, приложение не может получать токены для пользователей **этого пользователя**, чтобы выполнить аутентификацию в API управления Azure Digital Twins.
- В Azure Active Directory выдается исключение, указывающее, что экземпляр Azure Digital Twins не распознан в каталоге **пользователя**.

## <a name="problem-solution"></a>Решение проблемы

Чтобы решить проблему описанного выше сценария, нужно предпринять следующие действия, чтобы создать субъект-службу Azure Digital Twins в клиенте Azure Active Directory **пользователя**.

- Если у **пользователя** еще нет подписки Azure с клиентом Azure Active Directory:

  - Администратор клиента Azure Active Directory **пользователя** должен получить [подписку Azure с оплатой по мере использования](https://azure.microsoft.com/offers/ms-azr-0003p/),
  - после этого администратор клиента Azure AD **пользователя** должен [связать его с новой подпиской](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity).

- На [портале Azure](https://portal.azure.com) администратор клиента Azure AD **пользователя** должен выполнить описанные ниже действия:

  1. Поиск **подписок** в верхнем поле поиска Azure. Выберите **Подписку**.
  1. Выберите подписку с клиентом Azure Active Directory, который будет использоваться в приложении **разработчика**.

     [![Подписки Azure Active Directory](media/multitenant/ad-subscriptions.png)](media/multitenant/ad-subscriptions.png#lightbox)

  1. Выберите **поставщиков ресурсов.**
  1. Найти **Microsoft.IoTSpaces**.
  1. Выберите **Зарегистрировать**.

     [![Поставщики ресурсов Azure Active Directory](media/multitenant/ad-resource-providers.png)](media/multitenant/ad-resource-providers.png#lightbox)
  
## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения см. в статье [Создание определяемых пользователем функций в Azure Digital Twins](./how-to-user-defined-functions.md).

- Дополнительные сведения об использовании управления доступом на основе ролей для дальнейшей защиты приложения см. в статье [Создание назначений ролей и управление ими в Azure Digital Twins](./security-create-manage-role-assignments.md).
