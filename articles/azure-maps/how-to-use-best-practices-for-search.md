---
title: Procedimiento para buscar de forma eficaz con el servicio de búsqueda de Azure Maps | Microsoft Docs
description: Aprenda a usar los procedimientos recomendados para hacer búsquedas con el servicio de búsqueda de Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: de9e484e43c87375c2fdf9b34dd2efce3bb8aa8c
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72429170"
---
# <a name="best-practices-to-use-azure-maps-search-service"></a>Procedimientos recomendados para utilizar el servicio de búsqueda de Azure Maps

El [servicio de búsqueda](https://docs.microsoft.com/rest/api/maps/search) de Azure Maps incluye API con distintas capacidades, desde la búsqueda de direcciones hasta la búsqueda de datos de puntos de interés (POI) cerca de una ubicación concreta. En este artículo, compartiremos los procedimientos recomendados para llamar los datos a través de los servicios de búsqueda de Azure Maps. Aprenderá a:

* Crear consultas que devuelvan coincidencias relevantes
* Limitar los resultados de la búsqueda
* Conocer la diferencia entre diferentes tipos de resultados
* Leer la estructura de respuesta de la búsqueda de direcciones


## <a name="prerequisites"></a>Requisitos previos

Para realizar cualquier llamada a las API del servicio Azure Maps, necesita una cuenta de Maps y una clave. Para obtener información sobre cómo crear una cuenta, siga las instrucciones de [Administrar cuentas](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account) y siga los pasos de [Obtención de la clave principal](./tutorial-search-location.md#getkey) para recuperar una clave de suscripción principal para su cuenta.

> [!Tip]
> Para hacer una consulta al servicio de búsqueda, puede usar la [aplicación Postman](https://www.getpostman.com/apps) para crear llamadas de REST o el entorno de desarrollo de API que prefiera.


## <a name="best-practices-for-geocoding"></a>Procedimientos recomendados para la geocodificación

Al buscar una dirección completa o parcial mediante el servicio de búsqueda de Azure Maps, se toma el término de búsqueda y se devuelven las coordenadas de longitud y latitud de la dirección. Este proceso se denomina “geocodificación”. La capacidad de obtener las coordenadas geográficas en un país depende de la cobertura de los datos de carretera y la precisión de la codificación geográfica del servicio de geocodificación.

Consulte la [cobertura de geocodificación](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage) para obtener más información sobre las capacidades de geocodificación de Azure Maps por país o región.

### <a name="limit-search-results"></a>Limitar los resultados de la búsqueda

   En esta sección, obtendrá información sobre cómo usar las API de búsqueda de Azure Maps para limitar los resultados de búsqueda. 

   > [!Note]
   > No todas las API de búsqueda son totalmente compatibles con los siguientes parámetros

   **Resultados de búsqueda de sesgos geográficos**

   Con el fin de aplicar un sesgo geográfico a los resultados del área relevante para el usuario, debe agregar siempre la ubicación con el máximo detalle posible. Para restringir los resultados de búsqueda, puede agregar los siguientes tipos de entrada:

   1. Use el parámetro `countrySet`, por ejemplo, “US, FR”. De forma predeterminada, se hacen búsquedas en todo el mundo, lo cual puede devolver resultados innecesarios. Si la consulta no incluye el parámetro `countrySet`, la búsqueda puede devolver resultados inexactos. Por ejemplo, al buscar una ciudad denominada **Bellevue**, se devolverán resultados de Estados Unidos y Francia, puesto que en ambos países hay una ciudad que se denomina **Bellevue**.

   2. Puede usar los parámetros `btmRight` y `topleft` para establecer el rectángulo de selección que restringirá la búsqueda a una zona específica del mapa.

   3. Para influir en el área de relevancia de los resultados, puede definir los parámetros coordinados `lat` y `lon`, y establecer el radio de la zona de búsqueda con el parámetro `radius`.


   **Parámetros de búsqueda aproximada**

   1. Los parámetros `minFuzzyLevel` y `maxFuzzyLevel` contribuyen a devolver coincidencias relevantes, incluso cuando los parámetros de consulta no se corresponden exactamente con la información deseada. La mayoría de las consultas de búsqueda tienen como valores predeterminados `minFuzzyLevel=1` y `maxFuzzyLevel=2` para aumentar el rendimiento y reducir los resultados poco comunes. Pongamos como ejemplo el término de búsqueda “restrant”, que coincide con “restaurante” cuando el valor `maxFuzzyLevel` se establece en 2. Los niveles de aproximación predeterminados se pueden invalidar según las necesidades de la solicitud. 

   2. También puede especificar el conjunto exacto de tipos de resultados que se van a devolver usando el parámetro `idxSet`. Con este fin, puede enviar una lista de índices separada por comas, sin tener en cuenta el orden de los elementos. Estos son los índices admitidos:

       * `Addr` - **Intervalos de direcciones**: en algunas calles, hay puntos de la dirección que se interpolan desde el principio hasta el final de la calle. Estos puntos se representan como intervalos de direcciones.
       * `Geo` - **Geografías**: áreas en un mapa que representan una división administrativa de un terreno, es decir, país, estado, ciudad.
       * `PAD` - **Dirección del punto**:  puntos de un mapa cuya dirección específica, con el nombre y el número de la calle, se puede encontrar en un índice, por ejemplo, Soquel Dr 2501. Es el nivel con más precisión que está disponible para las direcciones.  
       * `POI` - **Puntos de interés**: puntos de un mapa que merecen atención y pueden ser de interés.  [Get Search Address](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) no devuelve ningún punto de interés.  
       * `Str` - **Calles**: representación de calles en el mapa.
       * `XStr` - **Cruce de calles o intersecciones**:  representación de cruces, lugares en los que dos calles forman una intersección.


       **Ejemplos de uso**:

       * idxSet=POI (solo búsqueda de puntos de interés) 

       * idxSet=PAD,Addr (solo direcciones de búsqueda, PAD= dirección del punto, Addr= intervalo de direcciones)

### <a name="reverse-geocode-and-geography-entity-type-filter"></a>Invertir el filtro de tipo de entidad de código geográfico y geografía

Al realizar una búsqueda de código geográfico inversa con la [API de búsqueda inversa de la dirección](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), el servicio tiene la capacidad de devolver los polígonos de las áreas administrativas. Al especificar el parámetro `entityType` en la solicitud, puede restringir la búsqueda de los tipos de entidad geográfica especificados. La respuesta resultante contendrá el identificador geográfico, así como el tipo de entidad coincidente. Si proporciona más de una entidad, el punto de conexión devolverá la **entidad más pequeña que esté disponible**. El identificador de la geometría devuelto puede utilizarse para obtener la geometría de esa ubicación geográfica a través del [servicio para obtener el polígono](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon).

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

### <a name="search-results-language"></a>Idioma de los resultados de la búsqueda

El parámetro `language` le permite establecer en qué idioma se van a devolver los resultados de la búsqueda. Si no se establece el idioma en la solicitud, de forma predeterminada el servicio de búsqueda usará el idioma más común en el país o región. Además, cuando los datos no están disponibles en el idioma especificado, se usa el idioma predeterminado. En [idiomas admitidos](https://docs.microsoft.com/azure/azure-maps/supported-languages) verá una lista de los idiomas admitidos con relación a los servicios de Azure Maps por país o región.


### <a name="predictive-mode-auto-suggest"></a>Modo predictivo (sugerencias automáticas)

Para encontrar más coincidencias para las consultas parciales, el parámetro `typeahead` debe establecerse en “true”. La consulta se interpretará como una entrada parcial y la búsqueda pasará al modo predictivo. En caso contrario, el servicio supondrá que se ha pasado toda la información pertinente.

En la consulta de ejemplo siguiente puede ver que se hace la consulta “Microso” al servicio de la dirección de búsqueda con el parámetro `typeahead` establecido en **true**. Si observa la respuesta, puede ver que el servicio de búsqueda interpreta la consulta como una consulta parcial y la respuesta contiene los resultados de la consulta sugerida automáticamente.

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


### <a name="uri-encoding-to-handle-special-characters"></a>Codificación de URI para controlar los caracteres especiales 

Para buscar direcciones de cruces de calles, es decir, “Paseo de Gracia & Aragón, Barcelona”, hay que codificar el carácter especial “&” antes de enviar la solicitud. Se recomienda codificar los datos de caracteres en un URI, donde todos los caracteres se codifican mediante un carácter “%” y un valor hexadecimal de dos caracteres que se corresponde a su carácter UTF-8.

**Ejemplos de uso**:

Obtener la dirección de búsqueda:

```
query=1st Avenue & E 111th St, New York
```

 debe codificarse como:

```
query=1st%20Avenue%20%26%20E%20111th%20St%2C%20New%20York
```


Estos son los distintos métodos que hay que usar para los distintos lenguajes: 

JavaScript/TypeScript:
```Javascript
encodeURIComponent(query)
```

C#/VB:
```csharp
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

Ruby:
```Ruby
CGI::escape(query) 
```

Swift:
```Swift
query.stringByAddingPercentEncodingWithAllowedCharacters(.URLHostAllowedCharacterSet()) 
```

Go:
```Go
import ("net/url") 
url.QueryEscape(query)
```


## <a name="best-practices-for-poi-search"></a>Procedimientos recomendados para la búsqueda de puntos de interés

La búsqueda de puntos de interés (POI) le permite pedir resultados de POI por nombre, por ejemplo, buscar un negocio por su nombre. Es muy recomendable que use el parámetro `countrySet` para especificar los países en los que su aplicación necesita cobertura, ya que, de forma predeterminada, se busca en todo el mundo, lo que probablemente devuelve resultados innecesarios o comporta largos tiempos de búsqueda.

### <a name="brand-search"></a>Búsqueda de marcas

Para mejorar la relevancia de los resultados y la información de la respuesta, la respuesta de la búsqueda de puntos de interés (POI) incluye la información de marca que se puede usar para analizar la respuesta.

Haremos una solicitud de [búsqueda de categorías de POI](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory) para gasolineras cerca del campus de Microsoft (Redmond, WA). Si observa la respuesta, verá información de la marca para cada punto de interés devuelto.

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


### <a name="airport-search"></a>Búsqueda de aeropuertos

La búsqueda de POI admite la búsqueda de aeropuertos mediante el uso de los códigos de aeropuerto oficiales. Por ejemplo, **SEA** (aeropuerto internacional de Seattle Tacoma). 

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=SEA 
```

### <a name="nearby-search"></a>Búsqueda de elementos cercanos

Para recuperar solo resultados de POI cercanos a una ubicación específica, la [API de búsquedas cercanas](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) debe contener la elección adecuada. Este punto de conexión solo devolverá resultados de POI y no tendrá en cuenta ningún parámetro de consulta de búsqueda. Para limitar los resultados, se recomienda establecer el radio.

## <a name="understanding-the-responses"></a>Entender las respuestas

Vamos a hacer una solicitud de búsqueda de una dirección de Seattle al [servicio de búsqueda](https://docs.microsoft.com/rest/api/maps/search) de Azure Maps. Si observa detenidamente la URL solicitada a continuación, hemos establecido el parámetro `countrySet` en **US** para buscar la dirección en los Estados Unidos de América.

**Consulta de ejemplo:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1&query=400%20Broad%20Street%2C%20Seattle%2C%20WA&countrySet=US
```

Echemos un vistazo a la siguiente estructura de respuesta. Los tipos de resultado de los objetos de resultado de la respuesta son diferentes. Si observa detenidamente, puede ver que tenemos tres tipos diferentes de objetos de resultado: “Dirección del punto”, “Calle” y “Cruce”. Tenga en cuenta que la búsqueda de direcciones no devuelve puntos de interés. El parámetro `Score` de cada objeto de respuesta indica la puntuación de coincidencia relativa a las puntuaciones de otros objetos en la misma respuesta. Vea [Obtener la dirección de búsqueda](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) para obtener más información sobre los parámetros del objeto de respuesta.

**Tipos de resultado admitidos:**

* **Dirección del punto:** puntos en un mapa con una dirección específica, que incluye el nombre y número de calle. Es el máximo nivel de precisión que está disponible para las direcciones. 

* **Intervalo de direcciones:**  en algunas calles, hay puntos de la dirección que se interpolan desde el principio hasta el final de la calle. Estos puntos se representan como intervalos de direcciones. 

* **Geografía:** áreas en un mapa que representan una división administrativa de un terreno, es decir, país, estado, ciudad. 

* **POI (puntos de interés):** puntos de un mapa que merecen atención y pueden ser de interés.

* **Calle:** representación de calles en el mapa. Las direcciones se resuelven en la coordenada de latitud y longitud de la calle que contiene la dirección. No se puede procesar el número de casa. 

* **Intersección:** intersecciones. Representaciones de cruces, lugares en los que dos calles forman una intersección.

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

Cuando el tipo de respuesta es **Geometría**, puede incluir el identificador de geometría que se devuelve en el objeto **dataSources** bajo “geometría” e “id”. Por ejemplo, el [servicio para obtener el polígono](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) permite solicitar los datos de geometría en formato GeoJSON, por ejemplo, el contorno de ciudad o aeropuerto de un conjunto de entidades. Puede usar estos datos de límites para [geovallas](https://docs.microsoft.com/azure/azure-maps/tutorial-geofence) o [puntos de interés de búsqueda dentro de la geometría](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry).


La API para [buscar direcciones](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) o [buscar coincidencias](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) puede incluir el **identificador de geometría** que se devuelve en el objeto dataSources bajo “geometría” e “id”.


```JSON 
"dataSources": { 
        "geometry": { 
            "id": "00005557-4100-3c00-0000-000059690938" // The geometry ID is returned in the dataSources object under "geometry" and "id".
        }
} 
```

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [cómo crear solicitudes de servicio de búsqueda de Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address).
* Explore la [documentación de la API del servicio de búsqueda](https://docs.microsoft.com/rest/api/maps/search) de Azure Maps. 
