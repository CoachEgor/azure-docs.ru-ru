---
title: Руководство по Поиск маршрута к местоположению | Microsoft Azure Maps
description: В этом руководстве показано, как отобразить маршрут к местоположению (точку интереса) на карте с помощью службы построения маршрутов в Microsoft Azure Maps.
author: philmea
ms.author: philmea
ms.date: 01/14/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 98c36176ecd2996e5f735c52017162a076ef4bde
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80333764"
---
# <a name="tutorial-route-to-a-point-of-interest-using-azure-maps"></a>Руководство по Поиск маршрута к точке интереса с помощью службы "Карты Azure"

В этом руководстве показано, как использовать учетную запись службы "Карты Azure" и пакет SDK службы построения маршрутов, чтобы найти маршрут к объекту. В этом руководстве описано следующее:

> [!div class="checklist"]
> * создание веб-страницы с помощью API элементов управления картой;
> * Настройка координат адреса.
> * Отправка запроса к службе построения маршрутов поиска маршрута к объекту.

## <a name="prerequisites"></a>Предварительные требования

Перед тем, как продолжить работу, выполните шаги в разделе [Создание учетной записи службы "Карты Azure"](quick-demo-map-app.md#create-an-account-with-azure-maps). Для этого вам понадобится подписка с ценовой категорией S1. Выполните действия, описанные в разделе [Получение первичного ключа для учетной записи](quick-demo-map-app.md#get-the-primary-key-for-your-account), чтобы получить первичный ключ для вашей учетной записи. Дополнительные сведения о проверке подлинности в Azure Maps см. в [этой статье](how-to-manage-authentication.md).

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>Создание карты

Чтобы создать статическую HTML-страницу со встроенным API элементов управления картой, сделайте следующее.

1. На локальном компьютере создайте файл **MapRoute.html**.
2. Добавьте в него следующие компоненты HTML.

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Route</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/service/2/atlas-service.min.js"></script>

        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```

    Обратите внимание на то, что заголовок HTML содержит файлы ресурсов CSS и JavaScript, размещенные в библиотеке Azure Map Control. Обратите внимание на событие `onload` в тексте страницы, которое вызовет функцию `GetMap` после загрузки текста страницы. Эта функция будет содержать внутренний код JavaScript для доступа к интерфейсам службы Azure Maps. 

3. Добавьте следующий код JavaScript в функцию `GetMap`. Замените строку `<Your Azure Maps Key>` первичным ключом, скопированным из учетной записи службы Maps.

    ```JavaScript
   //Instantiate a map object
   var map = new atlas.Map("myMap", {
        //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
        authOptions: {
           authType: 'subscriptionKey',
           subscriptionKey: '<Your Azure Maps Key>'
        }
   });
   ```

    `atlas.Map` предоставляет элемент управления для визуальной интерактивной веб-карты и является компонентом API Azure Map Control.

4. Сохраните файл и откройте его в браузере. На этом этапе у вас есть базовая карта, которую можно будет дополнительно доработать.

   ![Просмотр базовой карты](media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Определение способа отображения маршрута

В этом руководстве простой маршрут будет отображаться с использованием значков символов для обозначения начальной и конечной точек и линии для обозначения пути маршрута.

1. После инициализации карты добавьте следующий код JavaScript.

    ```JavaScript
    //Wait until the map resources are ready.
    map.events.add('ready', function() {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: '#2272B9',
            strokeWidth: 5,
            lineJoin: 'round',
            lineCap: 'round'
        }), 'labels');

        //Add a layer for rendering point data.
        map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: ['get', 'icon'],
                allowOverlap: true
           },
            textOptions: {
                textField: ['get', 'title'],
                offset: [0, 1.2]
            },
            filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']] //Only render Point or MultiPoints in this layer.
        }));
    });
    ```
    
    В обработчике событий карт `ready` создается источник данных для хранения линий маршрута, а также начальной и конечной точек. Слой линий создается и привязывается к источнику данных, чтобы определить, как будет отображаться линия маршрута. Линия маршрута будет выполнена в красивом оттенке синего. Она будет иметь ширину пять пикселей, скругленные линейные соединения и ограничения. При добавлении слоя карты передается второй параметр со значением `'labels'`, в котором указывается, что этот слой будет отображаться под метками карты. Это гарантирует, что линия маршрута не будет закрывать дорожные метки. Слой символов создается и привязывается к источнику данных. Этот слой определяет, как отображаются начальная и конечная точки. В этом случае выражения добавлены для извлечения изображения значка и текстовых подписей из свойств в каждом объекте точки. 
    
2. В этом руководстве задайте в качестве начальной точки офис корпорации Майкрософт, а в качестве конечной — заправку в Сиэтле. В обработчике событий карт `ready` добавьте указанный ниже код.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end points of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: "Redmond",
        icon: "pin-blue"
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: "Seattle",
        icon: "pin-round-blue"
    });

    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 80
    });
    ```

    Этот код создает два [объекта GeoJSON Point](https://en.wikipedia.org/wiki/GeoJSON) для представления начальной и конечной точек маршрута и добавляет точки к источнику данных. В каждую точку добавляется свойство `title` и `icon`. Последний блок задает позицию камеры используя данные широты и долготы начальной и конечной точек с помощью свойства карты [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-).

3. Сохраните файл **MapRoute.html** и обновите страницу в браузере. Теперь на карте будет крупным планом показан Сиэтл. Вы можете видеть голубую пометку, обозначающую начальную точку, и круглую голубую пометку, обозначающую конечную точку.

   ![Просмотр начальной и конечной точки маршрута на карте](media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Получение направлений

В этом разделе показано, как использовать API службы маршрутов Azure Maps. API службы маршрутов находит маршрут от заданной начальной точки до конечной точки. В службе есть интерфейсы API для планирования самого *быстрого*, *краткого*, *экономичного* или *захватывающего* маршрута между двумя расположениями. Эта служба также позволяет пользователям планировать маршруты в будущем с помощью обширной базы данных Azure, содержащей исторический трафик. Пользователи могут видеть прогноз продолжительности маршрута в любой выбранный день и время. Дополнительные сведения см. в статье [Route — Get Route Directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Маршрут. Получение направления маршрута). Все приведенные ниже функции необходимо добавить **в блок загрузки карты eventListener**. Это позволит гарантировать их загрузку после подготовки ресурсов карты для доступа.

1. В функцию GetMap в коде JavaScript добавьте следующее.

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   `SubscriptionKeyCredential` создает политику `SubscriptionKeyCredentialPolicy` для аутентификации HTTP-запросов для Azure Maps с помощью ключа подписки. `atlas.service.MapsURL.newPipeline()` принимает политику `SubscriptionKeyCredential` и создает экземпляр [конвейера](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest). `routeURL` представляет собой URL-адрес для операций [маршрута](https://docs.microsoft.com/rest/api/maps/route) Azure Maps.

2. После настройки учетных данных и URL-адреса добавьте следующий код JavaScript для создания маршрута из начальной точки в конечную. `routeURL` запрашивает службу маршрутов Azure Maps, чтобы рассчитать направления маршрута. Коллекция компонентов GeoJSON из ответа извлекается с помощью метода `geojson.getFeatures()` и добавляется в источник данных.

    ```JavaScript
    //Start and end point input to the routeURL
    var coordinates= [[startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]], [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]];

    //Make a search route request
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {
        //Get data features from response
        var data = directions.geojson.getFeatures();
        datasource.add(data);
    });
    ```

3. Сохраните файл **MapRoute.html** и обновите страницу в браузере. Для успешного подключения к интерфейсам API службы "Карты Azure" должна использоваться карта следующего вида.

    ![Azure Map Control и служба построения маршрутов](./media/tutorial-route-location/map-route.png)

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как выполнять следующие задачи:

> [!div class="checklist"]
> * создание веб-страницы с помощью API элементов управления картой;
> * Настройка координат адреса.
> * Отправка запроса к службе построения маршрутов для поиска маршрута к точке интереса.

> [!div class="nextstepaction"]
> [Просмотр полного исходного кода](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)

> [!div class="nextstepaction"]
> [Просмотр примера, работающего в реальном времени](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)

В следующем руководстве показано, как создать запрос на поиск маршрута с такими ограничениями, как вид транспорта или тип груза, а затем отобразить несколько маршрутов на одной карте.

> [!div class="nextstepaction"]
> [Поиск маршрутов для различных способов перемещения с помощью службы "Карты Azure"](./tutorial-prioritized-routes.md)
