---
title: Как запросить данные в режиме реального времени в Azure Maps | Документация Майкрософт
description: Запросите данные в режиме реального времени с помощью службы мобильности Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 5f69bcafbc3abb39b4185e0144ff6029808c0704
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75432974"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Запрос данных в режиме реального времени с помощью службы мобильности Azure Maps

В этой статье показано, как использовать [службу Azure Maps Mobility Service](https://aka.ms/AzureMapsMobilityService) для запроса транзитных данных в режиме реального времени.

В этой статье раскрываются следующие темы:


 * Запросить следующие поступления в реальном времени для всех строк, поступающих на заданную точку
 * Запросите сведения в режиме реального времени для заданной станции закрепления велосипеда.


## <a name="prerequisites"></a>Технические условия

Для выполнения любых вызовов Azure Maps общедоступных транзитных API-интерфейсов требуется учетная запись Maps и ключ. Чтобы получить сведения о создании учетной записи и получении ключа, следуйте инструкциям в разделе [Создание учетной](quick-demo-map-app.md#create-an-account-with-azure-maps) записи для создания подписки на учетную запись Azure Maps и выполните действия, описанные в статье [Получение первичного ключа](quick-demo-map-app.md#get-the-primary-key-for-your-account) для получения первичного ключа для вашей учетной записи. Дополнительные сведения о проверке подлинности в Azure Maps см. в разделе [Управление проверкой подлинности в Azure Maps](./how-to-manage-authentication.md).


В этой статье для создания вызовов REST используется [приложение Postman](https://www.getpostman.com/apps). Вы можете использовать любую среду разработки API.


## <a name="request-real-time-arrivals-for-a-stop"></a>Запрос прибытия в режиме реального времени для завершения

Чтобы запросить данные о поступлении в режиме реального времени для определенной общей транзитной пересылки, необходимо выполнить запрос к [API прибытия в реальном времени](https://aka.ms/AzureMapsMobilityRealTimeArrivals) для [службы Azure Maps Mobility Service](https://aka.ms/AzureMapsMobilityService). Для выполнения запроса потребуются **Метроид** и **стопид** . Чтобы узнать больше о том, как запросить эти параметры, ознакомьтесь с нашим руководством по [запросу общих транзитных маршрутов](https://aka.ms/AMapsHowToGuidePublicTransitRouting). 

Давайте будем использовать "522" в качестве идентификатора Metro, который представляет собой идентификатор Metro для области "Сиэтл – Tacoma – Бельвью, WA", и используйте идентификатор "522---2060603", который является остановкой шины в "NE 24 St & 162nd Ave NE, Бельвью WA". Чтобы запросить следующие пять данных о поступлении в режиме реального времени для следующих прямых появления на этом этапе, выполните следующие действия.

1. Создайте коллекцию для хранения запросов. В приложении POST выберите **создать**. В окне **создать новое** выберите **коллекция**. Присвойте имя коллекции и нажмите кнопку **создать** .

2. Чтобы создать запрос, нажмите кнопку **создать** еще раз. В окне **создать новое** выберите **запрос**. Введите **имя запроса** для запроса, выберите коллекцию, созданную на предыдущем шаге, в качестве расположения для сохранения запроса, а затем нажмите кнопку **сохранить**.

    ![Создание запроса в POST](./media/how-to-request-transit-data/postman-new.png)

3. Выберите метод получения HTTP на вкладке Построитель и введите следующий URL-адрес для создания запроса GET.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

4. После успешного выполнения запроса вы получите следующий ответ.  Обратите внимание, что параметр "scheduleType" определяет, основывается ли предполагаемое время прибытия на основе данных в режиме реального времени или статических.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```


## <a name="real-time-data-for-bike-docking-station"></a>Данные в режиме реального времени для стыковочного узла велосипеда

[API получения сведений о Закреплениях](https://aka.ms/AzureMapsMobilityTransitDock) для службы Azure Maps Mobility Service позволяет запрашивать статическую информацию и сведения в режиме реального времени, такие как сведения о доступности и вакансии для заданной велосипедной станции или мотороллере стыковочного узла. Мы создадим запрос на получение данных в режиме реального времени для стыковочного узла велосипедов.

Чтобы выполнить запрос к API-интерфейсу получения сведений о DOCKER, вам потребуется **доккид** для этой станции. Вы можете получить идентификатор DOCKER, выполнив поисковый запрос к [API-интерфейсу Get поблизости](https://aka.ms/AzureMapsMobilityNearbyTransit) и задав для параметра **ObjectType** значение "бикедокк". Выполните следующие действия, чтобы получить данные в режиме реального времени для стыковочного узла велосипедов.


### <a name="get-dock-id"></a>Получить идентификатор DOCKER

Чтобы получить **доккид**, выполните следующие действия, чтобы выполнить запрос к API-интерфейсу получения ближайших транзитных:

1. В окне POST щелкните **новый запрос** | **получить запрос** и назовите его **Get DOCKER ID (получить идентификатор Dock**).

2.  На вкладке Построитель выберите метод **Get** HTTP, введите следующий URL-адрес запроса и нажмите кнопку **Отправить**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. После успешного выполнения запроса вы получите следующий ответ. Обратите внимание, что теперь у нас есть **идентификатор** в ответе, который можно использовать позже в качестве параметра запроса в API получения сведений о закреплениях.

    ```JSON
    {
        "results": [
            {
                "id": "121---4640799",
                "type": "bikeDock",
                "objectDetails": {
                    "availableVehicles": 0,
                    "vacantLocations": 31,
                    "lastUpdated": "2019-09-07T00:55:19Z",
                    "operatorInfo": {
                        "id": "121---80",
                        "name": "Citi Bike"
                    }
                },
                "position": {
                    "latitude": 40.767128,
                    "longitude": -73.962243
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 40.768039,
                        "longitude": -73.963413
                    },
                    "btmRightPoint": {
                        "latitude": 40.766216,
                        "longitude": -73.961072
                    }
                }
            }
        ]
    }
    ```


### <a name="get-real-time-bike-dock-status"></a>Получение состояния закрепления велосипеда в режиме реального времени

Выполните приведенные ниже действия, чтобы выполнить запрос к API получения сведений о закреплениях для получения данных в режиме реального времени для выбранной закрепления.

1. В окне POST щелкните **новый запрос** | **получить запрос** и назовите его **получить данные закрепления в режиме реального времени**.

2.  На вкладке Построитель выберите метод **Get** HTTP, введите следующий URL-адрес запроса и нажмите кнопку **Отправить**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. После успешного выполнения запроса вы получите ответ на следующую структуру:

    ```JSON
    {
        "availableVehicles": 0,
        "vacantLocations": 31,
        "position": {
            "latitude": 40.767128,
            "longitude": -73.962246
        },
        "lastUpdated": "2019-09-07T00:55:19Z",
        "operatorInfo": {
            "id": "121---80",
            "name": "Citi Bike"
        }
    }
    ```


## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как запросить транзитные данные с помощью службы Mobility Service:

> [!div class="nextstepaction"]
> [Запрос передачи данных](how-to-request-transit-data.md)

Изучите документацию по API службы Mobility Service Azure Maps.

> [!div class="nextstepaction"]
> [Документация по API службы Mobility Service](https://aka.ms/AzureMapsMobilityService)
