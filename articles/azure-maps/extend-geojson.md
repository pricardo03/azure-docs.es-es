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
ms.openlocfilehash: 319f9cba23d088553f361b6a0d648bbde94e0743
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968568"
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

Algunas API de Azure Maps (por ejemplo: [geometría Search Inside](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) aceptan geometrías como "Circle", que no son parte de las [especificaciones de GeoJSON][1].

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

Así es cómo se podrá representar un círculo centrado en (latitud: 47.639754, longitud:-122.126986) con un radio igual a 100 metros, mediante un objeto `GeoJSON Feature`:

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
