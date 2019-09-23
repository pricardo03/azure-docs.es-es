---
title: Creación de una geovalla con Azure Maps | Microsoft Docs
description: Configure una geovalla con Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a020ef91e52a5d801557399df827d3641bfb974e
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70934188"
---
# <a name="set-up-a-geofence-by-using-azure-maps"></a>Configuración de una geovalla con Azure Maps

Este tutorial le guía por los pasos básicos para configurar la geovalla con Azure Maps. El escenario que tratamos en este tutorial es ayudar a los administradores de sitios de construcción a supervisar los equipos potencialmente peligrosos que se mueven más allá de las áreas de construcción designadas. Un sitio de construcción implica regulaciones y equipos costosos. Por lo general requiere que el equipo permanezca dentro del sitio de construcción y no salga sin permiso.

Vamos a usar Data Upload API de Azure Maps para almacenar una geovalla y Geofence API de Azure Maps para comprobar la ubicación del equipo en relación con la geovalla. Usaremos Azure Event Grid para transmitir los resultados de la geovalla y establecer una notificación basada en los resultados de la geovalla.
Para más información sobre Event Grid, consulte [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).
En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Cargar el área de la geovalla en el servicio de datos de Azure Maps con Data Upload API.
> *   Configurar una instancia de Event Grid para controlar los eventos de la geovalla.
> *   Configurar el controlador de eventos de la geovalla.
> *   Configurar alertas en respuesta a eventos de la geovalla con Logic Apps.
> *   Usar las API del servicio de geovalla de Azure Maps para realizar un seguimiento de si un recurso de construcción está dentro del sitio de construcción o no.


## <a name="prerequisites"></a>Requisitos previos

### <a name="create-an-azure-maps-account"></a>Crear una cuenta de Azure Maps 

