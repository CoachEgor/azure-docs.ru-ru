---
title: Добавление слоя символов к карте | Карты Microsoft Azure
description: В этой статье вы узнаете, как использовать слой символов для настройки и добавления символов на карте с помощью веб-пакета SDK для карт Microsoft Azure.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 8c39c7b57167d65dfa639d41665f5d5b38110183
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76933129"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Добавление слоя символов на карту

Символ, подключенный к источнику данных и используемый для отрисовки значка и (или) текста в заданной точке. Слои символов подготавливаются к просмотру с помощью WebGL и используются для отрисовки больших коллекций точек на карте. По сравнению с HTML-маркером, слой символов отображает большое количество данных точек на карте с более высокой производительностью. Однако слой символов не поддерживает традиционные элементы CSS и HTML для стилей.  

> [!TIP]
> Слои символов по умолчанию отображают координаты всех геометрических объектов в источнике данных. Чтобы ограничить слой таким образом, чтобы он отображал только возможности геометрических точек, задайте для свойства `filter` слоя значение `['==', ['geometry-type'], 'Point']` или `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` при необходимости можно включить также функции MultiPoint.

Диспетчер спрайтов Maps загружает пользовательские изображения, используемые на уровне символов. Он поддерживает следующие форматы изображений:

- JPEG
- PNG
- SVG
- BMP
- GIF (без анимации)

## <a name="add-a-symbol-layer"></a>Добавление слоя символов

Перед добавлением на карту слоя символов необходимо выполнить несколько действий. Сначала создайте источник данных и добавьте его на карту. Затем можно создать и передать в источнике данных слой символов для получения данных из источника данных. Наконец, необходимо добавить данные в источник данных, чтобы обеспечить визуализацию. В следующем коде показан код, который должен быть добавлен к карте после его загрузки. Код отображает одну точку на карте с помощью слоя символов. 

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a symbol layer to render icons and/or text at points on the map.
var layer = new atlas.layer.SymbolLayer(dataSource);

//Add the layer to the map.
map.layers.add(layer);

//Create a point and add it to the data source.
dataSource.add(new atlas.data.Point([0, 0]));
```

Существует четыре различных типа данных точек, которые можно добавить на карту:

- Геометрическая точка в формате JSON. Этот объект содержит только координаты точки и ничего другого. Класс поддержки `atlas.data.Point` можно использовать для простого создания этих объектов.
- Геометрическая геометрия MultiPoint — этот объект содержит координаты нескольких точек и ничего другого. Класс поддержки `atlas.data.MultiPoint` можно использовать для простого создания этих объектов.
- Функция геоjson — этот объект состоит из любой геометрической фигуры и набора свойств, содержащих метаданные, связанные с геометрическим объектом. Класс поддержки `atlas.data.Feature` можно использовать для простого создания этих объектов.
- класс `atlas.Shape` похож на функцию геоjson. Оба состоят из геометрической геометрии и набора свойств, содержащих метаданные, связанные с геометрическим объектом. Если объект геоjson добавляется в источник данных, его можно легко отобразить в слое. Однако при обновлении свойства координаты этого объекта геоjson источник данных и схема не изменяются. Это обусловлено тем, что в объекте JSON нет механизма для активации обновления. Класс Shape предоставляет функции для обновления содержащихся в ней данных. При внесении изменений источник данных и схема автоматически уведомляются и обновляются. 

В следующем примере кода создается геометрическая точка в формате JSON и передается в класс `atlas.Shape`, чтобы упростить его обновление. Центр схемы изначально используется для отрисовки символа. На карту добавляется событие щелчка, чтобы при его срабатывании координаты мыши использовались с фигурами `setCoordinates` функции. Координаты мыши записываются во время события щелчка. Затем `setCoordinates` обновляет расположение символа на карте.

<br/>

<iframe height='500' scrolling='no' title='Переключение расположения маркера' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Просмотрите фрагмент кода <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>Переключение расположения маркера</a> от Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) в <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> По умолчанию слои символов оптимизируют отрисовку символов путем скрытия перекрывающихся символов. При изменении масштаба скрытые символы становятся видимыми. Чтобы отключить эту функцию и вывести все символы в любое время, задайте для свойства `allowOverlap` параметров `iconOptions` значение `true`.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Добавление пользовательского значка в слой символов

Слои символов преобразовываются для просмотра с помощью WebGL. Таким образом, все ресурсы (например, образы значков) необходимо загрузить в контекст WebGL. В этом примере показано, как добавить пользовательский значок в ресурсы Map. Этот значок используется для отрисовки данных точек с помощью пользовательского символа на карте. Свойство `textField` слоя символа требует указания выражения. В этом случае нам нужно отобразить свойство температуры. Так как температура является числом, ее необходимо преобразовать в строку. Кроме того, мы хотим добавить к нему «° F». Для этого объединения можно использовать выражение. `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Значок изображения пользовательских символов' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>См. ручку <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>Custom Symbol Image Icon</a> (Значок изображения пользовательских символов) от Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) в <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Azure Maps веб-пакет SDK предоставляет несколько настраиваемых шаблонов изображений, которые можно использовать с уровнем символов. Дополнительные сведения см. в документе [Использование шаблонов изображений](how-to-use-image-templates-web-sdk.md) .

## <a name="customize-a-symbol-layer"></a>Настройка слоя символа 

Слой символа имеет множество доступных вариантов стилизации. Вот средство для проверки этих различных параметров.

<br/>

<iframe height='700' scrolling='no' title='Параметры слоя символов' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Просмотрите ручку <a href='https://codepen.io/azuremaps/pen/PxVXje/'>Symbol Layer Options</a> (Параметры слоя символов) от Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) в <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Если требуется отобразить только текст с помощью слоя символов, можно скрыть значок, задав для свойства `image` параметров значка значение `'none'`.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о классах и методах, которые используются в этой статье:

> [!div class="nextstepaction"]
> [SymbolLayer class](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) (Класс SymbolLayer)

> [!div class="nextstepaction"]
> [SymbolLayerOptions interface](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest) (Интерфейс SymbolLayerOptions)

> [!div class="nextstepaction"]
> [IconOptions interface](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest) (Интерфейс IconOptions)

> [!div class="nextstepaction"]
> [текстоптионс](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

Дополнительные примеры кода для добавления в карты см. в следующих статьях:

> [!div class="nextstepaction"]
> [Создание источника данных](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Добавление всплывающего окна](map-add-popup.md)

> [!div class="nextstepaction"]
> [Использование стилистических выражений на основе данных](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Использование шаблонов изображений](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Добавить слой линий](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Добавление слоя многоугольников](map-add-shape.md)

> [!div class="nextstepaction"]
> [Добавление слоя пузырьков](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Добавление слоя пузырьков на карту](map-add-bubble-layer.md)
