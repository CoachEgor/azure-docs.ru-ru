---
title: Расширение геометрических объектов GeoJSON в Azure Maps | Документация Майкрософт
description: Сведения о том, как расширить геометрические объекты GeoJSON в Azure Maps
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: be3c31951c4721a861f9239c5220419dec11b6bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799153"
---
# <a name="extending-geojson-geometries"></a>Расширение геометрических объектов GeoJSON

Azure Maps предоставляет список эффективных API-интерфейсов для поиска внутри географических функций или с их использованием.
Эти API-интерфейсы стандартизированы в [спецификации GeoJSON][1] для представления географических функций (например, границ штатов и маршрутов).  

[Спецификация GeoJSON][1] поддерживает только следующие геометрические объекты:

* GeometryCollection;
* LineString;
* MultiLineString;
* MultiPoint;
* MultiPolygon;
* Point;
* Polygon.

Некоторые API-интерфейсы Azure Maps (например: [Поиск внутри Geometry](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) примите геометрические объекты, такие как «Circle», которые не являются частью [спецификации GeoJSON][1].

В этой статье приводится подробное объяснение того, как Azure Maps расширяет [спецификацию GeoJSON][1] для представления определенных геометрических объектов.

### <a name="circle"></a>Круг

Геометрический объект `Circle` не поддерживается [спецификацией GeoJSON][1]. Для представления круга мы используем объект `GeoJSON Feature`.

Геометрический объект `Circle`, представленный с помощью объекта `GeoJSON Feature`, __должен__ содержать следующие параметры:

1. Центр.
   >Центр круга представлен с помощью типа `GeoJSON Point`.

2. Радиус.
   >Значение `radius` круга представлено с помощью свойств `GeoJSON Feature`. Значение радиуса отображается в _метрах_ и должно быть типа `double`.

3. SubType
   >Геометрический объект "круг" также должен содержать свойство `subType`. Оно должно входить в свойства `GeoJSON Feature`, и для него должно использоваться значение _Circle_.


#### <a name="example"></a>Пример

Вот как будет представлять центр в круг (широта: 47.639754, долготы:-122.126986) с радиусом равным 100 метров, с помощью `GeoJSON Feature` объекта:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

[1]: https://tools.ietf.org/html/rfc7946