Para completar los pasos de este tutorial, siga las instrucciones de [Administrar cuentas](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account) para crear una suscripción de cuenta de Azure Maps con el plan de tarifa S1 y siga los pasos de [Obtención de la clave principal](./tutorial-search-location.md#getkey) para obtener la clave de suscripción principal de la cuenta.

## <a name="upload-geofences"></a>Carga de geovallas

Para cargar la geovalla en el sitio de construcción mediante Data Upload API, se va a usar la aplicación Postman. Para este tutorial, se asume que hay un área total del sitio en construcción, que es un parámetro difícil que el equipo de construcción no debe infringir. Las infracciones de esta valla son un ataque grave y se notifican a Operations Manager. Se puede utilizar un conjunto optimizado de vallas adicionales que realizan el seguimiento de diferentes áreas de construcción dentro del área de construcción general de acuerdo con la programación. Se supone que la geovalla principal tiene un subsitio 1, con un tiempo de expiración establecido y expirará después de dicho período. Se pueden crear más geovallas anidadas según sus requisitos. Por ejemplo, en el subsitio 1 se podría realizar el trabajo durante la semana 1 a 4 de la programación y en el subsitio 2 el trabajo se lleva a cabo durante la semana 5 a 7. Se pueden cargar todas estas vallas como un único conjunto de datos al principio del proyecto y se pueden utilizar para realizar un seguimiento de las reglas basadas en el tiempo y el espacio. Para más información sobre el formato de datos de la geovalla, consulte [Geofence GeoJSON data](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) (Datos GeoJSON de geovalla). Para más información sobre cómo cargar datos en el servicio Azure Maps, consulte la [documentación de Data Upload API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview).

Abra la aplicación Postman y siga estos pasos para cargar la geovalla del sitio de construcción mediante Data Upload API de Azure Maps.

1. Abra la aplicación Postman, haga clic en New | Create new (Nuevo | Crear nuevo) y seleccione Request (Solicitud). Escriba un nombre de solicitud para la carga de datos de geovalla, seleccione una colección o carpeta donde guardarlo y haga clic en Save (Guardar).

    ![Carga de geovallas con Postman](./media/tutorial-geofence/postman-new.png)

2. Seleccione el método POST HTTP en la pestaña del generador e introduzca la siguiente URL para realizar una solicitud POST.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    El parámetro GEOJSON en la ruta de acceso de la dirección URL representa el formato de datos de los datos que se cargan.

3. Haga clic en **Params** (Parámetros) y escriba los siguientes pares de clave-valor que se usarán para la dirección URL de la solicitud POST. Reemplace el valor de subscription-key por la clave de suscripción principal de Azure Maps.
   
    ![Parámetros de clave-valor de Postman](./media/tutorial-geofence/postman-key-vals.png)

4. Haga clic en **Body** (Cuerpo), seleccione el formato de entrada sin procesar y elija JSON como formato de entrada en la lista desplegable. Proporcione el siguiente código JSON como datos que se van a cargar:

   ```JSON
   {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13393688201903,
                  47.63829579223815
                ],
                [
                  -122.13389128446579,
                  47.63782047131512
                ],
                [
                  -122.13240802288054,
                  47.63783312249837
                ],
                [
                  -122.13238388299942,
                  47.63829037035086
                ],
                [
                  -122.13393688201903,
                  47.63829579223815
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "1"
          }
        },
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13374376296996,
                  47.63784758098976
                ],
                [
                  -122.13277012109755,
                  47.63784577367854
                ],
                [
                  -122.13314831256866,
                  47.6382813338708
                ],
                [
                  -122.1334782242775,
                  47.63827591198201
                ],
                [
                  -122.13374376296996,
                  47.63784758098976
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "2",
            "validityTime": {
              "expiredTime": "2019-01-15T00:00:00",
              "validityPeriod": [
                {
                  "startTime": "2019-01-08T01:00:00",
                  "endTime": "2019-01-08T17:00:00",
                  "recurrenceType": "Daily",
                  "recurrenceFrequency": 1,
                  "businessDayOnly": true
                }
              ]
            }
          }
        }
      ]
   }
   ```

5. Haga clic en Send (Enviar) y revise el encabezado de la respuesta. El encabezado de ubicación contiene el identificador URI para acceder o descargar los datos para su uso futuro. También contiene un `udId` único para los datos cargados.

   ```HTTP
   https://atlas.microsoft.com/mapData/{udId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

## <a name="set-up-an-event-handler"></a>Configuración de un controlador de eventos

Para notificar a Operations Manager sobre los eventos de entrada y salida, hay que crear un controlador de eventos que reciba las notificaciones.

Se crearán dos servicios [Logic Apps](https://docs.microsoft.com/azure/event-grid/event-handlers#logic-apps) para controlar, introducir y generar eventos. También se crearán desencadenadores de eventos en las instancias de Logic Apps que se desencadenarán con estos eventos. La idea consiste en enviar alertas, en este caso correos electrónicos a Operations Manager cada vez que el equipo entra o sale del sitio. La siguiente ilustración muestra la creación de una aplicación lógica para el evento de entrada de la geovalla. De forma similar, puede crear otro para el evento de salida.
Para más información, puede ver todos los [controladores de eventos admitidos](https://docs.microsoft.com/azure/event-grid/event-handlers).

1. Creación de una aplicación lógica en Azure Portal

   ![Creación de instancias de Logic Apps](./media/tutorial-geofence/logic-app.png)

2. Seleccione un desencadenador de la solicitud HTTP y después seleccione "Send an email" (Enviar un correo electrónico) como acción en el conector de Outlook
  
   ![Esquema de Logic Apps](./media/tutorial-geofence/logic-app-schema.png)

3. Guarde la aplicación lógica para generar el punto de conexión de la dirección URL HTTP y copie dicha dirección.

   ![Punto de conexión de Logic Apps](./media/tutorial-geofence/logic-app-endpoint.png)


## <a name="create-an-azure-maps-events-subscription"></a>Creación de una suscripción a eventos de Azure Maps

Azure Maps admite tres tipos de eventos. Puede echar un vistazo a los tipos de eventos de Azure Maps admitidos [aquí](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps). Se van a crear dos suscripciones diferentes, una para los eventos de entrada y otra para los de salida.

Siga estos pasos para crear una suscripción de eventos para los eventos de entrada de la geovalla. Puede suscribirse a eventos de salida de la geovalla de forma similar.

1. Vaya a la cuenta de Azure Maps mediante [este vínculo del portal](https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/dashboard/) y seleccione la pestaña Events (Eventos).

   ![Eventos de Azure Maps](./media/tutorial-geofence/events-tab.png)

2. Para crear una suscripción de eventos, seleccione la suscripción de eventos desde la página correspondiente.

   ![Suscripción a eventos de Azure Maps](./media/tutorial-geofence/create-event-subscription.png)

3. Asigne un nombre a la suscripción de eventos y suscríbase al tipo de evento de entrada. Ahora, seleccione webhook como Tipo de punto de conexión y copie el punto de conexión de la dirección URL HTTP de la aplicación lógica en Endpoint (Punto de conexión).

   ![Suscripción de eventos](./media/tutorial-geofence/events-subscription.png)


## <a name="use-geofence-api"></a>Uso de Geofence API

Puede usar Geofence API para comprobar si un **dispositivo** (el equipo forma parte del estado) está dentro o fuera de una geovalla. Para comprender mejor Get Geofence API, se realiza la consulta en diferentes ubicaciones donde un equipo determinado se ha movido con el tiempo. La siguiente ilustración muestra cinco ubicaciones de un equipo de construcción en particular con un **id. de dispositivo** único como se observa en orden cronológico. Cada una de estas cinco ubicaciones se utiliza para evaluar el cambio de estado de entrada y salida de la geovalla frente a la misma. Si se produce un cambio de estado, el servicio de geovalla desencadena un evento, que envía Event Grid a la aplicación lógica. Como resultado, el administrador de la operación recibirá la correspondiente notificación de entrada o salida por correo electrónico.

> [!Note]
> El escenario y el comportamiento anteriores se basan en el mismo **id. de dispositivo**, de modo que refleja las cinco ubicaciones diferentes como en la siguiente ilustración.

![Mapa de geovalla](./media/tutorial-geofence/geofence.png)

En la aplicación Postman, abra una nueva pestaña en la misma colección que ha creado anteriormente. Seleccione el método GET HTTP en la pestaña del generador:

A continuación se presentan cinco solicitudes HTTP GET de Geofence API, con diferentes coordenadas de ubicación correspondientes del equipo según se observan en orden cronológico. Cada solicitud va seguida por el cuerpo de la respuesta.
 
1. Ubicación 1:
    
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
   ![Consulta de la geovalla 1](./media/tutorial-geofence/geofence-query1.png)

   Si se observa la respuesta anterior, la distancia negativa desde la geovalla principal significa que el equipo está dentro de la geovalla y la distancia positiva desde la geovalla del subsitio significa que está fuera de la geovalla del subsitio. 

2. Ubicación 2: 
   
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
    
   ![Consulta de la geovalla 2](./media/tutorial-geofence/geofence-query2.png)

   Si se observa cuidadosamente la respuesta anterior de JSON, el equipo está fuera del subsitio, pero dentro de la valla principal. No desencadena ningún evento y no se envía ningún correo electrónico.

3. Ubicación 3: 
  
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Consulta de la geovalla 3](./media/tutorial-geofence/geofence-query3.png)

   Se ha producido un cambio de estado y ahora el equipo se encuentra dentro de las geovallas principal y secundaria. Este proceso publica un evento y se enviará un correo electrónico de notificación a Operations Manager.

4. Ubicación 4: 

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
  
   ![Consulta de la geovalla 4](./media/tutorial-geofence/geofence-query4.png)

   Al observar atentamente la respuesta correspondiente, se puede observar que aquí no se publica ningún evento a pesar de que el equipo haya salido de la geovalla del subsitio. Si se observa la hora especificada por el usuario en la solicitud GET, se puede ver que la geovalla del subsitio ha expirado en relación a esta hora y que el equipo sigue en la geovalla principal. También puede ver el identificador de geometría de la geovalla del subsitio en `expiredGeofenceGeometryId` en el cuerpo de respuesta.


5. Ubicación 5:
      
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63799&lon=-122.134505&userTime=2019-01-16&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Consulta de la geovalla 5](./media/tutorial-geofence/geofence-query5.png)

   Puede ver que el equipo ha salido de la geovalla del sitio de construcción principal. Publica un evento, pero es una infracción grave y se envía un correo electrónico de alerta crítica a Operations Manager.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido cómo configurar una geovalla al cargarla en Azure Maps y un servicio de datos mediante Data Upload API. También ha aprendido a usar Event Grid de Azure Maps para suscribirse y controlar los eventos de la geovalla. 

* Consulte [Control de tipos de contenido en Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type), para obtener información sobre cómo usar Logic Apps para analizar JSON y crear una lógica más compleja.
* Para más información acerca de los controladores de eventos de Event Grid, consulte [Controladores de eventos de Azure Event Grid](https://docs.microsoft.com/azure/event-grid/event-handlers).
