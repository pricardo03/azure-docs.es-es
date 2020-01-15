---
title: Migración de servicios web desde Google Maps | Microsoft Docs
description: Tutorial sobre cómo migrar servicios web desde Google Maps a Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 8fc3270d15f77576ff353a110d8a3a9a17c7144d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75474998"
---
# <a name="migrate-web-service-from-google-maps"></a>Migración de un servicio web desde Google Maps

Tanto Azure como Google Maps proporcionan acceso a API espaciales a través de servicios web REST. Las interfaces de API de estas plataformas realizan funcionalidades similares, pero emplean convenciones de nomenclatura y objetos de respuesta diferentes.

En la siguiente tabla se indican las API de servicio de Azure Maps que proporcionan una funcionalidad similar a las API de servicio de Google Maps enumeradas.

| API de servicio de Google Maps | API de servicio de Azure Maps                                                                      |
|-------------------------|---------------------------------------------------------------------------------------------|
| Direcciones              | [Route](https://docs.microsoft.com/rest/api/maps/route)                               |
| Matriz de distancia         | [Matriz de ruta](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) |
| Codificación geográfica               | [Búsqueda](https://docs.microsoft.com/rest/api/maps/search)                             |
| Búsqueda de lugares           | [Búsqueda](https://docs.microsoft.com/rest/api/maps/search)                             |
| Autocompletar para lugares      | [Búsqueda](https://docs.microsoft.com/rest/api/maps/search)                             |
| Mapa estático              | [Render](https://docs.microsoft.com/rest/api/maps/render/getmapimage)                 |
| Zona horaria               | [Zona horaria](https://docs.microsoft.com/rest/api/maps/timezone)                        |

Las siguientes API de servicio no están disponibles actualmente en Azure Maps:

- Elevation
- Geolocalización
- Detalles de lugares y fotos. Los números de teléfono y la dirección URL del sitio web están disponibles en la API de búsqueda de Azure Maps.
- Direcciones URL de mapas
- Carreteras: los datos de límite de velocidad están disponibles a través de las API de ruta y geocodificación inversa en Azure Maps.
- Vista de calle estática

Azure Maps tiene varios servicios web REST extra que pueden ser de su interés:

- [Operaciones espaciales](https://docs.microsoft.com/rest/api/maps/spatial): descargue operaciones y cálculos espaciales complejos (como geovallas) en un servicio.
- [Tráfico](https://docs.microsoft.com/rest/api/maps/traffic): acceda a datos de incidencias y de flujo circulatorio en tiempo real.

## <a name="geocoding-addresses"></a>Geocodificación de direcciones

La geocodificación es el proceso por el cual una dirección (por ejemplo, "1 Microsoft Way, Redmond, WA") se convierte en una coordenada (como longitud: -122,1298 y latitud: 47,64005). Las coordenadas se suelen usar para centrar un mapa o colocar en él un marcador.

Azure Maps proporciona varios métodos de geocodificación de direcciones:

- [**Geocodificación de direcciones de forma libre**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress): especifique una única cadena de dirección (por ejemplo, "1 Microsoft Way, Redmond, WA") y procese la solicitud inmediatamente. Este es el método recomendable si hay que geocodificar direcciones individuales rápidamente.
- [**Geocodificación de direcciones estructurada**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressstructured): especifique las partes de una dirección (como el nombre de la calle, la ciudad, el país y el código postal) y procese la solicitud inmediatamente. Este es el método recomendable si hay que geocodificar direcciones individuales rápidamente y los datos ya se han analizado en partes individuales.
- [**Geocodificación de direcciones por lotes**](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatchpreview): cree una solicitud que contenga hasta 10 000 direcciones y procéselas durante un período de tiempo determinado. Todas las direcciones se geocodificarán en paralelo en el servidor y, cuando la operación se complete, puede descargar el conjunto de resultados completo. Este es el método recomendable para geocodificar grandes conjuntos de datos.
- [**Búsqueda aproximada**](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy): esta API combina la geocodificación de direcciones con la búsqueda de puntos de interés. La API toma una cadena de forma libre (que puede ser una dirección, un lugar, un punto de referencia, un punto de interés o una categoría de punto de interés) y procesa la solicitud inmediatamente. Se recomienda usar esta API en situaciones en las que los usuarios pueden buscar direcciones o puntos de interés desde el mismo cuadro de texto.
- [**Búsqueda parcial por lotes**](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview): cree una solicitud que contenga hasta 10 000 direcciones, lugares, puntos de referencia o puntos de interés y procéselos durante un período de tiempo determinado. Todos los datos se procesarán en paralelo en el servidor y, cuando la operación se complete, puede descargar el conjunto de resultados completo.

En la siguiente tabla se establecen referencias cruzadas entre los parámetros de API de Google Maps y los parámetros de API similares correspondientes en Azure Maps.

| Parámetro de API de Google Maps | Parámetro de API de Azure Maps similar  |
|---------------------------|--------------------------------------|
| `address`                   | `query`                            |
| `bounds`                    | `topLeft` y `btmRight`           |
| `components`                | `streetNumber`<br/>`streetName`<br/>`crossStreet`<br/>`postalCode`<br/>`municipality` (ciudad/municipio)<br/>`municipalitySubdivision` (barrio, ciudades más grandes o más pequeñas)<br/>`countrySubdivision` (estado o provincia)<br/>`countrySecondarySubdivision` (condado)<br/>`countryTertiarySubdivision` (distrito)<br/>`countryCode` (código de país de dos letras) |
| `key`                       | `subscription-key` (vea también la documentación de [autenticación con Azure Maps](azure-maps-authentication.md)) |
| `language`                  | `language` (vea la documentación de [compatibilidad de idiomas](supported-languages.md))  |
| `region`                    | `countrySet`                       |

[Aquí](how-to-search-for-address.md) se documenta un ejemplo sobre cómo usar el servicio de búsqueda. Asegúrese de revisar la documentación de [procedimientos recomendados de búsqueda](how-to-use-best-practices-for-search.md).

> [!TIP]
> Las API de geocodificación de direcciones de forma libre y de búsqueda aproximada se pueden usar en el modo Autocompletar si se agrega `&amp;typeahead=true` a la dirección URL de la solicitud. Esto indicará al servidor que el texto de entrada probablemente sea parcial y cambiará al modo predictivo.

## <a name="reverse-geocode-a-coordinate"></a>Geocodificación inversa de una coordenada

La geocodificación inversa es el proceso por el cual unas coordenadas geográficas (como, por ejemplo, una longitud de -122,1298 y una latitud de 47,64005) se convierten en la dirección aproximada correspondiente (como "1 Microsoft Way, Redmond, WA").

Azure Maps proporciona diversos métodos de geocodificación inversa:

- [**Geocodificador inverso de direcciones**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse): especifique una única coordenada geográfica para obtener la dirección aproximada correspondiente y procese la solicitud inmediatamente.
- [**Geocodificador inverso de cruces de direcciones**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet): especifique una única coordenada geográfica para obtener información sobre un cruce de direcciones (por ejemplo, "Primera" con "Central") y procese la solicitud inmediatamente.
- [**Geocodificador inverso de direcciones por lotes**](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatchpreview): cree una solicitud que contenga hasta 10 000 coordenadas y procéselas durante un período de tiempo determinado. Todos los datos se procesarán en paralelo en el servidor y, cuando la operación se complete, puede descargar el conjunto de resultados completo.

En la siguiente tabla se establecen referencias cruzadas entre los parámetros de API de Google Maps y los parámetros de API similares correspondientes en Azure Maps.

| Parámetro de API de Google Maps   | Parámetro de API de Azure Maps similar   |
|-----------------------------|---------------------------------------|
| `key`                       | `subscription-key` (vea también la documentación de [autenticación con Azure Maps](azure-maps-authentication.md)) |
| `language`                  | `language` (vea la documentación de [compatibilidad de idiomas](supported-languages.md))  |
| `latlng`                    | `query`  |
| `location_type`             | *N/D*     |
| `result_type`               | `entityType`    |

Asegúrese de revisar la documentación de [procedimientos recomendados de búsqueda](how-to-use-best-practices-for-search.md).

La API de geocodificación inversa de Azure Maps tiene algunas características extra que no están disponibles en Google Maps y que pueden ser útiles de cara a la integración al migrar la aplicación:

- Obtención de datos de límite de velocidad
- Obtención de información de uso de carreteras: comarcales, arterias, accesos limitados, rampas, etc.
- Lado de la calle en el que está la coordenada

## <a name="search-for-points-of-interest"></a>Búsqueda de puntos de interés

Para realizar búsquedas en los datos de punto de interés de Google Maps se usa la API de búsqueda de lugares. Esta API ofrece tres formas diferentes de buscar puntos de interés:

- **Buscar lugares a partir de texto:** se busca un punto de interés en función de su nombre, dirección o número de teléfono.
- **Búsqueda de elementos cercanos:** se buscan los puntos de interés que están a una determinada distancia de una ubicación.
- **Búsqueda de texto:** se buscan lugares mediante un texto de forma libre que incluye información del punto de interés y la ubicación (por ejemplo, "pizza en Nueva York" o "restaurantes cerca de la calle Central").

Azure Maps proporciona varias API de búsqueda de puntos de interés:

- [**Búsqueda de puntos de interés**](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi): se buscan puntos de interés por su nombre (por ejemplo, "Starbucks").
- [**Búsqueda de categorías de puntos de interés**](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory): se buscan puntos de interés por su categoría (por ejemplo, "restaurante").
- [**Búsqueda de elementos cercanos**](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby): se buscan los puntos de interés que están a una determinada distancia de una ubicación.
- [**Búsqueda aproximada**](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy): esta API combina la geocodificación de direcciones con la búsqueda de puntos de interés. La API toma una cadena de forma libre (que puede ser una dirección, un lugar, un punto de referencia, un punto de interés o una categoría de punto de interés) y procesa la solicitud inmediatamente. Se recomienda usar esta API en situaciones en las que los usuarios pueden buscar direcciones o puntos de interés desde el mismo cuadro de texto.
- [**Búsqueda en una geometría**](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry): se buscan los puntos de interés que se encuentran dentro de una geometría específica (polígono).
- [**Búsqueda a lo largo de una ruta**](https://docs.microsoft.com/rest/api/maps/search/postsearchalongroute): se buscan los puntos de interés que hay a lo largo del trayecto de una ruta específica.
- [**Búsqueda parcial por lotes**](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview): cree una solicitud que contenga hasta 10 000 direcciones, lugares, puntos de referencia o puntos de interés y procéselos durante un período de tiempo determinado. Todos los datos se procesarán en paralelo en el servidor y, cuando la operación se complete, puede descargar el conjunto de resultados completo.

Actualmente, Azure Maps no tiene ninguna API similar a la API de búsqueda de texto de Google Maps.

> [!TIP]
> Las API de búsqueda aproximada, de búsqueda de puntos de interés y de búsqueda de categorías de puntos de interés se pueden usar en el modo Autocompletar si se agrega `&amp;typeahead=true` a la dirección URL de la solicitud. Esto indicará al servidor que el texto de entrada probablemente sea parcial y cambiará al modo predictivo.

Asegúrese de revisar la documentación de [procedimientos recomendados de búsqueda](how-to-use-best-practices-for-search.md).

### <a name="find-place-from-text"></a>Búsqueda de lugares a partir de texto

Para hallar puntos de interés por su nombre o dirección, se pueden usar las API de Azure Maps de [búsqueda de puntos de interés](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) y de [búsqueda aproximada](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

En la siguiente tabla se establecen referencias cruzadas entre los parámetros de API de Google Maps y los parámetros de API similares correspondientes en Azure Maps.

| Parámetro de API de Google Maps | Parámetro de API de Azure Maps similar |
|---------------------------|-------------------------------------|
| `fields`                  | *N/D*                               |
| `input`                   | `query`                             |
| `inputtype`               | *N/D*                               |
| `key`                     | `subscription-key` (vea también la documentación de [autenticación con Azure Maps](azure-maps-authentication.md)) |
| `language`                | `language` (vea la documentación de [compatibilidad de idiomas](supported-languages.md))  |
| `locationbias`            | `lat`, `lon` y `radius`<br/>`topLeft` y `btmRight`<br/>`countrySet`  |

### <a name="nearby-search"></a>Búsqueda de elementos cercanos

Los puntos de interés cercanos se pueden recuperar en Azure Maps mediante la API de [búsqueda de elementos cercanos](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby).

En la siguiente tabla se establecen referencias cruzadas entre los parámetros de API de Google Maps y los parámetros de API similares correspondientes en Azure Maps.

| Parámetro de API de Google Maps | Parámetro de API de Azure Maps similar  |
|---------------------------|--------------------------------------|
| `key`                       | `subscription-key` (vea también la documentación de [autenticación con Azure Maps](azure-maps-authentication.md)) |
| `keyword`                   | `categorySet` y `brandSet`        |
| `language`                  | `language` (vea la documentación de [compatibilidad de idiomas](supported-languages.md))  |
| `location`                  | `lat` y `lon`                     |
| `maxprice`                  | *N/D*                               |
| `minprice`                  | *N/D*                               |
| `name`                      | `categorySet` y `brandSet`        |
| `opennow`                   | *N/D*                               |
| `pagetoken`                 | `ofs` y `limit`                   |
| `radius`                    | `radius`                            |
| `rankby`                    | *N/D*                               |
| `type`                      | `categorySet –` Vea la documentación de [categorías de búsqueda admitidas](supported-search-categories.md).   |

## <a name="calculate-routes-and-directions"></a>Cálculo de rutas y direcciones

Azure Maps se puede usar para calcular rutas y direcciones. Azure Maps tiene muchas funcionalidades similares a las del servicio de enrutamiento de Google Maps, como las siguientes:

- Horas de salida y de llegada
- Rutas de tráfico predictivas y en tiempo real
- Distintos modos de desplazamiento (coche, peatón, bicicleta)

> [!NOTE]
> Azure Maps requiere que todos los puntos de referencia sean coordenadas y que las direcciones estén geocodificadas.

El servicio de enrutamiento de Azure Maps proporciona las siguientes API para calcular rutas:

- [**Calcular ruta**](https://docs.microsoft.com/rest/api/maps/route/getroutedirections): calcula una ruta y la solicitud se procesa inmediatamente. Esta API admite solicitudes tanto GET como POST. Se recomienda usar solicitudes POST cuando se especifique un gran número de puntos de referencia o al usar muchas opciones de ruta para asegurarse de que la solicitud de dirección URL no se eternice y cause problemas.
- [**Ruta por lotes**](https://docs.microsoft.com/rest/api/maps/route/postroutedirectionsbatchpreview): cree una solicitud que contenga hasta 1000 solicitudes de ruta y procéselas durante un período de tiempo determinado. Todos los datos se procesarán en paralelo en el servidor y, cuando la operación se complete, puede descargar el conjunto de resultados completo.
- [**Servicios de movilidad**](https://docs.microsoft.com/rest/api/maps/mobility): calcula rutas y direcciones según el transporte público.

En la siguiente tabla se establecen referencias cruzadas entre los parámetros de API de Google Maps y los parámetros de API similares correspondientes en Azure Maps.

| Parámetro de API de Google Maps    | Parámetro de API de Azure Maps similar  |
|------------------------------|--------------------------------------|
| `alternatives`                 | `maxAlternatives`                  |
| `arrival_time`                | `arriveAt`                          |
| `avoid`                        | `avoid`                            |
| `departure_time`              | `departAt`                          |
| `destination`                  | `query` (se coordina con el formato `"lat0,lon0:lat1,lon1…."`)  |
| `key`                          | `subscription-key` (vea también la documentación de [autenticación con Azure Maps](azure-maps-authentication.md)) |
| `language`                     | `language` (vea la documentación de [compatibilidad de idiomas](supported-languages.md))   |
| `mode`                         | `travelMode`                       |
| `optimize`                     | `computeBestOrder`                 |
| `origin`                       | `query`                            |
| `region`                       | *No procede* (se trata de una característica relacionada con la geocodificación. Use el parámetro *countrySet* al usar la API de geocodificación de Azure Maps)  |
| `traffic_model`               | *No procede* (solo puede especificar si los datos de tráfico deben usarse con el parámetro *traffic*) |
| `transit_mode`                | Vea la documentación de los [servicios de movilidad](https://docs.microsoft.com/rest/api/maps/mobility). |
| `transit_routing_preference` | Vea la documentación de los [servicios de movilidad](https://docs.microsoft.com/rest/api/maps/mobility). |
| `units`                        | *No procede* (Azure Maps usa exclusivamente el sistema métrico)  |
| `waypoints`                    | `query`                            |

> [!TIP]
> La API de ruta de Azure Maps solo devuelve de forma predeterminada un resumen (distancia y lapsos de tiempo) y las coordenadas del trayecto de ruta. Use el parámetro `instructionsType` para obtener instrucciones paso a paso. El parámetro `routeRepresentation` se puede usar para descartar el resumen y el trayecto de ruta.

La API de enrutamiento de Azure Maps tiene numerosas características extra que no están disponibles en Google Maps y que pueden ser útiles de cara a la integración al migrar la aplicación:

- Compatibilidad con el tipo de ruta: la más corta, la más rápida y la de mayor ahorro de combustible.
- Compatibilidad con más modos de desplazamiento: autobús, motocicleta, taxi, camión y furgoneta.
- Compatibilidad con 150 puntos de referencia.
- Cálculo de varias duraciones de recorrido en una misma solicitud; tráfico histórico, tráfico en directo, sin tráfico.
- Posibilidad de descartar otros tipos de carreteras: transporte compartido, carreteras sin asfaltar, caminos ya usados.
- Especificación de las áreas personalizadas que deben evitarse.
- Límite de elevación al que se puede ascender en la ruta.
- Enrutamiento basado en la especificación del motor. Cálculo de rutas para vehículos eléctricos o de combustión en función de la carga o el combustible que queda y de las especificaciones del motor.
- Compatibilidad con parámetros de ruta de vehículo comercial; dimensiones del vehículo, peso, número de ejes y tipo de mercancía.
- Especificación de la velocidad máxima del vehículo.

Aparte de esto, el servicio de enrutamiento de Azure Maps permite también [calcular intervalos de ruta posible](https://docs.microsoft.com/rest/api/maps/route/getrouterange) (también conocidos como isocronas) que trazan un polígono que cubre un área que se puede recorrer hacia cualquier dirección desde un punto de origen dentro de un período de tiempo especificado o una cantidad de combustible/carga determinada.

## <a name="retrieve-a-map-image"></a>Recuperación de una imagen de mapa

Azure Maps proporciona una API para representar imágenes de mapa estático con datos superpuestos. La API de [representación de imágenes de mapa](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile) de Azure Maps es comparable a la API de mapa estático de Google Maps.

> [!NOTE]
> Azure Maps requiere que el punto central y todas las ubicaciones de marcadores y rutas sean coordenadas con el formato "longitud latitud" (mientras que Google Maps usa el formato "latitud,longitud") y que las direcciones estén geocodificadas.

En la siguiente tabla se establecen referencias cruzadas entre los parámetros de API de Google Maps y los parámetros de API similares correspondientes en Azure Maps.

| Parámetro de API de Google Maps | Parámetro de API de Azure Maps similar  |
|---------------------------|--------------------------------------|
| `center`                    | `center`                           |
| `format`                    | `format` (se especifica como parte de la ruta de dirección URL). Actualmente, solo se admite el formato PNG. |
| `key`                       | `subscription-key` (vea también la documentación de [autenticación con Azure Maps](azure-maps-authentication.md)) |
| `language`                  | `language` (vea la documentación de [compatibilidad de idiomas](supported-languages.md))  |
| `maptype`                   | `layer` y `style` (vea la documentación de [estilos de mapa compatibles](supported-map-styles.md)) |
| `markers`                   | `pins`                             |
| `path`                      | `path`                             |
| `region`                    | *No procede* (se trata de una característica relacionada con la geocodificación. Use el parámetro `countrySet` al usar la API de geocodificación de Azure Maps)  |
| `scale`                     | *N/D*                              |
| `size`                      | `width` y `height` (puede tener un tamaño máximo de 8192 x 8192) |
| `style`                     | *N/D*                              |
| `visible`                   | *N/D*                              |
| `zoom`                      | `zoom`                             |

> [!NOTE]
> Azure Maps usa un sistema de mosaicos que tienen el doble del tamaño de los mosaicos de mapa usados en Google Maps, con lo cual el valor de nivel de zoom en Azure Maps será un nivel de zoom más cercano en Azure Maps que en Google Maps. Para compensarlo, reduzca a uno menos el nivel de zoom en las solicitudes que vaya a migrar.

Para más información, vea la [Guía de procedimientos de la API de representación de imágenes de mapa](how-to-render-custom-data.md).

Además de poder generar una imagen de mapa estático, el servicio de representación de Azure Maps también ofrece la posibilidad de acceder directamente a los mosaicos de mapa en formato de trama (PNG) y vectorial:

- [**Mosaico de mapa**](https://docs.microsoft.com/rest/api/maps/render/getmaptile): obtenga mosaicos de trama (PNG) y vectoriales de los mapas base (carreteras, fronteras, entorno).
- [**Mosaico de imagen de mapa**](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile): se obtienen mosaicos de imágenes aéreas y por satélite.

> [!TIP]
> Hace unos años, muchas aplicaciones de Google Maps pasaron de experiencias de mapa interactivo a imágenes de mapa estático como método de ahorro económico. En Azure Maps, con frecuencia resulta mucho más rentable usar el control de mapa interactivo del SDK web, ya que los costos se derivan de las cargas de mosaicos de mapa. Los mosaicos de mapa en Azure Maps son grandes y a menudo se tarda muy poco en volver a crear la misma vista de mapa que un mapa estático, ya que el explorador almacena los mosaicos de mapa automáticamente en la memoria caché. En consecuencia, el control de mapa interactivo suele generar solo una fracción de una transacción al reproducir una vista de mapa estática. Las operaciones de panorámica y zoom cargarán más mosaicos; sin embargo, existen opciones en el control de mapa para deshabilitar este comportamiento si así se prefiere. El control de mapa interactivo también proporciona muchas más opciones de visualización que los servicios de mapa estático.

### <a name="marker-url-parameter-format-comparison"></a>Comparación de formatos de parámetro de dirección URL de marcador

**Antes: Google Maps**

En Google Maps se pueden agregar marcadores a una imagen de mapa estático usando el parámetro `markers` en la dirección URL. El parámetro `markers` toma un estilo y una lista de ubicaciones que se van a representar en el mapa con ese estilo, como se indica aquí:

```
&markers=markerStyles|markerLocation1|markerLocation2|...
```

Para usar más estilos, hay que agregar más parámetros `markers` a la dirección URL con un estilo y un conjunto de ubicaciones diferentes.

Las ubicaciones de marcador se especifican con el formato "latitud,longitud".

Los estilos de marcador en Google Maps se agregan con el formato `optionName:value`, con cada estilo separado por un carácter de barra vertical (\|), así: "optionName1:value1\|optionName2:value2". Fíjese en que los nombres de opción y los valores se separan con dos puntos (:). Se pueden usar los siguientes nombres de opción de estilo para aplicar estilo a los marcadores de Google Maps:

- `color`: color del icono de marcador predeterminado. Puede ser un color hexadecimal de 24 bits (`0xrrggbb`) o uno de los valores siguientes: `black`, `brown`, `green`, `purple`, `yellow`, `blue`, `gray`, `orange`, `red``white`.
- `label`: un solo carácter alfanumérico en mayúsculas que se va a mostrar en la parte superior del icono.
- `size`: tamaño del marcador. Puede ser `tiny`, `mid` o `small`.

En Google Maps también se pueden usar iconos personalizados con los siguientes nombres de opción de estilo:

- `anchor`: especifica la alineación de la imagen de icono con la coordenada. Puede ser un valor de píxel (x, y) o uno de los siguientes valores: `top`, `bottom`, `left`, `right`, `center`, `topleft`, `topright`, `bottomleft`, `bottomright`.
- `icon`: dirección URL que apunta a la imagen de icono.

Por ejemplo, en Google Maps se puede agregar un marcador rojo de tamaño medio al mapa en las coordenadas (longitud: -110, latitud: 45) con el siguiente parámetro de dirección URL:

```
&markers=color:red|size:mid|45,-110
```

<center>

![Marcador de Google Maps](media/migrate-google-maps-web-services/google-maps-marker.png)</center>

**Después: Azure Maps**

En Azure Maps también se pueden agregar marcadores a una imagen de mapa estático especificando el parámetro `pins` en la dirección URL. Al igual que Google Maps, se puede especificar un estilo y una lista de ubicaciones en este parámetro, y el parámetro `pins` se puede especificar varias veces para admitir marcadores con distintos estilos.

```
&pins=iconType|pinStyles||pinLocation1|pinLocation2|...
```

Para usar más estilos, hay que agregar más parámetros `pins` a la dirección URL con un estilo y un conjunto de ubicaciones diferentes.

Cuando se trata de anclar ubicaciones, Azure Maps requiere que las coordenadas tengan el formato "longitud latitud", mientras que Google Maps usa el formato "latitud,longitud". Fíjese en que hay un espacio (y no una coma) entre la longitud y la latitud en Azure Maps.

El valor `iconType` especifica el tipo de anclaje que se va a crear y puede tener los valores siguientes:

- `default`: icono de anclaje predeterminado.
- `none`: no se muestra ningún icono, solo se representarán etiquetas.
- `custom`: especifica que se va a usar un icono personalizado. Se puede agregar una dirección URL que apunte a la imagen del icono al final del parámetro `pins`, después de la información de ubicación del anclaje.
- `{udid}`: identificador de datos único (UDID) de un icono almacenado en la plataforma de almacenamiento de datos de Azure Maps.

Los estilos de anclaje en Azure Maps se agregan con el formato `optionNameValue`, con cada estilo separado por un carácter de barra vertical (\|), así: `iconType|optionName1Value1|optionName2Value2`. Fíjese en que los nombres de opción y los valores no se separan. Se pueden usar los siguientes nombres de opción de estilo para aplicar estilo a los marcadores de Azure Maps:

- `al`: especifica la opacidad (valor alfanumérico) del marcador. Puede ser un número entre 0 y 1.
- `an`: especifica el delimitador del anclaje. Los valores de píxel x e y se especifican con el formato "x y".
- `co`: color del anclaje. Debe ser un color hexadecimal de 24 bits: de `000000` a `FFFFFF`.
- `la`: especifica el delimitador de la etiqueta. Los valores de píxel x e y se especifican con el formato "x y".
- `lc`: color de la etiqueta. Debe ser un color hexadecimal de 24 bits: de `000000` a `FFFFFF`.
- `ls`: tamaño de la etiqueta en píxeles. Puede ser un número mayor que 0.
- `ro`: valor en grados para girar el icono. Puede ser un número entre -360 y 360.
- `sc`: valor de escala del icono de anclaje. Puede ser un número mayor que 0.

Los valores de etiqueta se especifican para cada ubicación de anclaje, en lugar de tener un valor de etiqueta único que se aplica a todos los marcadores de la lista de ubicaciones. El valor de etiqueta puede ser una cadena de varios caracteres y especificarse entre comillas simples para no confundirlo con un valor de estilo o de ubicación.

Por ejemplo, en Azure Maps, se puede agregar un icono predeterminado rojo (`FF0000`) con la etiqueta "Space Needle" situada debajo (15 50) del icono en las coordenadas (longitud: -122,349300, latitud: 47,620180) con el siguiente parámetro de dirección URL:

```
&pins=default|coFF0000|la15 50||'Space Needle' -122.349300 47.620180
```

<center>

![Marcador de Azure Maps](media/migrate-google-maps-web-services/azure-maps-marker.png)</center>

En el siguiente ejemplo se agregan tres anclajes con los valores de etiqueta "1", "2" y "3":

```
&pins=default||'1'-122 45|'2'-119.5 43.2|'3'-121.67 47.12
```

<center>

![Varios marcadores en Azure Maps](media/migrate-google-maps-web-services/azure-maps-multiple-markers.png)</center>

### <a name="path-url-parameter-format-comparison"></a>Comparación de formatos de parámetro de dirección URL de ruta

**Antes: Google Maps**

En Google Maps se pueden agregar líneas y polígonos a una imagen de mapa estático usando el parámetro `path` en la dirección URL. El parámetro `path` toma un estilo y una lista de ubicaciones que se van a representar en el mapa con ese estilo, como se indica aquí:

```
&path=pathStyles|pathLocation1|pathLocation2|...
```

Para usar más estilos, hay que agregar más parámetros `path` a la dirección URL con un estilo y un conjunto de ubicaciones diferentes.

Las ubicaciones de las rutas en Google Maps se especifican con el formato `latitude1,longitude1|latitude2,longitude2|…`. Las rutas se pueden codificar o contener direcciones de puntos.

Los estilos de ruta en Google Maps se agregan con el formato `optionName:value`, con cada estilo separado por un carácter de barra vertical (\|), así: `optionName1:value1|optionName2:value2`. Fíjese en que los nombres de opción y los valores se separan con dos puntos (:). Se pueden usar los siguientes nombres de opción de estilo para aplicar estilo a las rutas de Google Maps:

- `color`: color de la ruta o del contorno del polígono. Puede ser un color hexadecimal de 24 bits (`0xrrggbb`), un color hexadecimal de 32 bits (`0xrrggbbbaa`) o uno de los valores siguientes: black, brown, green, purple, yellow, blue, gray, orange, red, white.
- `fillColor`: color con el que se va a rellenar el área de la ruta (polígono). Puede ser un color hexadecimal de 24 bits (`0xrrggbb`), un color hexadecimal de 32 bits (`0xrrggbbbaa`) o uno de los valores siguientes: black, brown, green, purple, yellow, blue, gray, orange, red, white.
- `geodesic`: indica si la ruta debe ser una línea que siga la curvatura de la tierra.
- `weight`: grosor de la línea de ruta en píxeles.

Por ejemplo, en Google Maps se puede agregar una línea roja con una opacidad del 50 % y un grosor de 4 píxeles al mapa entre las coordenadas (longitud: -110, latitud: 45 y longitud: -100, latitud: 50) con el siguiente parámetro de dirección URL:

```
&path=color:0xFF000088|weight:4|45,-110|50,-100
```

<center>

![Polilínea de Google Maps](media/migrate-google-maps-web-services/google-maps-polyline.png)</center>

**Después: Azure Maps**

En Azure Maps también se pueden agregar líneas y polígonos a una imagen de mapa estático especificando el parámetro `path` en la dirección URL. Al igual que Google Maps, se puede especificar un estilo y una lista de ubicaciones en este parámetro, y el parámetro `path` se puede especificar varias veces para representar varios círculos, líneas y polígonos con distintos estilos.

```
&path=pathStyles||pathLocation1|pathLocation2|...
```

Cuando se trata de enrutar ubicaciones, Azure Maps requiere que las coordenadas tengan el formato "longitud latitud", mientras que Google Maps usa el formato "latitud,longitud". Fíjese en que hay un espacio (y no una coma) entre la longitud y la latitud en Azure Maps. Azure Maps no admite rutas codificadas ni direcciones de puntos. Se pueden cargar conjuntos de datos más grandes como un objeto GeoJSON en la API de almacenamiento de datos de Azure Maps, como se documenta [aquí](how-to-render-custom-data.md#get-data-from-azure-maps-data-storage).

Los estilos de ruta en Azure Maps se agregan con el formato `optionNameValue`, con cada estilo separado por un carácter de barra vertical (\|), así: `optionName1Value1|optionName2Value2`. Fíjese en que los nombres de opción y los valores no se separan. Se pueden usar los siguientes nombres de opción de estilo para aplicar estilo a las rutas de Azure Maps:

- `fa`: opacidad del color de relleno (alfanumérico) usada al representar polígonos. Puede ser un número entre 0 y 1.
- `fc`: color de relleno usado para representar el área de un polígono.
- `la`: opacidad del color de línea (alfanumérico) usada al representar líneas y el contorno de polígonos. Puede ser un número entre 0 y 1.
- `lc`: color de línea usado para representar líneas y el contorno de polígonos.
- `lw`: ancho de la línea en píxeles.
- `ra`: especifica el radio de un círculo en metros.

Por ejemplo, en Azure Maps se puede agregar una línea roja con una opacidad del 50 % y un grosor de 4 píxeles al mapa entre las coordenadas (longitud: -110, latitud: 45 y longitud: -100, latitud: 50) con el siguiente parámetro de dirección URL:

```
&path=lcFF0000|la.5|lw4||-110 45|-100 50
```

<center>

![Polilínea de Azure Maps](media/migrate-google-maps-web-services/azure-maps-polyline.png)</center>

## <a name="calculate-a-distance-matrix"></a>Cálculo de una matriz de distancia

Azure Maps proporciona una API para calcular los tiempos y las distancias de desplazamiento entre un conjunto de ubicaciones como una matriz de distancia. La API de matriz de distancia de Azure Maps es similar a la API de matriz de distancia de Google Maps:

- [**Matriz de ruta**](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview): calcula asincrónicamente los tiempos y las distancias de desplazamiento de un conjunto de orígenes y destinos. Se admite un máximo de 700 celdas por solicitud (número de orígenes multiplicado por el número de destinos). Teniendo en cuenta esa restricción, estos son algunos ejemplos de posibles dimensiones de la matriz: 700 x 1, 50 x 10, 10 x 10, 28 x 25, 10 x 70.

> [!NOTE]
> Las solicitudes a la API de matriz de distancia solo se pueden realizar mediante una solicitud POST, con la información de origen y de destino en el cuerpo de la solicitud. Además, Azure Maps requiere que todos los orígenes y destinos sean coordenadas y que las direcciones estén geocodificadas.

En la siguiente tabla se establecen referencias cruzadas entre los parámetros de API de Google Maps y los parámetros de API similares correspondientes en Azure Maps.

| Parámetro de API de Google Maps      | Parámetro de API de Azure Maps similar  |
|--------------------------------|--------------------------------------|
| `arrivial_time`                | `arriveAt`                           |
| `avoid`                        | `avoid`                              |
| `depature_time`                | `departAt`                           |
| `destinations`                 | `destination`: especifíquelo como GeoJSON en el cuerpo de la solicitud POST. |
| `key`                          | `subscription-key` (vea también la documentación de [autenticación con Azure Maps](azure-maps-authentication.md)) |
| `language`                     | `language` (vea la documentación de [compatibilidad de idiomas](supported-languages.md))  |
| `mode`                         | `travelMode`                         |
| `origins`                      | `origins`: especifíquelo como GeoJSON en el cuerpo de la solicitud POST.  |
| `region`                       | *No procede* (se trata de una característica relacionada con la geocodificación. Use el parámetro `countrySet` al usar la API de geocodificación de Azure Maps) |
| `traffic_model`                | *No procede* (solo puede especificar si los datos de tráfico deben usarse con el parámetro `traffic`) |
| `transit_mode`                 | *No procede*: actualmente no se admiten matrices de distancia basadas en el tránsito.  |
| `transit_routing_preference`   | *No procede*: actualmente no se admiten matrices de distancia basadas en el tránsito.  |
| `units`                        | *No procede* (Azure Maps usa exclusivamente el sistema métrico) |

> [!TIP]
> Todas las opciones de enrutamiento avanzadas disponibles en la API de enrutamiento de Azure Maps (enrutamiento de camiones, especificaciones del motor, elementos que pueden evitarse...) son compatibles con la API de matriz de distancia de Azure Maps.

## <a name="get-a-time-zone"></a>Obtención de una zona horaria

Azure Maps proporciona una API para obtener la zona horaria en la que una coordenada se encuentra. La API de zona horaria de Azure Maps es similar a la API de zona horaria de Google Maps:

- [**Zona horaria por coordenada**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebycoordinates): especifique una coordenada y obtenga los detalles de la zona horaria en la que se encuentra.

En la siguiente tabla se establecen referencias cruzadas entre los parámetros de API de Google Maps y los parámetros de API similares correspondientes en Azure Maps.

| Parámetro de API de Google Maps | Parámetro de API de Azure Maps similar   |
|---------------------------|---------------------------------------|
| `key`                       | `subscription-key` (vea también la documentación de [autenticación con Azure Maps](azure-maps-authentication.md))       |
| `language`                  | `language` (vea la documentación de [compatibilidad de idiomas](supported-languages.md))    |
| `location`                  | `query`             |
| `timestamp`                 | `timeStamp`         |

Además de esto, la plataforma de Azure Maps también proporciona otras API de zona horaria que ayudan en las conversiones de identificadores y nombres de zona horaria:

- [**Zona horaria por identificador**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebyid): devuelve la información de zona horaria actual, histórica y futura relativa al identificador de zona horaria de IANA especificado.
- [**Enumeración de zonas horarias, IANA**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumiana): devuelve una lista completa de los identificadores de zona horaria de IANA. Las actualizaciones del servicio IANA se reflejan en el sistema en el plazo de un día.
- [**Enumeración de zonas horarias, Windows**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumwindows): devuelve una lista completa de los identificadores de zona horaria de Windows.
- [**Versión de IANA de zona horaria**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneianaversion): devuelve el número de versión de IANA actual utilizado por Azure Maps.
- [**Zona horaria de Windows a IANA**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonewindowstoiana): devuelve el identificador de IANA correspondiente a un identificador de zona horaria de Windows válido especificado. Se pueden obtener varios identificadores de IANA de un mismo identificador de Windows.

## <a name="client-libraries"></a>Bibliotecas de clientes

Azure Maps proporciona bibliotecas de cliente para los siguientes lenguajes de programación:

- JavaScript, TypeScript, Node.js ([documentación](how-to-use-services-module.md) \| [paquete de NPM](https://www.npmjs.com/package/azure-maps-rest))

Bibliotecas de cliente de código abierto para otros lenguajes de programación:

- .NET Standard 2.0 ([proyecto de GitHub](https://github.com/perfahlen/AzureMapsRestServices) \| [paquete NuGet](https://www.nuget.org/packages/AzureMapsRestToolkit/)

## <a name="additional-resources"></a>Recursos adicionales

Estos son algunos recursos y documentación adicionales sobre los servicios REST de Azure Maps.

- [Procedimientos recomendados de búsqueda](how-to-use-best-practices-for-search.md)
- [Búsqueda de una dirección](how-to-search-for-address.md)
- [Documentación de referencia de la API del servicio REST de Azure Maps](https://docs.microsoft.com/rest/api/maps/)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los servicios REST de Azure Maps.

> [!div class="nextstepaction"]
> [Procedimientos recomendados de uso del servicio de búsqueda](how-to-use-best-practices-for-search.md)

> [!div class="nextstepaction"]
> [Uso del módulo de servicios (SDK web)](how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Ejemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-maps)