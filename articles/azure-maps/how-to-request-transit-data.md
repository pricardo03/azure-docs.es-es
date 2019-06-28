---
title: Solicitud de información de transporte en Azure Maps | Microsoft Docs
description: Solicite información sobre el transporte público mediante Mobility Service de Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: e8250763153f7c5b71f3906a560365dadfd55694
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735577"
---
# <a name="request-public-transit-data-using-the-azure-maps-mobility-service"></a>Solicitud de información sobre el transporte público mediante Mobility Service de Azure Maps 

En este artículo se muestra cómo usar [Mobility Service](https://aka.ms/AzureMapsMobilityService) de Azure Maps para solicitar información sobre el transporte público, como las paradas, las rutas y tiempos de los trayectos.

En este artículo, aprenderá a:

* Obtener un identificador de área metropolitana mediante [Get Metro Area API](https://aka.ms/AzureMapsMobilityMetro).
* Solicitar las paradas próximas mediante el servicio [Get Nearby Transit](https://aka.ms/AzureMapsMobilityNearbyTransit).
* Consultar [Get Transit Routes API](https://aka.ms/AzureMapsMobilityTransitRoute) para planear una ruta en transporte público.
* Solicitar el trayecto de la ruta y la programación detallada con [Get Transit Itinerary API](https://aka.ms/https://azure.microsoft.com/services/azure-maps/).


## <a name="prerequisites"></a>Requisitos previos

Para realizar una llamada a cualquier API de transporte público de Azure Maps, necesita una cuenta de Maps y una clave. Para más información sobre cómo crear una cuenta y recuperar una clave, consulte [Administración de la cuenta y las claves de Azure Maps](how-to-manage-account-keys.md).

Este artículo utiliza la [aplicación Postman](https://www.getpostman.com/apps) para generar llamadas REST. Puede usar cualquier entorno de desarrollo de API que prefiera.


## <a name="get-a-metro-area-id"></a>Obtención de un identificador de área metropolitana

Para solicitar información de transporte para una determinada área metropolitana necesitará el `metroId` del área de la que desee solicitar los datos. [Get Metro Area API](https://aka.ms/AzureMapsMobilityMetro) permite solicitar áreas metropolitanas en las que Mobility Service de Azure Maps está disponible. La respuesta incluye detalles como `metroId`, `metroName` y una representación de las rutas del área metropolitana en formato GeoJSON.

Vamos a hacer una solicitud para obtener el área metropolitana del identificador de Seattle-Tacoma. Para solicitar el identificador de un área metropolitana, siga estos pasos:

1. Cree una colección en la que vaya a almacenar las solicitudes. En la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Collection** (Colección). Asigne un nombre a la colección y seleccione el botón **Create** (Crear).

2. Para crear la solicitud, seleccione **New** (Nuevo) otra vez. En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un **nombre de solicitud**, seleccione la colección que creó en el paso anterior como la ubicación en la que se va a guardar la solicitud y, a continuación, seleccione **Save** (Guardar).
    
    ![Creación de una solicitud en Postman](./media/how-to-request-transit-data/postman-new.png)

3. Seleccione el método GET HTTP en la pestaña del generador y escriba la siguiente dirección URL para realizar una solicitud GET.

    ```HTTP
    https://atlas.microsoft.com/mobility/metroArea/id/json?subscription-key={subscription-key}&api-version=1.0&query=47.63096,-122.126
    ```

4. Si la solicitud es correcta, recibirá esta respuesta:

    ```JSON
    {
        "results": [
            {
                "metroId": "522",
                "metroName": "Seattle–Tacoma–Bellevue, WA",
                "geometry": {
                    "type": "Polygon",
                    "coordinates": [
                        [
                            [
                                -121.99604,
                                47.16147
                            ],
                            [
                                -121.97051,
                                47.17222
                            ],
                            [
                                -121.96308,
                                47.17671
                            ],
                            [
                                -121.95725,
                                47.18314
                            ],
                            ...,
                            ...,
                            ...,
                            ...,
                            [
                                -122.18711,
                                47.15571
                            ],
                            [
                                -122.01525,
                                47.16008
                            ],
                            [
                                -122.00553,
                                47.15919
                            ],
                            [
                                -121.99604,
                                47.16147
                            ]
                        ]
                    ]
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 48.5853,
                        "longitude": -124.80934
                    },
                    "btmRightPoint": {
                        "latitude": 46.90534,
                        "longitude": -121.55032
                    }
                }
            }
        ]
    }
    ```

5. Copie el `metroId` para usarlo más adelante.

## <a name="request-nearby-transit-stops"></a>Solicitud de paradas cercanas

El servicio [Get Nearby Transit](https://aka.ms/AzureMapsMobilityNearbyTransit) de Azure Maps permite buscar objetos de transporte, como paradas de transporte público y bicicletas compartidas alrededor de un punto concreto y devuelve los detalles de ese objeto. A continuación vamos a solicitar al servicio que busque paradas de transporte público cercanas, en un radio de 300 metros a partir de un punto concreto. En la solicitud debemos incluir el `metroId` recuperado anteriormente.

Para realizar una solicitud a [Get Nearby Transit](https://aka.ms/AzureMapsMobilityNearbyTransit), siga estos pasos:

1. En Postman, haga clic en **New Request** (Nueva solicitud)  | **GET request** (Solicitud GET) y asígnele el nombre **Get Nearby stops** (Obtener paradas cercanas).

2. En la pestaña Builder (Generador), seleccione el método HTTP **GET**, escriba la siguiente dirección URL de solicitud para el punto de conexión de API y haga clic en **Send** (Enviar).

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=47.63096,-122.126&radius=300&objectType=stop
    ```

3. Si la solicitud es correcta, la estructura de la respuesta se parecerá a la siguiente:

    ```JSON
    {
        "results": [
            {
                "id": "2060603",
                "type": "Stop",
                "objectDetails": {
                    "stopKey": "71300",
                    "stopName": "Ne 24th St & 162nd Ave Ne",
                    "mainTransitType": "BUS",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631504,
                    "longitude": -122.125275
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.63241381296315,
                        "longitude": -122.12659096560266
                    },
                    "btmRightPoint": {
                        "latitude": 47.630594172088166,
                        "longitude": -122.12395908007201
                    }
                }
            },
            {
                "id": "2061020",
                "type": "Stop",
                "objectDetails": {
                    "stopKey": "68372",
                    "stopName": "Ne 24th St & 160th Ave Ne",
                    "mainTransitType": "BUS",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631409,
                    "longitude": -122.127136
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632318791818726,
                        "longitude": -122.12845199584025
                    },
                    "btmRightPoint": {
                        "latitude": 47.63049919323126,
                        "longitude": -122.12582004983427
                    }
                }
            },
            {
                "id": "2060604",
                "type": "Stop",
                "objectDetails": {
                    "stopKey": "71310",
                    "stopName": "Ne 24th St & 160th Ave Ne",
                    "mainTransitType": "BUS",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631565,
                    "longitude": -122.127808
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632474784183636,
                        "longitude": -122.12912401149087
                    },
                    "btmRightPoint": {
                        "latitude": 47.630655200865796,
                        "longitude": -122.12649203418405
                    }
                }
            }
        ]
    }    
    ```

Si observa detenidamente la estructura de respuesta, verá que contiene los parámetros de cada objeto de transporte, como `id`, `type`, `stopName`, `mainTransitType`, `mainAgencyName` y la posición (coordenadas) del objeto.

Para entenderlo, usaremos el `id` de una de las paradas de autobús como origen de nuestra ruta en la sección siguiente.  


## <a name="request-a-transit-route"></a>Solicitud de una ruta de transporte

[Get Transit Routes API](https://aka.ms/AzureMapsMobilityTransitRoute) de Azure Maps permite planear un viaje gracias a que devuelve las mejores opciones de ruta entre un origen y un destino. El servicio proporciona varios modos de transporte (por ejemplo, a pie, en bici y en transporte público). A continuación buscaremos una ruta a partir de la parada de autobús más cercana a Space Needle (Seattle).

### <a name="get-location-coordinates-for-destination"></a>Obtención de las coordenadas de posición de destino

Para obtener las coordenadas de posición de Space Needle usaremos [Get Search Fuzzy](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) de Azure Maps.

Para realizar una solicitud a Get Search Fuzzy, siga estos pasos:

1. En Postman, haga clic en **New Request** (Nueva solicitud)  | **GET request** (Solicitud GET) y asígnele el nombre **Get location coordinates** (Obtener coordenadas de la ubicación).

2.  En la pestaña Builder (Generador), seleccione el método HTTP **GET**, escriba la siguiente dirección URL de solicitud y haga clic en **Send** (Enviar).
 
    ```HTTP
    https://atlas.microsoft.com/search/fuzzy/json?subscription-key={subscription-key}&api-version=1.0&query=space needle
    ```
    
3. Si observa detenidamente la respuesta, contiene varias ubicaciones en los resultados para Space Needle y también contiene las coordenadas de ubicación para cada una de ellas en **position**. Copie `lat` y `lon` de position del primer resultado.
    
   ```JSON
   {
        "summary": {
            "query": "space needle",
            "queryType": "NON_NEAR",
            "queryTime": 61,
            "numResults": 8,
            "offset": 0,
            "totalResults": 24,
            "fuzzyLevel": 1
        },
        "results": [
            {
                "type": "POI",
                "id": "US/POI/p0/8309323",
                "score": 4.674,
                "info": "search:ta:840539000511573-US",
                "poi": {
                    "name": "Space Needle",
                    "phone": "+(1)-(206)-9052100",
                    "url": "www.spaceneedle.com",
                    "categories": [
                        "important tourist attraction",
                        "monument"
                    ],
                    "classifications": [
                        {
                            "code": "IMPORTANT_TOURIST_ATTRACTION",
                            "names": [
                                {
                                    "nameLocale": "en-US",
                                    "name": "important tourist attraction"
                                },
                                {
                                    "nameLocale": "en-US",
                                    "name": "monument"
                                }
                            ]
                        }
                    ]
                },
                "address": {
                    "streetNumber": "400",
                    "streetName": "Broad St",
                    "municipalitySubdivision": "South Lake Union, Seattle, Lower Queen Anne",
                    "municipality": "Seattle",
                    "countrySecondarySubdivision": "King",
                    "countryTertiarySubdivision": "Seattle",
                    "countrySubdivision": "WA",
                    "postalCode": "98109",
                    "countryCode": "US",
                    "country": "United States Of America",
                    "countryCodeISO3": "USA",
                    "freeformAddress": "400 Broad St, Seattle, WA 98109",
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
            ...,
            ...,
            ...
            
        ]
    }
    ``` 
    

### <a name="request-route"></a>Solicitud de ruta

Para realizar una solicitud de ruta, siga estos pasos:

1. En Postman, haga clic en **New Request** (Nueva solicitud)  | **GET request** (Solicitud GET) y asígnele el nombre **Get Route info** (Obtener información de ruta).

2. En la pestaña Builder (Generador), seleccione el método HTTP **GET**, escriba la siguiente dirección URL de solicitud para el punto de conexión de API y haga clic en **Send** (Enviar).

    Solicitaremos las rutas de transporte público en autobús; para ello, especificaremos los parámetros `modeType` y `transitType`. La solicitud de dirección URL contiene las ubicaciones recuperadas en las secciones anteriores. Como `originType` ahora tenemos **stopId** y, como `destionationType`, la posición **position**.

    Consulte la [lista de parámetros de URI](https://aka.ms/AzureMapsMobilityTransitRoute#uri-parameters) que puede usar en la solicitud a [Get Transit Routes API](https://aka.ms/AzureMapsMobilityTransitRoute). 
  
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/route/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&originType=stopId&origin=2060603&destionationType=position&destination=47.62039,-122.34928&modeType=publicTransit&transitType=bus
    ```

3. Si la solicitud es correcta, la estructura de la respuesta se parecerá a la siguiente:

    ```JSON
    {
        "results": [
            {
                "itineraryId": "302c38dd-6585-4fa1-bf78-44ebbc183e0c---2019040384C30774B4B94F178E7748644A476596:0---522",
                "departureTime": "2019-04-03T14:21:34-07:00",
                "arrivalTime": "2019-04-03T15:15:53-07:00",
                "travelTimeInSeconds": 3259,
                "numberOfLegs": 10,
                "legs": [
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:21:34-07:00",
                        "legEndTime": "2019-04-03T14:28:19-07:00",
                        "caption": "156th Avenue Northeast",
                        "lengthInMeters": 497
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T14:28:19-07:00",
                        "legEndTime": "2019-04-03T14:29:20-07:00",
                        "caption": "245"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T14:29:20-07:00",
                        "legEndTime": "2019-04-03T14:32:00-07:00",
                        "caption": "245",
                        "lengthInMeters": 1350
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:32:01-07:00",
                        "legEndTime": "2019-04-03T14:33:07-07:00",
                        "caption": "156th Avenue Northeast",
                        "lengthInMeters": 63
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T14:33:07-07:00",
                        "legEndTime": "2019-04-03T14:38:00-07:00",
                        "caption": "545"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T14:38:00-07:00",
                        "legEndTime": "2019-04-03T14:59:47-07:00",
                        "caption": "545",
                        "lengthInMeters": 16441
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:59:48-07:00",
                        "legEndTime": "2019-04-03T15:03:53-07:00",
                        "caption": "Denny Way",
                        "lengthInMeters": 308
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T15:03:53-07:00",
                        "legEndTime": "2019-04-03T15:07:26-07:00",
                        "caption": "8"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T15:07:26-07:00",
                        "legEndTime": "2019-04-03T15:12:12-07:00",
                        "caption": "8",
                        "lengthInMeters": 1057
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T15:12:13-07:00",
                        "legEndTime": "2019-04-03T15:15:53-07:00",
                        "caption": "47.6205,-122.3493",
                        "lengthInMeters": 268
                    }
                ]
            },
            ...,
            {
                "itineraryId": "302c38dd-6585-4fa1-bf78-44ebbc183e0c---2019040384C30774B4B94F178E7748644A476596:2---522",
                "departureTime": "2019-04-03T14:21:34-07:00",
                "arrivalTime": "2019-04-03T15:19:18-07:00",
                "travelTimeInSeconds": 3464,
                "numberOfLegs": 10,
                "legs": [
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:21:34-07:00",
                        "legEndTime": "2019-04-03T14:28:19-07:00",
                        "caption": "156th Avenue Northeast",
                        "lengthInMeters": 497
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T14:28:19-07:00",
                        "legEndTime": "2019-04-03T14:29:20-07:00",
                        "caption": "245"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T14:29:20-07:00",
                        "legEndTime": "2019-04-03T14:32:00-07:00",
                        "caption": "245",
                        "lengthInMeters": 1350
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:32:01-07:00",
                        "legEndTime": "2019-04-03T14:33:07-07:00",
                        "caption": "156th Avenue Northeast",
                        "lengthInMeters": 63
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T14:33:07-07:00",
                        "legEndTime": "2019-04-03T14:38:00-07:00",
                        "caption": "545"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T14:38:00-07:00",
                        "legEndTime": "2019-04-03T15:01:00-07:00",
                        "caption": "545",
                        "lengthInMeters": 17400
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T15:01:01-07:00",
                        "legEndTime": "2019-04-03T15:04:59-07:00",
                        "caption": "3rd Avenue",
                        "lengthInMeters": 269
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T15:04:59-07:00",
                        "legEndTime": "2019-04-03T15:09:14-07:00",
                        "caption": "33"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T15:09:14-07:00",
                        "legEndTime": "2019-04-03T15:12:52-07:00",
                        "caption": "33,24",
                        "lengthInMeters": 947
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T15:12:53-07:00",
                        "legEndTime": "2019-04-03T15:19:18-07:00",
                        "caption": "47.6205,-122.3493",
                        "lengthInMeters": 474
                    }
                ]
            }
        ]
    }
    ```

4. Si observa detenidamente, hay varias rutas de **Bus** en la respuesta. Cada ruta tiene un **ID de itinerario** único y un resumen que describe cada trayecto de la ruta. Ahora solicitaremos detalles de la ruta más rápida mediante el uso de `itineraryId` en la respuesta.

## <a name="request-fastest-route-itinerary"></a>Solicitud del itinerario más rápido

El servicio [Get Transit Itinerary](https://aka.ms/AzureMapsMobilityTransitItinerary) de Azure Maps permite solicitar datos de una ruta concreta con el uso del **ID de itinerario** que devuelve [Get Transit Routes API](https://aka.ms/AzureMapsMobilityTransitRoute). Para realizar una solicitud, siga estos pasos:

1. En Postman, haga clic en **New Request** (Nueva solicitud)  | **GET request** (Solicitud GET) y asígnele el nombre **Get Transit info** (Obtener información de transporte).

2. En la pestaña Builder (Generador), seleccione el método HTTP **GET**, escriba la siguiente dirección URL de solicitud para el punto de conexión de API y haga clic en **Send** (Enviar).

    Estableceremos el parámetro `detailType` en **geometry** para que la respuesta contenga la información de las paradas de transporte público y navegación detallada para las opciones de ruta a pie y en bici.

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/itinerary/json?api-version=1.0&subscription-key={subscription-key}&query={itineraryId}&detailType=geometry
    ```
    
3. Si la solicitud es correcta, la estructura de la respuesta se parecerá a la siguiente:

    ```JSON
    {
    "departureTime": "2019-05-01T11:16:56-07:00",
    "arrivalTime": "2019-05-01T12:23:45-07:00",
    "legs": [
                {
                    "legType": "Walk",
                    "legStartTime": "2019-05-01T11:16:56-07:00",
                    "legEndTime": "2019-05-01T11:24:06-07:00",
                    "walkingSteps": [
                        {
                            "direction": {
                                "relativeDirection": "left"
                            },
                            "streetName": "Northeast 24th Street"
                        },
                        {
                            "direction": {
                                "relativeDirection": "right"
                            },
                            "streetName": "156th Avenue Northeast"
                        }
                    ],
                    "walkingOrigin": {
                        "latitude": 47.63096,
                        "longitude": -122.126
                    },
                    "walkingDestination": {
                        "latitude": 47.631843,
                        "longitude": -122.132294
                    },
                    "geometry": {
                        "type": "LineString",
                        "coordinates": [
                            [
                                -122.126,
                                47.63096
                            ],
                            [
                                -122.12645,
                                47.63099
                            ],
                            ...,
                            ...,
                            [
                                -122.1323,
                                47.63184
                            ]
                        ]
                    }
                },
                {
                    "legType": "Wait",
                    "legStartTime": "2019-05-01T11:24:06-07:00",
                    "legEndTime": "2019-05-01T11:25:07-07:00",
                    "lineGroup": {
                        "lineGroupId": 666074,
                        "agencyId": 5872,
                        "agencyName": "Metro Transit",
                        "lineNumber": "245",
                        "caption1": "Kirkland Transit Center - Crossroads - Factoria",
                        "caption2": "245 Kirkland Transit Center - Crossroads - Factoria",
                        "color": "347E5D",
                        "transitType": "Bus"
                    },
                    "line": {
                        "lineId": 2756624,
                        "lineGroupId": 666074,
                        "direction": "forward",
                        "agencyId": 5872,
                        "lineNumber": "245",
                        "destination": "Kirkland Crossroads"
                    },
                    "stops": [
                        {
                            "stopId": 2061109,
                            "stopKey": "68788",
                            "stopName": "156th Ave NE & NE 24th St",
                            "position": {
                                "latitude": 47.631844,
                                "longitude": -122.132248
                            },
                            "mainTransitType": "Bus",
                            "mainAgencyId": 5872
                        },
                        {
                            "stopId": 2061059,
                            "stopKey": "68498",
                            "stopName": "156th Ave NE & Overlake Transit Center - Bay 8",
                            "position": {
                                "latitude": 47.643986,
                                "longitude": -122.132187
                            },
                            "mainTransitType": "Bus",
                            "mainAgencyId": 5872
                        }
                    ],
                    "waitOnVehicle": "false"
                },
                {
                    "legType": "Bus",
                    "legStartTime": "2019-05-01T11:25:07-07:00",
                    "legEndTime": "2019-05-01T11:30:00-07:00",
                    "lineGroup": {
                        "lineGroupId": 666074,
                        "agencyId": 5872,
                        "agencyName": "Metro Transit",
                        "lineNumber": "245",
                        "caption1": "Kirkland Transit Center - Crossroads - Factoria",
                        "caption2": "245 Kirkland Transit Center - Crossroads - Factoria",
                        "color": "347E5D",
                        "transitType": "Bus"
                    },
                    "line": {
                        "lineId": 2756624,
                        "lineGroupId": 666074,
                        "direction": "forward",
                        "agencyId": 5872,
                        "lineNumber": "245",
                        "destination": "Kirkland Crossroads"
                    },
                    "stops": [
                        {
                            "stopId": 2061109,
                            "stopKey": "68788",
                            "stopName": "156th Ave NE & NE 24th St",
                            "position": {
                                "latitude": 47.631844,
                                "longitude": -122.132248
                            },
                            "mainTransitType": "Bus",
                            "mainAgencyId": 5872
                        },
                        ...,
                        ...,
                        {
                            "stopId": 2061059,
                            "stopKey": "68498",
                            "stopName": "156th Ave NE & Overlake Transit Center - Bay 8",
                            "position": {
                                "latitude": 47.643986,
                                "longitude": -122.132187
                            },
                            "mainTransitType": "Bus",
                            "mainAgencyId": 5872
                        }
                    ],
                    "geometry": {
                        "type": "LineString",
                        "coordinates": [
                            [
                                -122.13235,
                                47.63184
                            ],
                            ...,
                            ...,
                            [
                                -122.1323,
                                47.64398
                            ]
                        ]
                    }
                },
                ...,
                ...,
                ...,
                {
                    "legType": "Tram",
                    "legStartTime": "2019-05-01T12:20:00-07:00",
                    "legEndTime": "2019-05-01T12:22:00-07:00",
                    "lineGroup": {
                        "lineGroupId": 4083239,
                        "agencyId": 1360766,
                        "agencyName": "Seattle Monorail",
                        "lineNumber": "Monorail",
                        "caption1": "Seattle Center - Westlake Center",
                        "caption2": "MONORAIL Seattle Center - Westlake Center",
                        "color": "00AEEF",
                        "transitType": "Tram"
                    },
                    "line": {
                        "lineId": 3769726,
                        "lineGroupId": 4083239,
                        "direction": "backward",
                        "agencyId": 1360766,
                        "lineNumber": "Monorail",
                        "destination": "Seattle Center"
                    },
                    "stops": [
                        {
                            "stopId": 32962125,
                            "stopName": "Westlake Station",
                            "position": {
                                "latitude": 47.611417,
                                "longitude": -122.337089
                            },
                            "mainTransitType": "Tram",
                            "mainAgencyId": 1360766
                        },
                        {
                            "stopId": 32962134,
                            "stopName": "Seattle Center",
                            "position": {
                                "latitude": 47.62123,
                                "longitude": -122.349746
                            },
                            "mainTransitType": "Tram",
                            "mainAgencyId": 1360766
                        }
                    ],
                    "geometry": {
                        "type": "LineString",
                        "coordinates": [
                            [
                                -122.3369,
                                47.61201
                            ],
                            ...,
                            ...,
                            [
                                -122.34973,
                                47.6212
                            ]
                        ]
                    }
                },
                {
                    "legType": "PathWayWalk",
                    "legStartTime": "2019-05-01T12:22:00-07:00",
                    "legEndTime": "2019-05-01T12:23:45-07:00"
                }
          ]
    }
    ```

## <a name="next-steps"></a>Pasos siguientes

Aprenda a solicitar datos en tiempo real mediante Mobility Service:

> [!div class="nextstepaction"]
> [Cómo solicitar datos en tiempo real](how-to-request-real-time-data.md)

Explore la documentación de la API Mobility Service de Azure Maps.

> [!div class="nextstepaction"]
> [Documentación de Mobility Service API](https://aka.ms/AzureMapsMobilityService)

