---
title: Cómo buscar de forma eficaz con el servicio de búsqueda de Azure Maps | Microsoft Docs
description: Aprenda a usar los procedimientos recomendados para la búsqueda mediante el servicio de búsqueda de Azure Maps
ms.author: v-musehg
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: f7a14e975a5ca3aee5588f55f43b28081c100074
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59358147"
---
# <a name="best-practices-to-use-azure-maps-search-service"></a>Procedimientos recomendados para utilizar el servicio de búsqueda de Azure Maps

Azure Maps [servicio Search](https://docs.microsoft.com/rest/api/maps/search) incluye las API con distintas capacidades, por ejemplo, desde la dirección de búsqueda para buscar datos de punto de interés (POI) en torno a una ubicación específica. En este artículo, se compartirán las prácticas recomendadas para llamar a los datos a través de servicios de búsqueda de Azure Maps. Aprenderá a:

* Crear consultas para devolver a coincidencias relevantes
* Limitar los resultados de búsqueda
* Obtenga información sobre la diferencia entre varios tipos de resultados
* Leer la estructura de respuesta de búsqueda de direcciones


## <a name="prerequisites"></a>Requisitos previos

Para realizar cualquier llamada a las API del servicio Azure Maps, necesita una cuenta de Maps y una clave. Para más información sobre cómo crear una cuenta y recuperar una clave, consulte [Administración de la cuenta y las claves de Azure Maps](how-to-manage-account-keys.md).

> [!Tip]
> Para consultar el servicio de búsqueda, puede usar el [aplicación Postman](https://www.getpostman.com/apps) para compilar REST llamadas o bien puede usar cualquier entorno de desarrollo de API que prefiera.


## <a name="best-practices-for-geocoding"></a>Procedimientos recomendados para la codificación geográfica

Al buscar una dirección completa o parcial mediante el servicio de búsqueda de Azure Maps, toma el término de búsqueda y devuelve las coordenadas de longitud y latitud de la dirección. Este proceso se denomina codificación geográfica. La capacidad de obtener las coordenadas geográficas en un país depende de la cobertura de los datos de carretera y la precisión de la codificación geográfica del servicio de geocodificación.

Consulte [cobertura de geocodificación](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage) para obtener más información sobre las capacidades de codificación geográfica de Azure Maps por país o región.

### <a name="limit-search-results"></a>Limitar los resultados de búsqueda

   En esta sección, obtendrá información sobre cómo usar la API de búsqueda de Azure Maps para limitar los resultados de búsqueda. 

   > [!Note]
   > Búsqueda no todas las API son totalmente compatibles con parámetros que aparecen a continuación

   **Resultados de búsqueda geográfica sesgo**

   Con el fin de geo-sesgo los resultados para el área relevante para el usuario, siempre debe agregar el máximo posible detallado entrada de la ubicación. Para restringir los resultados de búsqueda, considere la posibilidad de agregar los siguientes tipos de entrada:

   1. Establecer el `countrySet` parámetro, por ejemplo "US, FR". El comportamiento de búsqueda predeterminado es buscar en todo el mundo, devolución de resultados innecesarios. Si la consulta no incluye `countrySet` parámetro, la búsqueda puede devolver resultados inexactos. Por ejemplo, busque una ciudad denominada **Bellevue** se devolverán resultados de Estados Unidos y Francia, puesto que no hay ciudades denominadas **Bellevue** en Francia y en los Estados Unidos.

   2. Puede usar el `btmRight` y `topleft` cuadro parámetros para establecer el rectángulo de selección para restringir la búsqueda a un área específica en el mapa.

   3. Para influir en el área de relevancia para los resultados, puede definir la `lat`y `lon` coordinar los parámetros y establece el radio de la zona de búsqueda con el `radius` parámetro.


   **Parámetros de búsqueda aproximada**

   1. El `minFuzzyLevel` y `maxFuzzyLevel`, ayudar a devolver coincidencias relevantes, incluso cuando los parámetros de consulta no corresponden exactamente a la información deseada. De forma predeterminada la mayoría de las consultas de búsqueda `minFuzzyLevel=1` y `maxFuzzyLevel=2` para obtener un rendimiento y reducir los resultados inusuales. Poner un ejemplo de un término de búsqueda "restrant", coincide con "restaurante" cuando el `maxFuzzyLevel` está establecido en 2. Los niveles aproximada predeterminado pueden invalidarse según las necesidades de la solicitud. 

   2. También puede especificar el conjunto exacto de tipos de resultados para devolver mediante el `idxSet` parámetro. Para este propósito puede enviar la lista separada por comas de los índices, no importa el orden del elemento. Estos son los índices compatibles:

       * `Addr` - **Intervalos de direcciones**: Para algunas de las calles, hay puntos de dirección que se interpolan desde el principio y al final de la calle; estos puntos se representan como intervalos de direcciones.
       * `Geo` - **Ubicaciones geográficas**: Áreas en un mapa que representan una división administrativa de un terreno, es decir, país, estado, ciudad.
       * `PAD` - **Dirección del punto de**:  Puntos en un mapa donde dirección específica con un nombre de la calle y el número puede encontrarse en un índice, por ejemplo, Soquel 2501 de recuperación ante desastres. Es el máximo nivel de precisión para las direcciones disponible.  
       * `POI` - **Puntos de interés**: Puntos en un mapa que merecen atención y pueden ser de interés.  [Obtener la dirección de búsqueda](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) no se devolverá de interés.  
       * `Str` - **Calles**: Representación de calles en el mapa.
       * `XStr` - **Entre las calles/intersecciones**:  Representación de las uniones; lugares donde dos de las calles forman una intersección.


       **Ejemplos de uso**:

       * idxSet = POI (búsqueda de puntos de interés sólo) 

       * idxSet = PAD, Addr (búsqueda únicamente direcciones PAD = dirección de punto, Addr = intervalo de direcciones)

### <a name="reverse-geocode-and-geography-entity-type-filter"></a>Invertir el filtro de tipo de entidad de código geográfico y geography

Al realizar una búsqueda de código geográfico inversa con [API de búsqueda inversa de dirección](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), el servicio tiene la capacidad de devolver los polígonos de las áreas administrativas. Especificando el parámetro `entityType` en la solicitud, puede restringir la búsqueda de tipos de entidad de geografía especificada. La respuesta resultante contendrá el identificador de geografía, así como coincide con el tipo de entidad. Si proporciona más de una entidad, punto de conexión devolverá el **entidad más pequeña disponible**. Devuelve el identificador de geometría puede utilizarse para obtener la geometría de esa ubicación geográfica a través de [servicio obtener polígono](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon).

**Solicitud de ejemplo:**

```HTTP
https://atlas.microsoft.com/search/address/reverse/json?api-version=1.0&subscription-key={subscription-key}&query=47.6394532,-122.1304551&language=en-US&entityType=Municipality
```

**Respuesta:**

```JSON
{
    "summary": {
        "queryTime": 8,
        "numResults": 1
    },
    "addresses": [
        {
            "address": {
                "routeNumbers": [],
                "countryCode": "US",
                "countrySubdivision": "WA",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "municipality": "Redmond",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Redmond, WA",
                "boundingBox": {
                    "northEast": "47.717105,-122.034537",
                    "southWest": "47.627016,-122.164998",
                    "entity": "position"
                },
                "countrySubdivisionName": "Washington"
            },
            "position": "47.639454,-122.130455",
            "dataSources": {
                "geometry": {
                    "id": "00005557-4100-3c00-0000-0000596ae571"
                }
            },
            "entityType": "Municipality"
        }
    ]
}
```

### <a name="search-results-language"></a>Idioma de los resultados de búsqueda

El `language` parámetro le permite establecer en la búsqueda en qué lenguaje se deben devolver los resultados. Si no se establece el idioma en la solicitud, el servicio de búsqueda predeterminado automáticamente es el lenguaje más común en el país o región. Además, cuando los datos en el idioma especificado no están disponibles, se usa el idioma predeterminado. Consulte [idiomas admitidos](https://docs.microsoft.com/azure/azure-maps/supported-languages) para obtener una lista de idiomas compatibles con respecto a los servicios de Azure Maps por país o región.


### <a name="predictive-mode-auto-suggest"></a>Modo predictivo (sugerencias automáticas)

Para encontrar más coincidencias parciales de las consultas, `typeHead` parámetro debe establecerse en ' true '. La consulta se interpretará como una entrada parcial y la búsqueda pasará al modo de predicción. En caso contrario, el servicio supondrá que se ha pasado toda la información pertinente.

En el ejemplo de consulta, a continuación, puede ver que se consulta el servicio de la dirección de búsqueda "Microsoft" con el `typehead` parámetro establecido en **true**. Si observa que la respuesta, puede ver que el servicio de búsqueda interpreta la consulta como consulta parcial y respuesta contiene los resultados de consulta sugeridos automáticamente.

**Consulta de ejemplo:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1.0&typeahead=true&countrySet=US&lat=47.6370891183&lon=-122.123736172&query=Microsoft
```

**Respuesta:**

```JSON
{
    "summary": {
        "query": "microsoft",
        "queryType": "NON_NEAR",
        "queryTime": 25,
        "numResults": 6,
        "offset": 0,
        "totalResults": 6,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6370891183,
            "lon": -122.123736172
        }
    },
    "results": [
        {
            "type": "Street",
            "id": "US/STR/p0/10294417",
            "score": 2.594,
            "dist": 327.546040632591,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98052",
                "extendedPostalCode": "980526399,980528300",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Redmond, WA 98052",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.63989,
                "lon": -122.12509
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63748,
                    "lon": -122.12309
                },
                "btmRightPoint": {
                    "lat": 47.64223,
                    "lon": -122.13061
                }
            }
        },
        ...,
        ...,
        ...,
        ...,
        {
            "type": "Street",
            "id": "US/STR/p0/9063400",
            "score": 2.075,
            "dist": 3655467.6406921702,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Yorkmount, Charlotte",
                "municipality": "Charlotte",
                "countrySecondarySubdivision": "Mecklenburg",
                "countryTertiarySubdivision": "Township 1 Charlotte",
                "countrySubdivision": "NC",
                "postalCode": "28217",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Charlotte, NC 28217",
                "countrySubdivisionName": "North Carolina"
            },
            "position": {
                "lat": 35.14279,
                "lon": -80.91814
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14267,
                    "lon": -80.91814
                },
                "btmRightPoint": {
                    "lat": 35.14279,
                    "lon": -80.91824
                }
            }
        }
    ]
}
```


### <a name="uri-encoding-to-handle-special-characters"></a>URI de codificación para controlar los caracteres especiales 

Para buscar entre direcciones postales, es decir, "1 º Avenue & unión Street, Seattle", caracteres especiales '&' debe codificarse antes de enviar la solicitud. Se recomienda codificar los datos de caracteres en un URI, donde todos los caracteres se codifican mediante un carácter "%" y un valor hexadecimal de dos caracteres correspondiente a su carácter UTF-8.

**Ejemplos de uso**:

Obtenga la dirección de búsqueda:

```
query=1st Avenue & E 111th St, New York
```

 deberá codificarse como:

```
query"=1st%20Avenue%20%26%20E%20111th%20St%2C%20New%20York
```


Estos son los distintos métodos que se usará para distintos idiomas: 

JavaScript/TypeScript:
```Javascript
encodeURIComponent(query)
```

C#/VB:
```C#
Uri.EscapeDataString(query)
```

Java:
```Java
URLEncoder.encode(query, "UTF-8") 
```

Python:
```Python
import urllib.parse 
urllib.parse.quote(query)
```

C++:
```C++
#include <curl/curl.h>
curl_easy_escape(query)
```

PHP:
```PHP
urlencode(query)
```

rubí:
```Ruby
CGI::escape(query) 
```

veloz:
```Swift
query.stringByAddingPercentEncodingWithAllowedCharacters(.URLHostAllowedCharacterSet()) 
```

Go:
```Go
import ("net/url") 
url.QueryEscape(query)
```


## <a name="best-practices-for-poi-search"></a>Procedimientos recomendados para la búsqueda de POI

Puntos de interés (POI) búsqueda permite solicitar POI resultados por nombre, por ejemplo, búsqueda empresarial por su nombre. Se recomienda encarecidamente no usar el `countrySet` parámetro para especificar los países donde la aplicación necesita cobertura, ya que será el comportamiento predeterminado buscar en todo el mundo, devolución de resultados innecesarios o dar lugar a tiempos de búsqueda.

### <a name="brand-search"></a>Búsqueda de marca

Para mejorar la relevancia de los resultados y la información de la respuesta, la respuesta de búsqueda del punto de interés (POI) incluye la información de marca que se puede usar aún más para analizar la respuesta.

Vamos a hacer un [búsqueda de la categoría de POI](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory) solicitud para gasolineras cerca de campus de Microsoft (Redmond, WA). Si observa que la respuesta, puede ver información de marca para cada punto de interés devuelto.

**Consulta de ejemplo:**

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=gas%20station&limit=3&lat=47.6413362&lon=-122.1327968
```

