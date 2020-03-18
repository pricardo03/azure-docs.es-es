---
title: Conexión a un servicio Web Feature Service (WFS) | Microsoft Azure Maps
description: Obtenga información sobre cómo conectarse a un servicio WFS y consultar después el servicio WFS con el SDK web de Azure Maps y el módulo de E/S espacial.
author: farah-alyasari
ms.author: v-faalya
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 18ac583837c7cb8b2dabbfa6f7d7210c8afe3fcb
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78402757"
---
# <a name="connect-to-a-wfs-service"></a>Conexión a un servicio WFS

Web Feature Service (WFS) es un servicio web para la consulta de datos espaciales que tiene una API estandarizada, que se ha definido en Open Geospatial Consortium (OGC). La clase `WfsClient` del módulo de E/S espacial permite a los desarrolladores conectarse a un servicio WFS y consultar los datos del servicio.

La clase `WfsClient` admite las características siguientes:

- Versiones compatibles: `1.0.0`, `1.1.0` y `2.0.0`
- Operadores de filtro admitidos: comparaciones binarias, lógicos, matemáticos, de valor y `bbox`.
- Las solicitudes solo se realizan mediante `HTTP GET`.
- Operaciones compatibles:

    | | |
    | :-- | :-- |
    | GetCapabilities | Genera un documento de metadatos con operaciones y parámetros de WFS válidos |
    | GetFeature | Devuelve una selección de características de un origen de datos |
    | DescribeFeatureType | Devuelve los tipos de características admitidos |

## <a name="using-the-wfs-client"></a>Uso del cliente de WFS

La clase `atlas.io.ogc.WfsClient` del módulo de E/S espacial facilita la consulta de un servicio WFS y la conversión de las respuestas en objetos GeoJSON. Este objeto GeoJSON se puede usar para otros fines de asignación.

El código siguiente consulta un servicio WFS y representa las características devueltas en el mapa.

<br/>

<iframe height='700' scrolling='no' title='Ejemplo de WFS simple' src='//codepen.io/azuremaps/embed/MWwvVYY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el lápiz <a href='https://codepen.io/azuremaps/pen/MWwvVYY/'>Ejemplo de WFS simple</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="supported-filters"></a>Filtros admitidos

La especificación del estándar WFS usa filtros OGC. Los siguientes filtros son compatibles con el cliente WFS, suponiendo que el servicio al que se llama también admita estos filtros. Las cadenas de filtro personalizado se pueden pasar en la clase `CustomFilter`.

**Operadores lógicos**

- `And`
- `Or`
- `Not`

**Operadores de valor**

- `GmlObjectId`
- `ResourceId`

**Operadores matemáticos**

- `Add`
- `Sub`
- `Mul`
- `Div`

**Operadores de comparación**

- `PropertyIsEqualTo`
- `PropertyIsNotEqualTo`
- `PropertyIsLessThan`
- `PropertyIsGreaterThan`
- `PropertyIsLessThanOrEqualTo`
- `PropertyIsGreaterThanOrEqualTo`
- `PropertyIsLike`
- `PropertyIsNull`
- `PropertyIsNil`
- `PropertyIsBetween`

En el código siguiente se muestra el uso de distintos filtros con el cliente de WFS.

<br/>

<iframe height='500' scrolling='no' title= 'Ejemplos de filtros de WFS' src='//codepen.io/azuremaps/embed/NWqvYrV/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Vea el lápiz <a href='https://codepen.io/azuremaps/pen/NWqvYrV/'>Ejemplos de filtros de WFS</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="wfs-service-explorer"></a>Explorador de servicios WFS

En el código siguiente se usa el cliente de WFS para explorar los servicios WFS. Seleccione una capa de tipo de propiedad dentro del servicio y vea la leyenda asociada.

<br/>

<iframe height='700' style='width: 100%;' scrolling='no' title= 'Explorador de servicios WFS' src='//codepen.io/azuremaps/embed/bGdrvmG/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Vea el lápiz <a href='https://codepen.io/azuremaps/pen/bGdrvmG/'>Explorador de servicios WFS</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

También puede usar un servicio de proxy para cargar los recursos que se hospedan en los dominios que no están habilitados para CORs. En primer lugar, debe definir una variable que contenga la dirección URL del servicio proxy y establecer la opción `proxyService` para el cliente de WFS. Para representar una opción de servicio de proxy para el usuario, agregue una entrada de usuario a la interfaz de usuario. Cargue la dirección URL del servicio cuando se haga clic en la entrada. Los fragmentos de código siguientes muestran cómo usar el servicio de proxy.

```JavaScript

//A variable to hold the URL of the proxy service
var proxyServiceUrl = window.location.origin + 'CorsEnabledProxyService.ashx?url=';

//Create the WFS client to access the service and use the proxy service settings
client = new atlas.io.ogc.WfsClient({
    url: url,
    proxyService: (document.getElementById('useProxyService').checked) ? proxyServiceUrl : null
});

function proxyOptionChanged() {
    if (currentServiceUrl) {
        loadClient(currentServiceUrl);
    }
}

```

El siguiente fragmento de código HTML corresponde al código JavaScript anterior:

```html
<!-- use the proxy service -->
<input id="useProxyService" type="checkbox" onclick="proxyOptionChanged()"/>
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [WfsClient](https://docs.microsoft.com/JavaScript/api/azure-maps-spatial-io/atlas.io.ogc.wfsclient)

> [!div class="nextstepaction"]
> [WfsServiceOptions](https://docs.microsoft.com/JavaScript/api/azure-maps-spatial-io/atlas.wfsserviceoptions)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Aprovechamiento de las operaciones básicas](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Detalles de formatos de datos admitidos](spatial-io-supported-data-format-details.md)
