---
title: Geometrías GeoJSON extendidas en Azure Maps | Microsoft Docs
description: Obtenga información sobre cómo extender las geometrías GeoJSON en Azure Maps
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 52325248d21a5d5112c9a7f9497c3e03fdf102a4
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881979"
---
# <a name="extended-geojson-geometries"></a>Geometrías GeoJSON extendidas

Azure Maps proporciona una lista de API eficaces para buscar dentro o a lo largo de características geográficas.
Estas API están estandarizadas en las [especificaciones de GeoJSON][1] para representar las características geográficas (por ejemplo, los límites de estado o las rutas).  

Las [especificaciones de GeoJSON][1] solo admiten las geometrías siguientes:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Point
* Polygon

Algunas API de Azure Maps (por ejemplo: [Search Inside Geometry](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) aceptan geometrías como "Circle", que no son parte de las [especificaciones de GeoJSON][1].

Este artículo proporciona una explicación detallada sobre cómo Azure Maps amplía las [especificaciones de GeoJSON][1] para representar determinadas geometrías.

## <a name="circle"></a>Circle

La geometría `Circle` no es compatible con las [especificaciones de GeoJSON][1]. Usamos el objeto `GeoJSON Point Feature` para representar un círculo.

Una geometría `Circle` representada mediante el objeto `GeoJSON Feature` __debe__ contener lo siguiente:

- Center

    El centro del círculo se representa mediante un objeto `GeoJSON Point`.

- Radio

    `radius` se representa mediante las propiedades de `GeoJSON Feature`. El valor del radio está en _metros_ y debe ser del tipo `double`.

- SubType

    La geometría del círculo también debe contener la propiedad `subType`. Esta propiedad debe formar parte de las propiedades de `GeoJSON Feature` y su valor debe ser _Circle_.

#### <a name="example"></a>Ejemplo

Así es cómo representará un círculo centrado en (latitud: 47.639754, longitud:-122.126986) con un radio igual a 100 metros, mediante un objeto `GeoJSON Feature`:

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

## <a name="rectangle"></a>Rectángulo

La geometría `Rectangle` no es compatible con las [especificaciones de GeoJSON][1]. Usamos el objeto `GeoJSON Polygon Feature` para representar un rectángulo. La extensión de rectángulo se usa principalmente en el módulo de herramientas de dibujo del SDK para web.

Una geometría `Rectangle` representada mediante el objeto `GeoJSON Polygon Feature` __debe__ contener lo siguiente:

- Esquinas

    Las esquinas del rectángulo se representan mediante las coordenadas de un objeto `GeoJSON Polygon`. Debería haber cinco coordenadas, una para cada esquina, y una quinta que sea igual que la primera para cerrar el anillo del polígono. Se asumirá que estas coordenadas se alinean y se giran según desee el desarrollador.

- SubType

    La geometría del rectángulo también debe contener la propiedad `subType`. Esta propiedad debe formar parte de las propiedades de `GeoJSON Feature` y su valor debe ser _Rectangle_.

### <a name="example"></a>Ejemplo

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [[[5,25],[14,25],[14,29],[5,29],[5,25]]]
    },
    "properties": {
        "subType": "Rectangle"
    }
}

```
## <a name="next-steps"></a>Pasos siguientes

Más información sobre los datos de GeoJSON en Azure Maps:

> [!div class="nextstepaction"]
> [Formato de Geofence GeoJSON](geofence-geojson.md)

Revise el glosario de términos técnicos comunes asociados a las aplicaciones de Azure Maps y de inteligencia de ubicación:

> [!div class="nextstepaction"]
> [Glosario de Azure Maps](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
