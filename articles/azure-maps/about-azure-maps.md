---
title: Información general de Azure Maps | Microsoft Docs
description: Introducción a Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 07/12/2018
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 93fe8dc3f8ff991cd6c48923d9e2073e4e93f1ad
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39040854"
---
# <a name="what-is-azure-maps"></a>¿Qué es Azure Maps?
Azure Maps es una colección de servicios geoespaciales respaldado por los nuevos datos de asignación para que pueda proporcionar contexto geográfico preciso a las aplicaciones web y móviles. Contiene API REST para representar mapas, buscar puntos de interés, rutas a los puntos de interés, condiciones del tráfico, zonas horarias e IP de servicios de ubicación. Puede usar estas API con herramientas conocidas para desarrollar y escalar rápidamente las soluciones de Azure con información de ubicación integrada. Junto con las API REST, ofrece control de JavaScript basado en web que hace que el desarrollo sea sencillo, flexible y portátil en varios medios. 

En el siguiente vídeo se explica Azure Maps en profundidad:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="services-in-azure-maps"></a>Servicios de Azure Maps

Azure Maps consta de los siguientes seis servicios que pueden proporcionar contexto geográfico a las aplicaciones de Azure. 

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


### <a name="time-zone-service"></a>Time Zone Service

Time Zone Service permite consultar la información de zona horaria actual, histórica y futura mediante pares de latitud-longitud o un [identificador de IANA](http://www.iana.org/). Time Zone Service también permite convertir los identificadores de zona horaria de Microsoft Windows en zonas horarias de IANA, de forma que se captura una compensación de zona horaria a UTC y se obtiene la hora actual en una zona horaria respectiva. Una respuesta JSON típica de una consulta a Time Zone Service se parece al ejemplo siguiente:

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
    * Traffic Incidents: una vista exacta sobre los atascos y las incidencias de tráfico de toda la red de carreteras.

![Tráfico de Azure Maps](media/about-azure-maps/Introduction_Traffic.png)

Visite la página de las [Traffic API de Azure Maps](https://docs.microsoft.com/rest/api/maps/traffic) para más detalles.

### <a name="ip-to-location"></a>IP to Location

IP to Location es un servicio en versión preliminar que permite recuperar el código de país de dos letras de una dirección IP determinada. Este servicio ayuda a adaptar la aplicación a límites geopolíticos especiales, así como a mejorar la experiencia del usuario al cambiar el contenido de la aplicación en función de la ubicación geográfica. 


## <a name="programming-model"></a>Modelo de programación

Azure Maps se ha diseñado pensando en la movilidad y potencia las aplicaciones multiplataforma. Usa un modelo de programación independiente del idioma y admite salidas JSON mediante [API REST](https://docs.microsoft.com/rest/api/maps/). 

Además, Azure Maps ofrece un práctico [control de mapas de JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-javascript/?view=azure-iot-typescript-latest) con un modelo sencillo de programación para el desarrollo fácil y rápido de aplicaciones web y móviles. 


## <a name="usage"></a>Uso

A los servicios de Maps se accede simplemente con ir a [Azure Portal](http://portal.azure.com) y crear una cuenta de Azure Maps. 

Azure Maps usa un esquema de autenticación basado en claves. Su cuenta incluye dos claves que se generan previamente de manera automática. Comience a integrar estas funcionalidades de ubicación directamente en sus aplicaciones mediante el uso de una de sus claves en las solicitudes al servicio Azure Maps.

## <a name="supported-regions"></a>Regiones admitidas
La API de Azure Maps está actualmente disponible en todos los países, excepto en: 

* Argentina
* China
* India
* Marruecos
* Pakistán
* Corea del Sur

Compruebe la dirección IP actual y verifique que la ubicación de la dirección IP no esté en uno de los países no admitidos enumerados anteriormente.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las nuevas características de Azure Maps: 
    - [Enrutamiento de matrices, isocronas, búsqueda de IP, etc.](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/) 
- Pruebe una aplicación de ejemplo que demuestra el servicio para continuar
    - [Iniciar un mapa de búsqueda interactiva de demostración](quick-demo-map-app.md)
