---
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: include
ms.date: 1/31/2020
ms.openlocfilehash: c344d7e1aa1f6d45131295ba9aad1294c5ba548c
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2020
ms.locfileid: "96930749"
---
1. Чтобы создать хранилище Конфигурации приложений, войдите на [портал Azure](https://portal.azure.com). Выберите **Создать ресурс** в левом верхнем углу домашней страницы. В поле **Поиск в Marketplace** введите *Конфигурация приложений* и нажмите клавишу <kbd>ВВОД</kbd>.

    ![Поиск службы конфигурации приложений](media/azure-app-configuration-create/azure-portal-search.png)

1. В результатах поиска выберите **Конфигурация приложений** и щелкните **Создать**.

    ![Нажмите кнопку "Создать"](media/azure-app-configuration-create/azure-portal-app-configuration-create.png)

1. В области **создания Конфигурации приложений** введите следующие параметры:

    | Параметр | Рекомендуемое значение | Description |
    |---|---|---|
    | **Подписка** | Ваша подписка | Выберите подписку Azure, которую нужно использовать для тестирования конфигурации приложений. Если в вашей учетной записи есть только одна подписка, она выбирается автоматически, а список **Подписка** не отображается. |
    | **Группа ресурсов** | *AppConfigTestResources* | Выберите или создайте группу ресурсов для ресурса хранилища Конфигурации приложений. Эта группа может быть полезна для упорядочения нескольких ресурсов, которые может потребоваться удалить одновременно путем удаления группы ресурсов. Дополнительные сведения см. в статье [Управление ресурсами Azure через портал](../articles/azure-resource-manager/management/overview.md). |
    | **Имя ресурса** | Глобально уникальное имя | Введите уникальное имя для ресурса хранилища Конфигурации приложений. Имя должно быть строкой длиной от 5 до 50 символов и содержать только цифры, буквы и символ `-`. Имя не может начинаться или заканчиваться символом `-`. |
    | **Местоположение** | *Центральная часть США* | В поле **Расположение** укажите, где будет географически располагаться хранилище конфигураций приложения. Для наилучшей производительности создайте ресурс в одном регионе с другими компонентами приложения. |
    | **Ценовая категория** | *Бесплатный* | Выберите нужную ценовую категорию. Дополнительные сведения см.на [странице с ценами на Конфигурацию приложений](https://azure.microsoft.com/pricing/details/app-configuration). |

1. Чтобы проверить параметры, выберите **Просмотр и создание**.

1. Нажмите кнопку **Создать**. Развертывание может занять несколько минут.

1. По завершении развертывания перейдите к ресурсу Конфигурации приложений. Выберите **Параметры** > **Ключи доступа**. Запишите первичный ключ строки подключения только для чтения. Она пригодится позже для настройки обмена данными между приложением и созданным хранилищем Конфигурации приложений.