**Respuesta:**

```JSON
{
    "summary": {
        "query": "gas station",
        "queryType": "NON_NEAR",
        "queryTime": 206,
        "numResults": 3,
        "offset": 0,
        "totalResults": 742169,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6413362,
            "lon": -122.1327968
        }
    },
    "results": [
        {
            "type": "POI",
            "id": "US/POI/p0/245813",
            "score": 5.663,
            "dist": 1037.0280221303253,
            "info": "search:ta:840531000004190-US",
            "poi": {
                "name": "Chevron",
                "phone": "+(1)-(425)-6532200",
                "brands": [
                    {
                        "name": "Chevron"
                    }
                ],
                "url": "www.chevron.com",
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2444",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Northeast Bellevue, Bellevue",
                "municipality": "Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "2444 Bel Red Rd, Bellevue, WA 98007",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.63201,
                "lon": -122.13281
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63291,
                    "lon": -122.13414
                },
                "btmRightPoint": {
                    "lat": 47.63111,
                    "lon": -122.13148
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63223,
                        "lon": -122.13311
                    }
                }
            ]
        },
        ...,
        {
            "type": "POI",
            "id": "US/POI/p0/7727106",
            "score": 5.662,
            "dist": 1458.645407416307,
            "info": "search:ta:840539000488527-US",
            "poi": {
                "name": "BROWN BEAR CAR WASH",
                "phone": "+(1)-(425)-6442868",
                "brands": [
                    {
                        "name": "Texaco"
                    }
                ],
                "url": "www.texaco.com/",
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "15248",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98052",
                "extendedPostalCode": "980525511",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "15248 Bel Red Rd, Redmond, WA 98052",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62843,
                "lon": -122.13628
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62933,
                    "lon": -122.13761
                },
                "btmRightPoint": {
                    "lat": 47.62753,
                    "lon": -122.13495
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.62826,
                        "lon": -122.13626
                    }
                }
            ]
        }
    ]
}
```


