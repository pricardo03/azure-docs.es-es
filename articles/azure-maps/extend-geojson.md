---
title: Extender las geometrías GeoJSON en Azure Maps| Microsoft Docs
description: Obtenga información sobre cómo extender las geometrías GeoJSON en Azure Maps
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
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799139"
---
# <a name="extending-geojson-geometries"></a>Extender las geometrías GeoJSON

Azure Maps proporciona una lista de API eficaces para buscar dentro o a lo largo de características geográficas.
Estas API están estandarizadas en las [especificaciones de GeoJSON][1] para representar las características geográficas (por ejemplo: límites de estado, rutas).  

Las [especificaciones de GeoJSON][1] solo admiten las geometrías siguientes:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Point
* Polygon

Algunas API de Azure Maps (por ejemplo: [Búsqueda dentro de la geometría](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) aceptan geometrías como "Círculo", que no son parte de la [especificación GeoJSON][1].

Este artículo proporciona una explicación detallada sobre cómo Azure Maps amplía las [especificaciones de GeoJSON][1] para representar determinadas geometrías.

### <a name="circle"></a>Circle

La geometría `Circle` no es compatible con las [especificaciones de GeoJSON][1]. Usamos el objeto `GeoJSON Feature` para representar un círculo.

Una geometría `Circle` representada mediante el objeto `GeoJSON Feature` __debe__ contener lo siguiente:

1. Center
   >El centro del círculo se representa mediante un tipo `GeoJSON Point`.

2. Radio
   >`radius` se representa mediante las propiedades de `GeoJSON Feature`. El valor del radio está en _metros_ y debe ser del tipo `double`.

3. SubType
   >La geometría del círculo también debe contener la propiedad `subType`. Esta propiedad debe formar parte de las propiedades de `GeoJSON Feature` y su valor debe ser _Circle_


#### <a name="example"></a>Ejemplo

Aquí es cómo podrá representar un círculo centrado en (latitud: 47.639754, longitud:-122.126986) con un radio igual a 100 metros, con un `GeoJSON Feature` objeto:

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
