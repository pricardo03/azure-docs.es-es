---
title: Búsqueda eficaz con el servicio de búsqueda de Azure Maps | Microsoft Azure Maps
description: Obtenga información sobre cómo aplicar las prácticas recomendadas para el servicio Search de Microsoft Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 01/23/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 68c7408f13027ded7beaabf46fb663217a90c52b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845760"
---
# <a name="best-practices-to-use-azure-maps-search-service"></a>Procedimientos recomendados para utilizar el servicio de búsqueda de Azure Maps

El [servicio Search](https://docs.microsoft.com/rest/api/maps/search) de Azure Maps incluye API con varias funcionalidades. Por ejemplo, la API de búsqueda de direcciones se usa para buscar puntos de interés o datos alrededor de una ubicación específica. En este artículo se muestran las prácticas recomendadas que se aplican al llamar a datos desde el servicio Search de Azure Maps. Aprenderá a:

* Crear consultas que devuelvan coincidencias relevantes
* Limitar los resultados de la búsqueda
* Conocer la diferencia entre diferentes tipos de resultados
* Leer la estructura de respuesta de la búsqueda de direcciones


## <a name="prerequisites"></a>Prerequisites

Para llamar a las API del servicio Maps, necesita una cuenta de Azure Maps y una clave. Si es necesario, siga las instrucciones de [Creación de una cuenta](quick-demo-map-app.md#create-an-account-with-azure-maps) y [Obtención de la clave principal](quick-demo-map-app.md#get-the-primary-key-for-your-account). Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](./how-to-manage-authentication.md).

> [!Tip]
> Para hacer una consulta al servicio de búsqueda, puede usar la [aplicación Postman](https://www.getpostman.com/apps) para crear llamadas de REST o el entorno de desarrollo de API que prefiera.


## <a name="best-practices-for-geocoding-address-search"></a>Procedimientos recomendados para la geocodificación (búsqueda de direcciones)

Al buscar una dirección completa o parcial mediante el servicio Search de Azure Maps, la API lee las palabras clave de la consulta de búsqueda y devuelve las coordenadas de longitud y latitud de la dirección. Este proceso se denomina “geocodificación”. La capacidad de obtener las coordenadas geográficas en un país depende de la cobertura de los datos de carretera y la precisión de la codificación geográfica del servicio de geocodificación.

Consulte la [cobertura de geocodificación](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage) para obtener más información sobre las capacidades de geocodificación de Azure Maps por país o región.

### <a name="limit-search-results"></a>Limitar los resultados de la búsqueda

 Las API del servicio Search de Azure Maps pueden ayudar a limitar los resultados de la búsqueda para que pueda mostrar los datos pertinentes a los usuarios.

   > [!Note]
   > La siguiente lista no incluye todos los parámetros admitidos por las API de Search.

   **Resultados de búsqueda de sesgos geográficos**

   Con el fin de aplicar un sesgo geográfico a los resultados del área relevante para el usuario, debe agregar siempre la ubicación con el máximo detalle posible. Para restringir los resultados de búsqueda, puede agregar los siguientes tipos de entrada:

   1. Use el parámetro `countrySet`, por ejemplo, “US, FR”. De forma predeterminada, se hacen búsquedas en todo el mundo, lo cual puede devolver resultados innecesarios. Si la consulta tiene el parámetro `countrySet`, es posible que la búsqueda devuelva resultados imprecisos. Por ejemplo, al buscar una ciudad denominada **Bellevue**, se devolverán resultados de Estados Unidos y Francia, puesto que en ambos países hay una ciudad que se denomina **Bellevue**.

   2. Puede usar los parámetros `btmRight` y `topleft` para establecer el rectángulo de selección que restringirá la búsqueda a una zona específica del mapa.

   3. Para influir en el área de relevancia de los resultados, puede definir los parámetros coordinados `lat` y `lon`, y establecer el radio de la zona de búsqueda con el parámetro `radius`.


   **Parámetros de búsqueda aproximada**
   
  [Fuzzy Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) de Azure Maps es el servicio recomendado que se utiliza cuando no se sabe cuáles son las entradas de usuario para una consulta de búsqueda. La API combina la búsqueda de puntos de interés (POI) y la geocodificación en una *búsqueda de una sola línea* canónica.

   1. `minFuzzyLevel` y `maxFuzzyLevel` ayudan a devolver coincidencias relevantes incluso cuando los parámetros de consulta no coinciden exactamente con la información deseada. Para obtener el rendimiento y reducir los resultados inusuales, las búsquedas predeterminadas consultan `minFuzzyLevel=1` y `maxFuzzyLevel=2`. Pongamos como ejemplo el término de búsqueda “restrant”, que coincide con “restaurante” cuando el valor `maxFuzzyLevel` se establece en 2. Los niveles predeterminados de aproximación se pueden invalidar según sea necesario.  

   2. También puede clasificar por orden de prioridad el conjunto exacto de tipos de resultados que se van a devolver mediante el parámetro `idxSet`. Con este fin, puede enviar una lista de índices separada por comas sin tener en cuenta el orden de los elementos. Se admiten los índices siguientes:

       * `Addr` - **Intervalos de direcciones**: en algunas calles, hay puntos de la dirección que se interpolan desde el principio hasta el final de la calle. Esos puntos se representan como intervalos de direcciones.
       * `Geo` - **Geografías**: áreas en un mapa que representan una división administrativa de un terreno, es decir, país, estado, ciudad.
       * `PAD` - **Dirección del punto**:  puntos de un mapa cuya dirección específica, con el nombre y el número de la calle, se puede encontrar en un índice, por ejemplo, Soquel Dr 2501. Este valor de idxSet es el nivel más alto de precisión disponible para las direcciones.  
       * `POI` - **Puntos de interés**: puntos de un mapa que merecen atención y pueden ser de interés.  [Get Search Address](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) no devuelve ningún punto de interés.  
       * `Str` - **Calles**: representación de calles en el mapa.
       * `XStr` - **Cruce de calles o intersecciones**:  representación de cruces, lugares en los que dos calles forman una intersección.


       **Ejemplos de uso**:

       * idxSet=POI (solo búsqueda de puntos de interés) 

       * idxSet=PAD,Addr (solo direcciones de búsqueda, PAD= dirección del punto, Addr= intervalo de direcciones)

### <a name="reverse-geocode-and-geography-entity-type-filter"></a>Invertir el filtro de tipo de entidad de código geográfico y geografía

Al realizar una búsqueda de código geográfico inversa con la [API de búsqueda inversa de dirección](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), el servicio puede devolver los polígonos de las áreas administrativas. Al especificar el parámetro `entityType` en la solicitud, puede restringir la búsqueda de los tipos de entidad geográfica especificados. La respuesta resultante contendrá el identificador de geografía y el tipo de entidad coincidente. Si proporciona más de una entidad, el punto de conexión devolverá la **entidad más pequeña que esté disponible**. El identificador de la geometría devuelto puede utilizarse para obtener la geometría de esa ubicación geográfica a través del [servicio para obtener el polígono](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon).

**Solicitud de ejemplo:**

```HTTP
https://atlas.microsoft.com/search/address/reverse/json?api-version=1.0&subscription-key={subscription-key}&query=47.6394532,-122.1304551&language=en-US&entityType=Municipality
```

**Respuesta:**

```JSON
{
    "summary": {
        "queryTime": 14,
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

El parámetro `language` permite seleccionar el idioma de los resultados que la API devuelve. Si no se establece el idioma en la solicitud, de forma predeterminada el servicio de búsqueda usará el idioma más común en el país o región. Además, cuando los datos no están disponibles en el idioma especificado, se usa el idioma predeterminado. Consulte la lista de [idiomas admitidos](https://docs.microsoft.com/azure/azure-maps/supported-languages) en los servicios de Azure Maps por país o región.


### <a name="predictive-mode-autosuggest"></a>Modo predictivo (sugerencia automática)

Para encontrar más coincidencias para las consultas parciales, el parámetro `typeahead` debe establecerse en “true”. La consulta se interpretará como una entrada parcial y la búsqueda pasará al modo predictivo. En caso contrario, el servicio supondrá que se ha pasado toda la información pertinente.

En la consulta de ejemplo siguiente puede ver que se hace la consulta “Microso” al servicio de la dirección de búsqueda con el parámetro `typeahead` establecido en **true**. Si observa la respuesta, puede ver que el servicio de búsqueda interpretó la consulta como una consulta parcial. La respuesta contiene los resultados de la sugerencia automática de la consulta.

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
        "queryTime": 18,
        "numResults": 7,
        "offset": 0,
        "totalResults": 7,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6370891183,
            "lon": -122.123736172
        }
    },
    "results": [
        {
            "type": "Street",
            "id": "US/STR/p0/9438784",
            "score": 2.594099998474121,
            "dist": 314.0590106663596,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
            },
            "position": {
                "lat": 47.63988,
                "lon": -122.12438
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64223,
                    "lon": -122.1256,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63748,
                    "lon": -122.12309,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/1756074",
            "score": 2.592679977416992,
            "dist": 876.0272035824189,
            "address": {
                "streetName": "Microsoft Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Road, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.64032,
                "lon": -122.1344
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64253,
                    "lon": -122.13535,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63816,
                    "lon": -122.13305,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/1470668",
            "score": 2.5290400981903076,
            "dist": 2735.4883918101486,
            "address": {
                "streetName": "Microsoft West Campus Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft West Campus Road, Bellevue, WA 98007"
            },
            "position": {
                "lat": 47.65784,
                "lon": -122.14335
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.65785,
                    "lon": -122.14335,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.65784,
                    "lon": -122.14325,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/12812615",
            "score": 2.527509927749634,
            "dist": 2870.9579016916873,
            "address": {
                "streetName": "Microsoft West Campus Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft West Campus Road, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.66034,
                "lon": -122.1404
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.66039,
                    "lon": -122.14325,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.65778,
                    "lon": -122.13749,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/197588",
            "score": 2.4630401134490967,
            "dist": 878.1404663812472,
            "address": {
                "streetName": "157th Avenue Northeast",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "extendedPostalCode": "980525344, 980525398, 980525399",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "157th Avenue Northeast, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.64351,
                "lon": -122.13056
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64473,
                    "lon": -122.13058,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.6425,
                    "lon": -122.13016,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/3033991",
            "score": 2.0754499435424805,
            "dist": 3655467.8844475765,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Yorkmount, Charlotte",
            },
            "position": {
                "lat": 35.14267,
                "lon": -80.91824
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14287,
                    "lon": -80.91839,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 35.14267,
                    "lon": -80.91814,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/8395877",
            "score": 2.0754499435424805,
            "dist": 3655437.0037482483,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Charlotte",
                "municipality": "Charlotte",
                "countrySecondarySubdivision": "Mecklenburg",
                "countryTertiarySubdivision": "Township 1 Charlotte",
                "countrySubdivision": "NC",
                "countrySubdivisionName": "North Carolina",
                "postalCode": "28273",
                "extendedPostalCode": "282738105, 282738106, 282738108, 2827382, 282738200",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Charlotte, NC 28273"
            },
            "position": {
                "lat": 35.14134,
                "lon": -80.9198
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14274,
                    "lon": -80.92159,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 35.14002,
                    "lon": -80.91824,
                    "valid": true
                }
            }
        }
    ]
}
```


### <a name="uri-encoding-to-handle-special-characters"></a>Codificación de URI para controlar los caracteres especiales 

Para buscar direcciones compuestas, debe codificar el URI para controlar los caracteres especiales en la dirección. Considere este ejemplo de dirección: "1st Avenue & Union Street, Seattle". Es necesario codificar el carácter especial "&" antes de enviar la solicitud. Se recomienda codificar los datos de caracteres en un URI, donde todos los caracteres se codifican mediante un carácter “%” y un valor hexadecimal de dos caracteres que se corresponde a su carácter UTF-8.

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

También puede enviar una lista separada por comas de nombres de marca en la solicitud. Puede usar la lista para restringir los resultados a marcas específicas mediante el parámetro `brandSet`. No importa el orden de los elementos. Cuando se especifican varias marcas, solo se devuelven los resultados que pertenecen (como mínimo) a una de las listas proporcionadas.

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
        "queryTime": 276,
        "numResults": 3,
        "offset": 0,
        "totalResults": 762680,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6413362,
            "lon": -122.1327968
        }
    },
    "results": [
        {
            "type": "POI",
            "id": "US/POI/p0/8831765",
            "score": 5.6631999015808105,
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
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.chevron.com",
                "categories": [
                    "petrol station"
                ],
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
            },
            "position": {
                "lat": 47.63201,
                "lon": -122.13281
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63291,
                    "lon": -122.13414,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63111,
                    "lon": -122.13148,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63222,
                        "lon": -122.13312,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "POI",
            "id": "US/POI/p0/8831752",
            "score": 5.662710189819336,
            "dist": 1330.1278248163273,
            "info": "search:ta:840539001100326-US",
            "poi": {
                "name": "76",
                "phone": "+(1)-(425)-7472126",
                "brands": [
                    {
                        "name": "76"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.76.com",
                "categories": [
                    "petrol station"
                ],
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
                "streetNumber": "2421",
                "streetName": "148Th Ave Ne",
                "municipalitySubdivision": "Redmond, Bridle Trails, Bellevue",
                "municipality": "Redmond, Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "2421 148Th Ave Ne, Bellevue, WA 98007",
                "localName": "Bellevue"
            },
            "position": {
                "lat": 47.63187,
                "lon": -122.14365
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63277,
                    "lon": -122.14498,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63097,
                    "lon": -122.14232,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "minor",
                    "position": {
                        "lat": 47.63187,
                        "lon": -122.14374,
                        "valid": true
                    }
                },
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63186,
                        "lon": -122.14313,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "POI",
            "id": "US/POI/p0/8831764",
            "score": 5.662449836730957,
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
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.texaco.com/",
                "categories": [
                    "petrol station"
                ],
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
            },
            "position": {
                "lat": 47.62843,
                "lon": -122.13628
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62933,
                    "lon": -122.13761,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.62753,
                    "lon": -122.13495,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.62827,
                        "lon": -122.13628,
                        "valid": true
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
        "queryTime": 146,
        "numResults": 6,
        "offset": 0,
        "totalResults": 7,
        "fuzzyLevel": 1
    },
    "results": [
        {
            "type": "Point Address",
            "id": "US/PAD/p0/28725082",
            "score": 9.893799781799316,
            "address": {
                "streetNumber": "400",
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.62039,
                "lon": -122.34928
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62129,
                    "lon": -122.35061,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61949,
                    "lon": -122.34795,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.61982,
                        "lon": -122.34886,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "Street",
            "id": "US/STR/p0/6700384",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.61724,
                "lon": -122.35207
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61825,
                    "lon": -122.35336,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61626,
                    "lon": -122.35078,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/9701953",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.61965,
                "lon": -122.349
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62066,
                    "lon": -122.35041,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61857,
                    "lon": -122.34761,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/11721297",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Downtown Seattle, Denny Regrade, Belltown",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98121",
                "extendedPostalCode": "981211237",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA 98121"
            },
            "position": {
                "lat": 47.61825,
                "lon": -122.35078
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61857,
                    "lon": -122.35078,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61825,
                    "lon": -122.35041,
                    "valid": true
                }
            }
        },
        {
            "type": "Cross Street",
            "id": "US/XSTR/p1/232144",
            "score": 6.754479885101318,
            "address": {
                "streetName": "Broad Street & Valley Street",
                "municipalitySubdivision": "South Lake Union, Seattle",
            },
            "position": {
                "lat": 47.62545,
                "lon": -122.33974
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62635,
                    "lon": -122.34107,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.62455,
                    "lon": -122.33841,
                    "valid": true
                }
            }
        }
    ]
}
```

### <a name="geometry"></a>Geometría

Cuando el tipo de respuesta es **Geometría**, puede incluir el identificador de geometría que se devuelve en el objeto **dataSources** bajo “geometría” e “id”. Por ejemplo, la [API para obtener el polígono](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) permite solicitar los datos de geometría en formato GeoJSON. Por ejemplo, el contorno de ciudad o aeropuerto de un conjunto de entidades. Puede usar estos datos de límites para [geovallas](https://docs.microsoft.com/azure/azure-maps/tutorial-geofence) o [puntos de interés de búsqueda dentro de la geometría](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry).


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