### <a name="airport-search"></a>Búsqueda de aeropuerto

Búsqueda POI admite la búsqueda aeropuertos mediante el uso de los códigos de aeropuerto oficiales. Por ejemplo, **mar** (aeropuerto internacional de Seattle Tacoma). 

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=SEA 
```

### <a name="nearby-search"></a>Búsqueda cercano

Para recuperar solo los resultados POI en torno a una ubicación específica, la [cerca de la API de búsqueda](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) puede ser la elección adecuada. Este punto de conexión solo devolverá resultados POI y no toma un parámetro de consulta de búsqueda. Para limitar los resultados, se recomienda establecer el radio.

## <a name="understanding-the-responses"></a>Descripción de las respuestas

Vamos a hacer una solicitud de búsqueda de direcciones en el Azure Maps [servicio de búsqueda](https://docs.microsoft.com/rest/api/maps/search) una dirección en Seattle. Si observa detenidamente la dirección URL de solicitud siguiente, hemos configurado la `countrySet` parámetro **US** para buscar la dirección en los Estados Unidos de América.

**Consulta de ejemplo:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1&query=400%20Broad%20Street%2C%20Seattle%2C%20WA&countrySet=US
```

Aún más, echemos un vistazo a la siguiente estructura de respuesta. Los tipos de resultado de los objetos de resultado de la respuesta son diferentes. Si observa detenidamente que puede ver que tenemos tres tipos diferentes de objetos de resultado, que son "Entre Street", "Street" y "Dirección de punto". Tenga en cuenta que esa búsqueda de direcciones no devuelve de interés. El `Score` parámetro para cada objeto de respuesta indica la puntuación de coincidencia relativa a las puntuaciones de otros objetos en la misma respuesta. Consulte [obtener la dirección de búsqueda](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) para obtener más información acerca de los parámetros de objeto de respuesta.

