---
title: Cómo solicitar datos en tiempo real en Azure Maps | Microsoft Docs
description: Solicitar datos en tiempo real mediante el servicio de movilidad de Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 4d8d34d8dec52dd9173cea80c39097f46d32bff5
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735487"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Solicitud de datos en tiempo real mediante el servicio de movilidad de Azure Maps

Este artículo muestra cómo usar Azure Maps [Mobility Service](https://aka.ms/AzureMapsMobilityService) para solicitar datos de tránsito en tiempo real.

En este artículo, aprenderá cómo:


 * Solicitar siguientes llegadas en tiempo real para todas las líneas que llegan a la detención determinada
 * Solicitar información en tiempo real de una estación de acoplamiento de bicicleta determinado.


## <a name="prerequisites"></a>Requisitos previos

Para realizar llamadas a la API de tránsito pública Azure Maps, necesita una cuenta de Maps y la clave. Para más información sobre cómo crear una cuenta y recuperar una clave, consulte [Administración de la cuenta y las claves de Azure Maps](how-to-manage-account-keys.md).

Este artículo utiliza la [aplicación Postman](https://www.getpostman.com/apps) para generar llamadas REST. Puede usar cualquier entorno de desarrollo de API que prefiera.


## <a name="request-real-time-arrivals-for-a-stop"></a>Llegadas en tiempo real de solicitud de detención

Para solicitar datos de llegadas en tiempo real para un delimitador de tránsito pública determinada, deberá realizar una solicitud para el [en tiempo real las llegadas API](https://aka.ms/AzureMapsMobilityRealTimeArrivals) de las asignaciones de Azure [Mobility Service](https://aka.ms/AzureMapsMobilityService). Necesitará el **metroID** y **stopID** para completar la solicitud. Para obtener más información sobre cómo solicitar estos parámetros, consulte nuestros procedimientos guía [solicitar las rutas de tránsito pública](https://aka.ms/AMapsHowToGuidePublicTransitRouting). 

Vamos a usar "522" como nuestro metro identificador, que es el metro Id. de área "Seattle – Tacoma: Bellevue, WA" y use el Id. de detención "2060603", que es un bus detenerse en "Ne 24 St & Ave 162nd Ne, Bellevue WA". Para solicitar los siguientes cinco datos llegadas en tiempo real para todas las llegadas en vivo siguientes en este punto, complete los pasos siguientes:

1. Crear una colección en la que se va a almacenar las solicitudes. En la aplicación Postman, seleccione **New**. En el **crear nuevo** ventana, seleccione **colección**. Nombre de la colección y seleccione el **crear** botón.

2. Para crear la solicitud, seleccione **New** nuevo. En el **crear nuevo** ventana, seleccione **solicitar**. Escriba un **nombre de la solicitud** para la solicitud, seleccione la colección que creó en el paso anterior como la ubicación en la que se va a guardar la solicitud y, a continuación, seleccione **guardar**.

    ![Crear una solicitud en Postman](./media/how-to-request-transit-data/postman-new.png)

3. Seleccione el método HTTP GET en la pestaña builder y escriba la dirección URL siguiente para crear una solicitud GET.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=2060603&transitType=bus
    ```

4. Después de una solicitud correcta, recibirá la respuesta siguiente.  Tenga en cuenta ese parámetro scheduleType define si la hora de llegada estimado se basa en datos en tiempo real o estáticos.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 4,
                "scheduleType": "realTime",
                "patternId": 3860436,
                "line": {
                    "lineId": 2756599,
                    "lineGroupId": 666063,
                    "direction": "forward",
                    "agencyId": 5872,
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": 2060603,
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 30,
                "scheduleType": "scheduledTime",
                "patternId": 3860436,
                "line": {
                    "lineId": 2756599,
                    "lineGroupId": 666063,
                    "direction": "forward",
                    "agencyId": 5872,
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": 2060603,
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }


## Real-time availability and vacancy information for bike docking station

The [Get Transit Dock Info API](https://aka.ms/AzureMapsMobilityTransitDock) of the Azure Maps Mobility Service, allows to request static and real-time information for a given bike or scooter docking station. We will make a request to get real-time data for a docking station for bikes. 

In order to make a request to the Get Transit Dock Info API, you will need the **dockId** for that station. You can get the dock ID by making a search request to the [Get Nearby Transit API](https://aka.ms/AzureMapsMobilityNearbyTransit) and setting the **objectType** parameter to "bikeDock". Follow the steps below to get real-time data of a docking station for bikes.


### Get dock ID

To get **dockID**, follow the steps below to make a request to the Get Nearby Transit API:

1. In Postman, click **New Request** | **GET request** and name it **Get dock ID**.

2.  On the Builder tab, select the **GET** HTTP method, enter the following request URL, and click **Send**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. Después de una solicitud correcta, recibirá la respuesta siguiente. Tenga en cuenta que ahora tenemos la **id** en la respuesta, que se puede usar posteriormente como un parámetro de consulta en la solicitud a la API de información obtener Dock de tránsito.

    ```JSON
    {
        "results": [
            {
                "id": "121---4640799",
                "type": "bikeDock",
                "objectDetails": {
                    "availableVehicles": 0,
                    "vacantLocations": 30,
                    "lastUpdated": "2019-05-21T20:06:59-04:00",
                    "operatorInfo": {
                        "id": "80",
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


### <a name="get-real-time-bike-dock-status"></a>Obtener el estado de acoplamiento de bicicleta en tiempo real

Siga los pasos siguientes para realizar una solicitud en el tránsito acoplar información de API de Get para obtener datos en tiempo real para la base seleccionada.

1. En Postman, haga clic en **nueva solicitud** | **solicitud GET** y asígnele el nombre **obtener datos en tiempo real de acoplamiento**.

2.  En la pestaña Builder, seleccione el **obtener** método HTTP, escriba la dirección URL de solicitud siguiente y haga clic en **enviar**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. Después de una solicitud correcta, recibirá una respuesta de la estructura siguiente:

    ```JSON
    {
        "availableVehicles": 1,
        "vacantLocations": 29,
        "position": {
            "latitude": 40.767128,
            "longitude": -73.962246
        },
        "lastUpdated": "2019-05-21T20:26:47-04:00",
        "operatorInfo": {
            "id": "80",
            "name": "Citi Bike"
        }
    }
    ```


## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo solicitar datos de tránsito mediante el servicio de movilidad:

> [!div class="nextstepaction"]
> [Cómo solicitar datos de tránsito](how-to-request-transit-data.md)

Explore la documentación de API del servicio de movilidad de Azure Maps:

> [!div class="nextstepaction"]
> [Documentación de API del servicio de movilidad](https://aka.ms/AzureMapsMobilityService)
