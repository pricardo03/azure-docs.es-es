---
title: Información general de Azure Maps | Microsoft Docs
description: Introducción a Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 02/04/2019
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 8092cd169f93a6815e52517d805941ac7ddcbbc0
ms.sourcegitcommit: f9448a4d87226362a02b14d88290ad6b1aea9d82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66807503"
---
# <a name="what-is-azure-maps"></a>¿Qué es Azure Maps?

Azure Maps es una colección de servicios geoespaciales desanclada por los datos de asignación más recientes disponibles que proporciona contexto geográfico preciso tanto a las aplicaciones web como a las aplicaciones para dispositivos móviles. Azure Maps consta de varias API REST para representar **mapas** con varios estilos e imágenes satelitales, **buscar** direcciones, lugares y puntos de interés en todo el mundo; realizar un **enrutamiento** punto a punto, multipunto, optimización multipunto, isocrono, vehículo comercial, con capa de tráfico y enrutamiento de matrices; ver el mejor flujo e incidentes del tráfico del sector; servicios de **movilidad** que solicitan información de transporte público, uso compartido de bicicletas, uso compartido de ciclomotores y uso compartido de coches para planear rutas haciendo uso de medios de transporte alternativos y datos en tiempo real; establecer la ubicación del usuario mediante la **geolocalización** y convertir la ubicación en **zonas horarias**, así como capturar la hora en una ubicación. Además, Azure Maps ofrece servicios de **geovalla**, asignación de almacenamiento de **datos** (hospedaje de información de ubicación en Azure) y **Spatial Operations** que proporciona inteligencia de ubicación a través del análisis geoespacial. Los servicios de Azure Maps están disponibles directamente como API REST o mediante nuestras sólidas **Web SDK** o **Android SDK**. Estas herramientas permiten a los desarrolladores desarrollar y escalar rápidamente soluciones que integran información acerca de la ubicación en las soluciones de Azure desde la nube de Azure. Regístrese hoy mismo para obtener su [cuenta de Azure Maps](https://azure.microsoft.com/services/azure-maps/) y empezar a realizar desarrollos.

En el siguiente vídeo se explica Azure Maps en profundidad:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Controles de mapa

### <a name="web-sdk"></a>SDK web

El SDK web para Azure Maps permite personalizar mapas interactivos con contenido propio e imágenes para su presentación en las aplicaciones web o móviles. Este control usa WebGL, lo que permite representar grandes conjuntos de datos con alto rendimiento. Desarrolle con el SDK mediante JavaScript o TypeScript.

![SDK web de Azure Maps](media/about-azure-maps/Introduction_WebMapControl.png)

### <a name="android-sdk"></a>SDK de Android

Android SDK para Azure Maps permite crear eficaces aplicaciones de elaboración de mapas para móviles. 

![Android SDK para Azure Maps](media/about-azure-maps/AndroidSDK.png)

## <a name="services-in-azure-maps"></a>Servicios de Azure Maps

Azure Maps consta de los nueve servicios siguientes que pueden proporcionar contexto geográfico a las aplicaciones de Azure.

### <a name="data-service"></a>Data Service

Los datos son imprescindibles para los mapas. Por eso, acercar los datos de los clientes al servicio Azure Maps reducirá la latencia, aumentará la productividad y creará escenarios nuevos con un gran potencial para ponerlos en marcha en sus aplicaciones. Data Service permite cargar y almacenar datos geoespaciales para usarlos con operaciones espaciales o para la composición de imágenes con el fin de reducir la latencia, aumentar la productividad y habilitar nuevos escenarios en sus aplicaciones. Para más información acerca de este servicio, visite la página de las [API de Data Service](https://docs.microsoft.com/rest/api/maps/data).

### <a name="mobility-service"></a>Mobility Service

Los servicios de Azure Maps Mobility Services proporcionan la inteligencia de la ubicación en tiempo real de los servicios de transporte público cercanos, lo que incluye las paradas, la información de las rutas y cálculos del tiempo de los trayectos. El servicio permite buscar tipos de objeto específicos como paradas de transporte público, bicicletas/ciclomotores/coches compartidos en torno a una ubicación determinada que devuelve un conjunto de objetos de tránsito con detalles de los objetos. El servicio permite también a los desarrolladores solicitar datos de una línea de transporte que cubran la información básica y detalles adicionales, como la geometría de la línea, la lista de paradas, las llegadas del transporte programadas y en tiempo real y las alertas del servicio. Los usuarios también pueden solicitar cuántas bicicletas compartidas disponibles se dejan en la parada más cercana mediante la solicitud de información de las paradas. Mobility Service también tiene la capacidad de buscar los vehículos compartidos disponibles y devuelve detalles como la disponibilidad en el futuro y el nivel de combustible actual.
El servicio Azure Maps Mobility Services permite planear viajes en tiempo real, ya que devuelve las mejores opciones de ruta posibles y proporciona varias formas de viajar, entre las que se incluye a pie, en bicicleta y el transporte público disponible en el área metropolitana (ciudad). Además, los desarrolladores pueden solicitar detalles de los itinerarios con información adicional, como la geometría de la ruta y las programaciones de itinerarios detalladas.

Para más información acerca del servicio y de las diversas características, consulte nuestra [documentación de la API](https://docs.microsoft.com/rest/api/maps/mobility)

### <a name="render-service"></a>Render Service

Diseñado para que los desarrolladores creen aplicaciones web y móviles basadas en mapas. El servicio emplea imágenes de gráficos de trama de alta calidad, disponibles en 19 niveles de zoom, o imágenes de mapa en formato vectorial totalmente personalizables.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

Render Service ahora ofrece varias API en versión preliminar que permiten a los desarrolladores trabajar con imágenes de satélite. Para más información, lea el artículos sobre las [Render API de Azure Maps](https://docs.microsoft.com/rest/api/maps/render).

### <a name="route-service"></a>Route Service

Contiene sólidos cálculos de geometría de infraestructuras del mundo real y direcciones para varios modos de transporte. El servicio permite a los desarrolladores calcular direcciones entre distintos modos de viaje, como automóvil, camión, bicicleta o a pie. El servicio también puede tener en cuenta entradas como las condiciones del tráfico, las restricciones de peso o el transporte materiales peligrosos.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

Route Service ahora ofrece una versión preliminar de las características avanzadas, como el procesamiento por lotes de varias solicitudes de ruta, matrices de tiempo de desplazamiento y distancia entre un conjunto de orígenes y destinos, y búsqueda de rutas o distancias posibles de desplazamiento en función del tiempo o el combustible. Para más información sobre las funcionalidades de las rutas, lea el artículo sobre las [Route API de Azure Maps](https://docs.microsoft.com/rest/api/maps/route).

### <a name="search-service"></a>Servicio de búsqueda

Diseñado para que los desarrolladores puedan buscar direcciones, lugares, listados de empresas por nombre o categoría y otra información geográfica. Search Service puede asimismo realizar la [codificación inversa](https://en.wikipedia.org/wiki/Reverse_geocoding) de direcciones y cruces de calles en función de la longitud y la latitud.

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

Search Service también proporciona características avanzadas como la búsqueda en una ruta, la búsqueda en una zona más amplia, agrupación de solicitudes de búsqueda, y búsqueda de una zona mayor en lugar de una ubicación concreta. Las API de búsqueda en grupo y por zona son actualmente versiones preliminares. Para más detalles sobre las funcionalidades de búsqueda, lea la página sobre las [Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search).

### <a name="spatial-operations"></a>Spatial Operations

Spatial Operations de Azure Maps toma información de la ubicación y la analiza sobre la marcha para poder informar a nuestros clientes de los eventos en curso que están teniendo lugar en un tiempo y un espacio, lo que permite el análisis casi en tiempo real y el modelado predictivo de eventos. El servicio permite a los clientes de Azure Maps mejorar su inteligencia de ubicación con una biblioteca de cálculos matemáticos geoespaciales comunes, entre los que se incluyen servicios como el punto más cercano, la distancia ortodrómica y los límites de la ruta. Para más información acerca del servicio y de sus distintas características, consulte nuestra [documentación de la API](https://docs.microsoft.com/rest/api/maps/spatial).

### <a name="time-zone-service"></a>Time Zone Service

Time Zone Service permite consultar la información de zona horaria actual, histórica y futura mediante pares de latitud-longitud o un [identificador de IANA](https://www.iana.org/). Time Zone Service también permite convertir los identificadores de zona horaria de Microsoft Windows en zonas horarias de IANA, de forma que se captura una compensación de zona horaria a UTC y se obtiene la hora actual en una zona horaria respectiva. Una respuesta JSON típica de una consulta a Time Zone Service se parece al ejemplo siguiente:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Para más información sobre este servicio, visite la página de las [Timezone API de Azure Maps](https://docs.microsoft.com/rest/api/maps/timezone).

### <a name="traffic-service"></a>Traffic Service

Traffic Service es un conjunto de servicios web diseñados para los desarrolladores, para la creación de aplicaciones web y móviles que requieren tráfico. El servicio proporciona dos tipos de datos:

* Traffic Flow: velocidades observadas en tiempo real y tiempos de desplazamiento de todas las principales carreteras de la red.
* Traffic Incidents: una vista actualizada sobre los atascos y las incidencias de tráfico de toda la red de carreteras.

![Tráfico de Azure Maps](media/about-azure-maps/Introduction_Traffic.png)

Visite la página de las [Traffic API de Azure Maps](https://docs.microsoft.com/rest/api/maps/traffic) para más detalles.

### <a name="ip-to-location"></a>IP to Location

El servicio IP to Location permite obtener una vista previa del código de país de dos letras recuperado de una dirección IP determinada. Este servicio puede ayudarle a personalizar y mejorar la experiencia del usuario al potenciar el contenido de la aplicación personalizado según la ubicación geográfica.

Para más información sobre las API REST para el servicio IP to Location, visite la página sobre las [API de geolocalización de Azure Maps](https://docs.microsoft.com/rest/api/maps/geolocation).

## <a name="programming-model"></a>Modelo de programación

Azure Maps se ha diseñado pensando en la movilidad y potencia las aplicaciones multiplataforma. Usa un modelo de programación independiente del idioma y admite salidas JSON mediante [API REST](https://docs.microsoft.com/rest/api/maps/).

Además, Azure Maps ofrece un práctico [control de mapas de JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-control) con un modelo sencillo de programación para el desarrollo fácil y rápido de aplicaciones web y móviles.

## <a name="usage"></a>Uso

A los servicios de Maps se accede simplemente con ir a [Azure Portal](https://portal.azure.com) y crear una cuenta de Azure Maps.

Azure Maps usa un esquema de autenticación basado en claves. Su cuenta incluye dos claves que se generan previamente de manera automática. Comience a integrar estas funcionalidades de ubicación en su aplicación mediante el uso de la clave y la realización de una solicitud al servicio Azure Maps.

## <a name="supported-regions"></a>Regiones admitidas

La API de Azure Maps está actualmente disponible en todos los países y regiones, excepto en las siguientes:

* Argentina
* China
* India
* Marruecos
* Pakistán
* Corea del Sur

Verifique que la ubicación de la dirección IP actual no esté en uno de los países o las regiones no admitidos enumerados anteriormente.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las nuevas características de Azure Maps:

> [!div class="nextstepaction"]
> [Enrutamiento de matrices, isocronas, búsqueda de IP, etc.](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/)

Pruebe una aplicación de ejemplo que muestre el uso de Azure Maps:

> [!div class="nextstepaction"]
> [Inicio rápido: Creación de una aplicación web](quick-demo-map-app.md)
