---
title: Cómo solicitar datos en tiempo real en Azure Maps | Microsoft Docs
description: Solicite datos en tiempo real mediante Azure Maps Mobility Service.
author: walsehgal
ms.author: v-musehg
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 5f69bcafbc3abb39b4185e0144ff6029808c0704
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75432972"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Solicitud de datos en tiempo real mediante Azure Maps Mobility Service

En este artículo se muestra cómo usar Azure Maps [Mobility Service](https://aka.ms/AzureMapsMobilityService) para solicitar datos de tránsito en tiempo real.

En este artículo, aprenderá a:


 * Solicitar las siguientes llegadas en tiempo real para todas las líneas que llegan a una parada determinada.
 * Solicitar información en tiempo real de una estación de aparcamiento de bicicletas determinado.


## <a name="prerequisites"></a>Prerequisites

Para realizar una llamada a cualquier API de transporte público de Azure Maps, necesita una cuenta de Maps y una clave. Para más información sobre la creación de una cuenta y la obtención de una clave, siga las instrucciones que se indican en [Creación de una cuenta](quick-demo-map-app.md#create-an-account-with-azure-maps) para crear una suscripción a una cuenta de Azure Maps, y siga los pasos para [obtener la clave principal](quick-demo-map-app.md#get-the-primary-key-for-your-account) para su cuenta. Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](./how-to-manage-authentication.md).


Este artículo utiliza la [aplicación Postman](https://www.getpostman.com/apps) para generar llamadas REST. Puede usar cualquier entorno de desarrollo de API que prefiera.


## <a name="request-real-time-arrivals-for-a-stop"></a>Solicitud de llegadas en tiempo real en una parada

Para solicitar datos de llegadas en tiempo real para una parada de transporte público determinada, deberá realizar una solicitud a la [API de llegadas en tiempo real](https://aka.ms/AzureMapsMobilityRealTimeArrivals) de Azure Maps [Mobility Service](https://aka.ms/AzureMapsMobilityService). Necesitará los valores de **metroID** y **stopID** para completar la solicitud. Para más información sobre cómo solicitar estos parámetros, consulte nuestra guía paso a paso para [solicitar rutas de transporte público](https://aka.ms/AMapsHowToGuidePublicTransitRouting). 

Usemos "522" como identificador de metro, que es el id. de metro de la zona "Seattle – Tacoma – Bellevue, WA" y usemos el identificador de parada "522---2060603", que es una parada de bus en "Ne 24th St & 162nd Ave Ne, Bellevue WA". Para solicitar los siguientes cinco datos de llegadas en tiempo real para todas las siguientes llegadas en vivo de esta parada, complete los pasos siguientes:

1. Cree una colección en la que vaya a almacenar las solicitudes. En la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Collection** (Colección). Asigne un nombre a la colección y seleccione el botón **Create** (Crear).

2. Para crear la solicitud, seleccione **New** (Nuevo) otra vez. En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un **nombre de solicitud**, seleccione la colección que creó en el paso anterior como la ubicación en la que se va a guardar la solicitud y, a continuación, seleccione **Save** (Guardar).

    ![Creación de una solicitud en Postman](./media/how-to-request-transit-data/postman-new.png)

3. Seleccione el método GET HTTP en la pestaña del generador y escriba la siguiente dirección URL para realizar una solicitud GET.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

4. Si la solicitud es correcta, recibirá esta respuesta:  Observe que el parámetro "scheduleType" define si la hora de llegada estimada se basa en datos en tiempo real o en datos estáticos.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```


## <a name="real-time-data-for-bike-docking-station"></a>Datos en tiempo real para la estación de aparcamiento de bicicletas

La [API para obtener información de aparcamiento de tránsito](https://aka.ms/AzureMapsMobilityTransitDock) de Azure Maps Mobility Service permite solicitar información estática y en tiempo real, como la disponibilidad y la información de vacante para una estación de aparcamiento de bicicletas o moto determinada. Realizaremos una solicitud para obtener datos en tiempo real para una estación de aparcamiento de bicicletas.

Para realizar una solicitud a la API de para obtener información de aparcamiento de tránsito, necesitará el valor de **dockId** de esa estación. Puede obtener el identificador del aparcamiento al realizar una solicitud de búsqueda a la [API para obtener información de tránsito cercado](https://aka.ms/AzureMapsMobilityNearbyTransit) y establecer el parámetro **objectType** en "bikeDock". Siga los pasos siguientes para obtener datos en tiempo real de una estación de aparcamiento de bicicletas.


### <a name="get-dock-id"></a>Obtener el identificador de aparcamiento

Para obtener el valor de **dockID**, siga estos pasos y realice una solicitud a la API para obtener el tránsito cercano:

1. En Postman, haga clic en **New Request** (Nueva solicitud)  | **GET request** (Solicitud GET) y asígnele el nombre **Get dock ID** (Obtener id. de aparcamiento).

2.  En la pestaña Builder (Generador), seleccione el método HTTP **GET**, escriba la siguiente dirección URL de solicitud y haga clic en **Send** (Enviar).
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. Si la solicitud es correcta, recibirá esta respuesta: Observe que ahora tenemos el valor **id** en la respuesta, que se puede usar posteriormente como parámetro de consulta en la solicitud a la API para obtener información de aparcamiento de tránsito.

    ```JSON
    {
        "results": [
            {
                "id": "121---4640799",
                "type": "bikeDock",
                "objectDetails": {
                    "availableVehicles": 0,
                    "vacantLocations": 31,
                    "lastUpdated": "2019-09-07T00:55:19Z",
                    "operatorInfo": {
                        "id": "121---80",
                        "name": "Citi Bike"
                    }
                },
                "position": {
                    "latitude": 40.767128,
                    "longitude": -73.962243
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 40.768039,
                        "longitude": -73.963413
                    },
                    "btmRightPoint": {
                        "latitude": 40.766216,
                        "longitude": -73.961072
                    }
                }
            }
        ]
    }
    ```


### <a name="get-real-time-bike-dock-status"></a>Obtener el estado de aparcamiento de bicicletas en tiempo real

Siga los pasos siguientes para realizar una solicitud a la API para obtener información de aparcamiento de tránsito y obtener datos en tiempo real para el aparcamiento seleccionado.

1. En Postman, haga clic en **New Request** (Nueva solicitud)  | **GET request** (Solicitud GET) y asígnele el nombre **Get real-time dock data** (Obtener datos de aparcamiento en tiempo real).

2.  En la pestaña Builder (Generador), seleccione el método HTTP **GET**, escriba la siguiente dirección URL de solicitud y haga clic en **Send** (Enviar).
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. Después de una solicitud correcta, recibirá una respuesta de la estructura siguiente:

    ```JSON
    {
        "availableVehicles": 0,
        "vacantLocations": 31,
        "position": {
            "latitude": 40.767128,
            "longitude": -73.962246
        },
        "lastUpdated": "2019-09-07T00:55:19Z",
        "operatorInfo": {
            "id": "121---80",
            "name": "Citi Bike"
        }
    }
    ```


## <a name="next-steps"></a>Pasos siguientes

Aprenda a solicitar datos de tránsito mediante Mobility Service:

> [!div class="nextstepaction"]
> [Cómo solicitar datos de tránsito](how-to-request-transit-data.md)

Explore la documentación de la API de Azure Maps Mobility Service:

> [!div class="nextstepaction"]
> [Documentación de Mobility Service API](https://aka.ms/AzureMapsMobilityService)
