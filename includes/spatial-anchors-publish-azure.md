---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0dab71b6d169e26a3d7dc208dd09efe1143fbe13
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "67184976"
---
### <a name="open-the-publish-wizard"></a>Запуск мастера публикации

В **обозревателе решений** щелкните правой кнопкой мыши проект **SharingService** и выберите пункт **Опубликовать**.

Запустится мастер публикации. Выберите **Служба приложений** > **Опубликовать**, чтобы открыть диалоговое окно **Создание службы приложений**.

### <a name="sign-in-to-azure"></a>Вход в Azure

В диалоговом окне **Создание службы приложений** выберите **Добавить учетную запись** и войдите в подписку Azure. Если вы уже вошли, выберите нужную учетную запись из раскрывающегося списка.

> [!NOTE]
> Если вы уже выполнили вход, пока не нажимайте кнопку **Создать**.
>

### <a name="create-a-resource-group"></a>Создание группы ресурсов

[!INCLUDE [resource group intro text](resource-group.md)]

Рядом с **группой ресурсов** выберите команду **Создать**.

Присвойте группе ресурсов имя **myResourceGroup** и нажмите кнопку **ОК**.

### <a name="create-an-app-service-plan"></a>Создание плана службы приложений

[!INCLUDE [app-service-plan](app-service-plan.md)]

Рядом с полем **План размещения** выберите **Создать**.

В диалоговом окне **Настройка плана размещения** используйте следующие параметры:

| Параметр | Рекомендуемое значение | Description |
|-|-|-|
|План обслуживания приложения| MySharingServicePlan | Имя плана службы приложений. |
| Location | западная часть США | Центр обработки данных, где размещается веб-приложение. |
| Размер | Free | [Ценовая категория](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), которая определяет возможности размещения. |

Нажмите кнопку **ОК**.

### <a name="create-and-publish-the-web-app"></a>Создание и публикация веб-приложения

В поле **Имя приложения** введите уникальное имя (допустимые символы: `a-z`, `0-9` и `-`) или примите сгенерированное автоматически уникальное имя. URL-адрес веб-приложения: `https://<app_name>.azurewebsites.net`, где `<app_name>` — имя приложения.

Нажмите кнопку **Создать**, чтобы начать создавать ресурсы Azure.

Когда мастер завершит работу, веб-приложение ASP.NET Core будет опубликовано в Azure и запущено в браузере по умолчанию.

![Опубликованное веб-приложение ASP.NET в Azure](./media/spatial-anchors-azure/web-app-running-live.png)

Имя приложения из этого раздела используется в качестве префикса URL-адреса в формате `https://<app_name>.azurewebsites.net`. Запишите этот URL-адрес, так как он потребуется позднее.