**Tipos admitidos de resultado:**

* **Dirección de punto:** Puntos en un mapa con una dirección específica con un nombre de la calle y un número. El máximo nivel de precisión para las direcciones disponible. 

* **Intervalo de direcciones:**  Para algunas de las calles, hay puntos de dirección que se interpolan desde el principio y al final de la calle; estos puntos se representan como intervalos de direcciones. 

* **Geografía:** Áreas en un mapa que representan una división administrativa de un terreno, es decir, país, estado, ciudad. 

* **POI - (puntos de interés):** Puntos en un mapa que merecen atención y pueden ser de interés.

* **Calle:** Representación de calles en el mapa. Las direcciones se resuelven en la coordenada de latitud y longitud de la calle que contiene la dirección. No se puede procesar el número de casa. 

* **Cross Street:** Intersecciones. Representaciones de las uniones; lugares donde dos de las calles forman una intersección.

**Respuesta:**

```JSON
{
    "summary": {
        "query": "400 broad street seattle wa",
        "queryType": "NON_NEAR",
        "queryTime": 129,
        "numResults": 6,
        "offset": 0,
        "totalResults": 6,
        "fuzzyLevel": 1
    },
    "results": [
        {
            "type": "Point Address",
            "id": "US/PAD/p0/43076024",
            "score": 9.894,
            "address": {
                "streetNumber": "400",
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, South Lake Union, Lower Queen Anne",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "400 Broad Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62039,
                "lon": -122.34928
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62129,
                    "lon": -122.35061
                },
                "btmRightPoint": {
                    "lat": 47.61949,
                    "lon": -122.34795
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.61982,
                        "lon": -122.34886
                    }
                }
            ]
        },
        {
            "type": "Street",
            "id": "US/STR/p0/2440854",
            "score": 8.129,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Westlake, South Lake Union",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "extendedPostalCode": "981094347,981094700,981094701,981094702",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62553,
                "lon": -122.33936
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62545,
                    "lon": -122.33861
                },
                "btmRightPoint": {
                    "lat": 47.62574,
                    "lon": -122.33974
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/8450985",
            "score": 8.129,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Belltown",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109,98121",
                "extendedPostalCode": "981094991,981211117,981211237,981213206",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.61691,
                "lon": -122.35251
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61502,
                    "lon": -122.35041
                },
                "btmRightPoint": {
                    "lat": 47.61857,
                    "lon": -122.35484
                }
            }
        },
        ...,
        ...,
        {
            "type": "Cross Street",
            "id": "US/XSTR/p1/3816818",
            "score": 6.759,
            "address": {
                "streetName": "Broad Street & Valley Street",
                "municipalitySubdivision": "South Lake Union, Seattle",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street & Valley Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62574,
                "lon": -122.33861
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62664,
                    "lon": -122.33994
                },
                "btmRightPoint": {
                    "lat": 47.62484,
                    "lon": -122.33728
                }
            }
        }
    ]
}
```

### <a name="geometry"></a>Geometría

Cuando el tipo de respuesta es **geometría**, puede incluir el identificador de geometría que se devuelve en el **dataSources** objeto bajo "geometría" e "id". Por ejemplo, [servicio obtener polígono](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) permite solicitar los datos de geometría en formato GeoJSON, por ejemplo, una ciudad o un aeropuerto, el esquema para un conjunto de entidades. Puede usar estos datos de límites para [Geovallas](https://docs.microsoft.com/azure/azure-maps/tutorial-geofence) o [puntos de interés de búsqueda dentro de la geometría](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry).


[Buscar dirección](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) o [búsqueda aproximada](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) respuestas de API pueden incluir el **Id. de la geometría** que se devuelve en el objeto de orígenes de datos en "geometría" y "id".


```JSON 
"dataSources": { 
        "geometry": { 
            "id": "00005557-4100-3c00-0000-000059690938" // The geometry ID is returned in the dataSources object under "geometry" and "id".
        }
} 
```

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [cómo crear solicitudes de servicio de búsqueda de Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address).
* Explore la Azure Maps [documentación de API del servicio de búsqueda](https://docs.microsoft.com/rest/api/maps/search). 