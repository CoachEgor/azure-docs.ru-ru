---
title: Отображение направлений маршрута на карте | Карты Microsoft Azure
description: В этой статье вы узнаете, как отображать направления между двумя расположениями на карте с помощью веб-пакета SDK Microsoft Azure Maps.
author: Philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 1cde1aaa7c9dba3e28407439a46b0e0a3326e4fc
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123978"
---
# <a name="show-directions-from-a-to-b"></a>Отображение направлений от точки А до точки Б

В этой статье объясняется, как выполнить запрос маршрута и отобразить маршрут на карте.

Это можно сделать двумя способами. Во-первых — запрос к [API маршрута Azure Maps](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) через модуль службы. Второй способ — использовать [API выборки](https://fetch.spec.whatwg.org/) , чтобы выполнить поисковый запрос к [Azure Maps API маршрута](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Оба способа описаны ниже.

## <a name="query-the-route-via-service-module"></a>Запрос маршрута с помощью модуля службы

<iframe height='500' scrolling='no' title='Отображение направлений от точки А до точки Б на карте (модуль службы)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Просмотрите фрагмент кода <a href='https://codepen.io/azuremaps/pen/RBZbep/'>Отображение направлений от точки А до точки Б на карте (модуль службы)</a> для службы Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) на сайте <a href='https://codepen.io'>CodePen</a>.
</iframe>

В приведенном выше коде первый блок конструирует объект Map и устанавливает механизм проверки подлинности для использования маркера доступа. См. инструкции по [созданию карты](./map-create.md).

Второй блок кода создает `TokenCredential` для проверки подлинности HTTP-запросов, Azure Maps с маркером доступа. Затем он передает `TokenCredential` в `atlas.service.MapsURL.newPipeline()` и создает экземпляр [конвейера](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) . `routeURL` представляет собой URL-адрес для операций [маршрута](https://docs.microsoft.com/rest/api/maps/route) Azure Maps.

Третий блок кода создает и добавляет объект [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) к сопоставлению.

Четвертый блок кода создает объекты начальной и конечной [точек](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) и добавляет их в объект DataSource.

Линия — это [функция](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) для LineString. [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) отвечает за отображение линейных объектов в оболочке [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) в качестве линий на карте. Четвертый блок кода позволяет создать и добавить на карту слой линий. Ознакомьтесь со свойствами слоя линий в разделе о классе [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

[Слой символов](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) использует текст или значки для визуализации данных на основе точек, инкапсулированных в [источнике](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)данных. Текст или значки отображаются в виде символов на карте. Пятый блок кода создает и добавляет на карту слой символов.

Шестой блок кода запрашивает службу маршрутизации Azure Maps, которая является частью [модуля службы](how-to-use-services-module.md). Метод [калкулатераутедиректионс](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest#methods) в RouteUrl используется для получения маршрута между начальной и конечной точками. После этого коллекция функций геоjson из ответа извлекается с помощью `geojson.getFeatures()` метода и добавляется в источник данных. После этого ответ отображается в виде маршрута на карте. Дополнительные сведения о добавлении линии на карту см. [здесь](map-add-line-layer.md).

Последний блок кода задает границы сопоставлений с помощью свойства [сеткамера](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) на карте.

Запрос маршрута, источник данных, символ, уровни линии и границы камеры создаются внутри [прослушивателя событий](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events). Эта структура кода гарантирует, что результаты будут отображаться только после полной загрузки схемы.

## <a name="query-the-route-via-fetch-api"></a>Отправка запроса через API FETCH

<iframe height='500' scrolling='no' title='Отображение направлений от точки А до точки Б на карте' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Просмотрите фрагмент кода <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Отображение направлений от точки А до точки Б на карте</a> службы "Карты Azure" (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) в <a href='https://codepen.io'>CodePen</a>.
</iframe>

В приведенном выше коде первый блок кода конструирует объект Map и устанавливает механизм проверки подлинности для использования маркера доступа. См. инструкции по [созданию карты](./map-create.md).

Второй блок кода создает и добавляет на карту объект [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest).

Третий блок кода создает начальную и конечную точки для маршрута. Затем он добавляет их в источник данных. Инструкции по использованию [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) см. в разделе о [добавлении булавки на карту](map-add-pin.md).

[LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) отвечает за отображение линейных объектов в оболочке [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) в качестве линий на карте. Четвертый блок кода позволяет создать и добавить на карту слой линий. Ознакомьтесь со свойствами слоя линий в разделе о классе [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

В [слое символов](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) данные на основе точек в оболочке [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) преобразовываются для просмотра в качестве символов на карте с помощью текста или значков. Пятый блок кода создает и добавляет на карту слой символов. Сведения о свойствах слоя символов см. в разделе [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest).

Следующий блок кода создает точки `SouthWest` и `NorthEast` от точек начала и назначения и задает границы карты с помощью свойства карты [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-).

Последний блок кода использует [API выборки](https://fetch.spec.whatwg.org/) для выполнения запроса поиска в [Azure Maps API маршрута](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Затем выполняется синтаксический анализ ответа. Если ответ был успешным, сведения широты и долготы используются для создания массива с линией, соединяющей эти точки. Затем данные строки добавляются в источник данных для отрисовки маршрута на карте. См. инструкции по [добавлению линий на карту](map-add-line-layer.md).

Запрос маршрута, источник данных, символ, уровни линии и границы камеры создаются внутри [прослушивателя событий](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events). Опять же, мы хотим убедиться, что результаты отображаются после полной загрузки карт.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Рекомендации по использованию службы построения маршрутов](how-to-use-best-practices-for-search.md)

Дополнительные сведения о классах и методах, которые используются в этой статье:

> [!div class="nextstepaction"]
> [Схема](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Полные примеры кода см. в следующих статьях:

> [!div class="nextstepaction"]
> [Отображение данных дорожного движения на карте](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Взаимодействие с событиями Map-Mouse](./map-events.md)
