---
title: Добавление слоя линии к карте | Карты Microsoft Azure
description: В этой статье вы узнаете, как добавить слой линии в карту с помощью веб-пакета SDK для карт Microsoft Azure.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 8503b12be628fe7d5651221c9d0379bee3e292bd
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76933468"
---
# <a name="add-a-line-layer-to-the-map"></a>Добавление слоя линии к сопоставлению

Слой линий можно использовать для отображения `LineString` и `MultiLineString` компонентов в виде путей или маршрутов на карте. Слой линий также можно использовать для отображения структуры `Polygon` и `MultiPolygon`ных функций. Источник данных подключен к слою линии, чтобы предоставить ему данные для подготовки к просмотру. 

> [!TIP]
> По умолчанию линии слоев отображают координаты многоугольников, а также линии в источнике данных. Чтобы ограничить слой таким образом, чтобы он отображал только компоненты LineString, задайте для свойства `filter` слоя значение `['==', ['geometry-type'], 'LineString']` или `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]`, если требуется также включить функции MultiLineString.

В следующем коде показано, как создать строку. Добавьте строку в источник данных, а затем выводите ее на слой линии с помощью класса [линелайер](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) .

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a line and add it to the data source.
dataSource.add(new atlas.data.LineString([[-73.972340, 40.743270], [-74.004420, 40.756800]]));
  
//Create a line layer to render the line to the map.
map.layers.add(new atlas.layer.LineLayer(dataSource, null, {
    strokeColor: 'blue',
    strokeWidth: 5
}));
```

Ниже приведен полный пример выполнения кода описанной выше функциональности.

<br/>

<iframe height='500' scrolling='no' title='Добавление линии на карту' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Просмотрите фрагмент кода <a href='https://codepen.io/azuremaps/pen/qomaKv/'>Добавление линии на карту</a> службы "Карты Azure" (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) в <a href='https://codepen.io'>CodePen</a>.
</iframe>

Слои линий можно отформатировать с помощью [линелайероптионс](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest) и [использовать выражения стилей](data-driven-style-expressions-web-sdk.md), основанные на данных.

## <a name="add-symbols-along-a-line"></a>Добавление символов вдоль линии

В этом примере показано, как добавить значки стрелок вдоль линии на карте. При использовании слоя символов задайте для параметра "размещение" значение "Line". Этот параметр позволяет отображать символы вдоль линии и вращать значки (0 градусов = вправо).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Показывать стрелку и линию" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
См. <a href='https://codepen.io/azuremaps/pen/drBJwX/'>стрелку показ пера и построчно</a> Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) на <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Azure Maps веб-пакет SDK предоставляет несколько настраиваемых шаблонов изображений, которые можно использовать с уровнем символов. Дополнительные сведения см. в документе [Использование шаблонов изображений](how-to-use-image-templates-web-sdk.md) .

<a name="line-stroke-gradient"></a>

## <a name="add-a-stroke-gradient-to-a-line"></a>Добавление градиента обводки в линию

К линии можно применить один цвет обводки. Можно также заполнить линию градиентом цветов, чтобы отобразить переход от сегмента строки к следующему сегменту линии. Например, градиенты линий можно использовать для представления изменений по времени и расстоянию, а также по различным температурам в подключенной строке объектов. Чтобы применить эту функцию к строке, источник данных должен иметь параметр `lineMetrics` со значением true, а затем выражение цвета градиента может быть передано в параметр `strokeColor` линии. Выражение градиента обводки должно ссылаться на `['line-progress']` выражение данных, которое предоставляет вычисляемые показатели строк выражению.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Линия с градиентом обводки" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
См. <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>линию пера с градиентом обводки</a> Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) на <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-line-layer"></a>Настройка слоя линий

Слой линий имеет несколько параметров стилей. которые можно опробовать с помощью представленного ниже средства.

<br/>

<iframe height='700' scrolling='no' title='Параметры слоя линий' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Просмотрите фрагмент кода с <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>параметрами слоя линий</a> службы Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) на сайте <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о классах и методах, которые используются в этой статье:

> [!div class="nextstepaction"]
> [линелайер](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) 

> [!div class="nextstepaction"]
> [линелайероптионс](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)

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
> [Добавление слоя многоугольников](map-add-shape.md)
