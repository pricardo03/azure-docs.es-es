---
title: Integración de partners de sensores
description: Describe la integración de partners de sensores.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: e7de815b7254fb071b3094f9ae636b712b38684b
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797233"
---
# <a name="sensor-partner-integration"></a>Integración de partners de sensores
En este artículo se proporciona información sobre el componente **Traductor** de Azure FarmBeats.

Con este componente, los partners pueden desarrollar sensores que se integren con FarmBeats utilizando nuestra API y enviando telemetría y datos de dispositivos de clientes al centro de datos de FarmBeats. Los datos se visualizan mediante el acelerador de FarmBeats. Además, se pueden usar para fusionar datos y crear modelos de inteligencia artificial o de aprendizaje automático.

## <a name="link-farmbeats-account"></a>Vinculación de una cuenta de FarmBeats

Una vez que los clientes han adquirido e implementado dispositivos o sensores, pueden acceder a la telemetría y a los datos de los dispositivos en el portal de SaaS (software como servicio) de los partners de dispositivo. Los partners de dispositivos deben permitir que los clientes vinculen su cuenta a su instancia de FarmBeats en Azure. Las credenciales siguientes deben rellenarlas el cliente o el SI:

   - Nombre para mostrar (un campo opcional para que el usuario defina un nombre para esta integración)
   - Punto de conexión de API
   - Id. de inquilino
   - Id. de cliente
   - Secreto del cliente
   - Cadena de conexión de EventHub
   - Fecha de inicio

   > [!NOTE]
   > La fecha de inicio habilita la fuente de distribución de datos históricos, es decir, los datos a partir de la fecha especificada por el usuario.

## <a name="unlink-farmbeats"></a>Desvinculación de FarmBeats

Los clientes tienen la capacidad de desvincular una integración de FarmBeats. La desvinculación de FarmBeats no debería eliminar los metadatos de los dispositivos o sensores creados en el centro de datos del cliente. El proceso de desvinculación hace lo siguiente:

   - Detiene el flujo de telemetría.
   - Elimina y borra las credenciales de integración del partner de dispositivo.

## <a name="edit-farmbeats-integration"></a>Edición de la integración de FarmBeats

El cliente puede editar la integración de FarmBeats. El escenario principal de edición es cuando el secreto de cliente o la cadena de conexión cambian debido a la expiración; en este caso, el cliente solo puede editar los campos siguientes.

   - Nombre para mostrar (si procede)
   - Secreto de cliente (debe mostrarse con el formato "2x8***********" o la característica Mostrar u ocultar, en lugar de con texto no cifrado)
   - Cadena de conexión (debe mostrarse con el formato "2x8***********" o en la característica Mostrar u ocultar, en lugar de texto no cifrado)

   > [!NOTE]
   > La acción Editar no debe interrumpir la creación de objetos de metadatos.

## <a name="view-last-telemetry-sent"></a>Visualización de la última telemetría enviada

Puede ver la marca de tiempo de la última **telemetría enviada**. Esta es la hora a la que se envió correctamente la telemetría más reciente a FarmBeats.

## <a name="translator-development"></a>Desarrollo del componente Traductor

**Integración basada en la API REST**

Las funcionalidades de integración de datos de sensores de FarmBeats se exponen a través de la API REST. Algunas de las funcionalidades son definición de metadatos, aprovisionamiento de dispositivos o sensores, y administración de dispositivos y sensores.

**Ingesta de telemetría**

Los datos de telemetría se asignan a un mensaje canónico publicado en EventHub de Azure para su procesamiento. EventHub de Azure es un servicio que permite la ingesta de datos en tiempo real (telemetría) de dispositivos y aplicaciones conectados.

**Desarrollo de API**

Las API contienen documentación técnica de Swagger. Para obtener más información sobre las API y sus solicitudes y respuestas correspondientes, consulte [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

**Autenticación**

FarmBeats aprovecha la autenticación de Active Directory de Microsoft Azure. Azure App Service proporciona compatibilidad integrada con la autenticación y la autorización.

Para obtener más información, consulte [Azure Active Directory](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization).

El centro de datos de FarmBeats usa la autenticación de portador, que necesita las credenciales siguientes:
   - Id. de cliente
   - Secreto del cliente
   - Id. de inquilino

Con las credenciales anteriores, el autor de la llamada puede solicitar un token de acceso, que debe enviarse en las siguientes solicitudes de API en la sección de encabezado de la siguiente manera:

```
headers = {"Authorization": "Bearer " + access_token, …} 
```

A continuación, se muestra un ejemplo de código de Python que proporciona el token de acceso, que se puede usar para las posteriores llamadas API a FarmBeats: 

```python
import azure 

from azure.common.credentials import ServicePrincipalCredentials 
import adal 
#FarmBeats API Endpoint 
ENDPOINT = "https://<yourdatahub>.azurewebsites.net" [Azure website](https://<yourdatahub>.azurewebsites.net)
CLIENT_ID = "<Your Client ID>"   
CLIENT_SECRET = "<Your Client Secret>"   
TENANT_ID = "<Your Tenant ID>" 
AUTHORITY_HOST = 'https://login.microsoftonline.com' 
AUTHORITY = AUTHORITY_HOST + '/' + TENANT_ID 
#Authenticating with the credentials 
context = adal.AuthenticationContext(AUTHORITY) 
token_response = context.acquire_token_with_client_credentials(ENDPOINT, CLIENT_ID, CLIENT_SECRET) 
#Should get an access token here 
access_token = token_response.get('accessToken') 
```


**Encabezados de solicitud HTTP**

Estos son los encabezados de solicitud más comunes que deben especificarse al realizar una llamada API al centro de datos de FarmBeats:


**Encabezado** | **Descripción y ejemplo**
--- | ---
Content-Type | El formato de la solicitud (Content-Type: application/<format>). Para las API del centro de datos de FarmBeats, es JSON. Content-Type: application/json
Authorization | Especifica el token de acceso necesario para realizar una autorización de llamada API: Portador <token-Acceso>
Accept | El formato de respuesta. En el caso de las API del centro de datos de FarmBeats, el formato es JSON. Accept: Application/JSON

**Solicitudes de API**

Para realizar una solicitud de API REST (transferencia de estado representacional, por sus siglas en inglés), combine el método HTTP (GET, POST o PUT), la dirección URL al servicio de API, el URI (identificador uniforme de recursos) de un recurso para consultar, actualizar, eliminar o enviarle datos, y uno o varios encabezados de solicitud HTTP. La dirección URL del servicio de API es el punto de conexión de API proporcionado por el cliente. Por ejemplo: https://\<sucentrodedatos-sitioweb-nombre>.azurewebsites.net

Si lo desea, puede incluir parámetros de consulta en llamadas GET para filtrar, ordenar los datos en las respuestas y limitar su tamaño.

La siguiente solicitud de ejemplo se usa para obtener la lista de dispositivos:

```
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>”
```
La mayoría de las llamadas GET, POST y PUT requieren un cuerpo de solicitud JSON.

La siguiente solicitud de ejemplo se usa para crear un dispositivo (este ejemplo tiene una estructura de JSON de entrada con el cuerpo de la solicitud).

```
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  "accept: application/json" -H  "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  \"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

## <a name="data-format"></a>Formato de datos

JSON (notación de objetos JavaScript, por sus siglas en inglés) es un formato de datos común independiente del lenguaje que proporciona una representación de texto simple de estructuras de datos arbitrarias. Para obtener más información, visite [json.org](http://json.org).

## <a name="metadata-specifications"></a>Especificaciones de metadatos

El centro de datos de FarmBeats tiene las siguientes API que permiten a los partners de dispositivo crear y administrar los metadatos de dispositivos y sensores.  

- /**DeviceModel**: el modelo de dispositivo, que se corresponde con los metadatos del dispositivo, por ejemplo, el fabricante o el tipo de dispositivo (puerta de enlace o nodo).  
- /**Device**: el dispositivo, que se corresponde con un dispositivo físico presente en la granja.
- /**SensorModel**: el modelo de sensor, que se corresponde con los metadatos del sensor, por ejemplo, el fabricante, el tipo de sensor (analógico o digital) o la medida de sensor (como temperatura ambiente, presión, etc.).
- /**Sensor**: el sensor, que se corresponde con un sensor físico que registra valores. Un sensor normalmente se conecta a un dispositivo con un identificador de dispositivo.

  Modelo del dispositivo| DeviceModel se corresponde con los metadatos del dispositivo, por ejemplo, el fabricante o el tipo de dispositivo (puerta de enlace o nodo).
  --- | ---
  Type (Node, Gateway)  | 1 estrella |
  Fabricante  | 2 estrellas |
  ProductCode  | Código de producto del dispositivo o nombre o número de modelo. Por ejemplo, EnviroMonitor#6800. |
  Puertos  | Nombre y tipo de puerto (digital o analógico)  |
  NOMBRE  | Nombre para identificar el recurso. Por ejemplo, nombre del modelo o del producto. |
  DESCRIPCIÓN  | Proporciona una descripción significativa del modelo. |
  properties (Propiedades)  | Propiedades adicionales del fabricante. |
  **Dispositivo** | **Device se corresponde con un dispositivo físico presente en la granja. Cada dispositivo tiene un identificador de dispositivo único**. |
DeviceModelId  |Identificador del modelo de dispositivo asociado. |
HardwareId   |Identificador único del dispositivo, como la dirección MAC, etc.  |
reportingInterval |Intervalo de informes en segundos. |
Location    |Latitud (de-90 a +90), longitud (de -180 a 180) y elevación (en metros) del dispositivo. |
ParentDeviceId | Identificador del dispositivo primario al que está conectado este dispositivo. Por ejemplo: Un nodo conectado a una puerta de enlace; el valor parentDeviceID del nodo será la puerta de enlace. |
  NOMBRE  | Nombre para identificar el recurso.  Los partners de dispositivo deberán enviar un nombre que se corresponda con el del dispositivo en el lado del partner de dispositivo. Si el nombre del dispositivo está definido por el usuario en el lado del partner de dispositivo, ese mismo nombre definido por el usuario se debe propagar a FarmBeats.  |
  DESCRIPCIÓN  | Proporcione una descripción significativa.  |
  properties (Propiedades)  |Propiedades adicionales del fabricante.  |
  **Modelo del sensor** | SensorModel se corresponde con los metadatos del sensor, por ejemplo, el fabricante, el tipo de sensor (analógico o digital) o la medida de sensor (como temperatura ambiente, presión, etc.). |
  Type (analog, digital)  |Indica un tipo analógico o digital.|
  manufacturer  | Nombre del fabricante. |
  ProductCode  | Código de producto o nombre o número de modelo. Por ejemplo, RS-CO2-N01.  |
  SensorMeasures > Name  | Nombre de la medida del sensor. Solo se admiten minúsculas. Para medir desde diferentes profundidades, especifique la profundidad. Por ejemplo, soil_moisture_15cm. Este nombre tiene que corresponderse con los datos de telemetría. |
  sensorMeasures > DataType  | Tipo de datos de telemetría. Actualmente se admite Doble.  |
  sensorMeasures > Type  | Tipo de medida de los datos de telemetría del sensor. A continuación se indican los tipos definidos por el sistema: AmbientTemperature, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Phosphate, PointInTime, Potassium, Pressure, RainGauge, RelativeHumidity, Salinity, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun, WindSpeed, Evapotranspiration y PAR. Para agregar más, consulte la API /ExtendedType.
  sensorMeasures > Unit | Unidad de datos de telemetría del sensor. A continuación se indican las unidades definidas por el sistema: NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, MilliMeter, CentiMeter, Meter, Inch, Feet, Mile, KiloMeter, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Degree, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, PartsPerMillion, MicroMol, MicroMolesPerLiter, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, Liter, MilliLiter, Seconds, UnixTimestamp, MicroMolPerMeterSquaredPerSecond y InchesPerHour. Para agregar más, consulte la API /ExtendedType.
  SensorMeasures > aggregationType  | Puede ser none, average, maximum, minimum o StandardDeviation.
  SensorMeasures > depth  | La profundidad del sensor en centímetros (por ejemplo, medida de humedad de 10 cm por debajo del suelo)
  sensorMeasures > description  | Proporciona una descripción significativa de la medida.
  Nombre  | Nombre para identificar el recurso. Por ejemplo, nombre del modelo o del producto.
  description  | Proporciona una descripción significativa del modelo.
  properties  | Propiedades adicionales del fabricante.
  **Sensor**  |
  hardwareId  | Identificador único del sensor establecido por el fabricante.
  sensorModelId  | Identificador del modelo de sensor asociado.
  location  | Latitud (de -90 a +90), longitud (de -180 a 180) y elevación (en metros) del sensor.
  port > name  |Nombre y tipo de puerto al que está conectado el sensor en el dispositivo. Debe ser el mismo nombre que el definido en el modelo de dispositivo.
  deviceId  | Identificador del dispositivo al que está conectado el sensor.
  Nombre  | Nombre para identificar el recurso. Por ejemplo, el nombre del sensor o del producto, y el número de modelo o el código del producto.
  description  | Proporcione una descripción significativa.
  properties  | Propiedades adicionales del fabricante.

 Para obtener información sobre cada uno de los objetos y sus propiedades, consulte [Swagger](httpa://aka.ms/FarmBeatsDatahubSwagger).

 > [!NOTE]
 > Las API devuelven identificadores únicos para cada instancia creada. El componente Traductor debe conservar este identificador para la sincronización de los metadatos y la administración de dispositivos.


**Sincronización de metadatos**

El componente Traductor debe enviar actualizaciones de los metadatos. Por ejemplo, los escenarios de actualización son el cambio de nombre de dispositivo o sensor, o bien el cambio de ubicación de dispositivo o sensor.

El componente Traductor debe tener la capacidad de agregar nuevos dispositivos o sensores instalados por el usuario tras la vinculación de FarmBeats. Del mismo modo, si el usuario ha actualizado un dispositivo o sensor, debe actualizarse en FarmBeats para el dispositivo o sensor correspondiente. Los escenarios típicos de actualización de sensor o dispositivo podrían ser el cambio de ubicación del dispositivo, la adición de sensores en un nodo, etc.


> [!NOTE]
> No se pueden eliminar metadatos de dispositivo o sensor.
>
> Para actualizar los metadatos, es obligatorio llamar a /Get/{id} en el dispositivo o sensor, actualizar las propiedades cambiadas y, a continuación, realizar una llamada /Put/{id} para que no se pierdan las propiedades establecidas por el usuario.

### <a name="adding-new-typesunit"></a>Adición de nuevos tipos o unidades

FarmBeats permite agregar nuevas unidades y tipos de medidas de sensor. Para obtener más información sobre la API /ExtendedType, consulte [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

## <a name="telemetry-specifications"></a>Especificaciones de telemetría

Los datos de telemetría se asignan a un mensaje canónico publicado en EventHub de Azure para su procesamiento. EventHub de Azure es un servicio que permite la ingesta de datos en tiempo real (telemetría) de dispositivos y aplicaciones conectados.

## <a name="send-telemetry-data-to-farmbeats"></a>Envío de datos de telemetría a FarmBeats

Para enviar datos de telemetría a FarmBeats, deberá crear un cliente que envíe mensajes a un centro de eventos en FarmBeats. Si quiere obtener más información sobre los datos de telemetría, consulte [Envío de telemetría a un centro de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send).

Este es un ejemplo de código de Python que envía telemetría como un cliente a un centro de eventos especificado:

```python
import azure
from azure.eventhub import EventHubClient, Sender, EventData, Receiver, Offset
EVENTHUBCONNECTIONSTRING = "<EventHub Connection String provided by customer>"
EVENTHUBNAME = "<EventHub Name provided by customer>"

write_client = EventHubClient.from_connection_string(EVENTHUBCONNECTIONSTRING, eventhub=EVENTHUBNAME, debug=False)
sender = write_client.add_sender(partition="0")
write_client.run()
for i in range(5):
    telemetry = "<Canonical Telemetry message>"
    print("Sending telemetry: " + telemetry)
    sender.send(EventData(telemetry))
write_client.stop()

```

El formato de mensaje canónico es como el siguiente:

```
{
“deviceid”: “<id of the Device created>”,
 "timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>”
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": value
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": value
        }
      ]
    }
}

```

Todos los nombres de clave en la estructura de JSON de telemetría deben estar en minúsculas, por ejemplo, deviceid, sensordata, etc.

Mensaje de telemetría de ejemplo:


```
{
  "deviceid": "7f9b4b92-ba45-4a1d-a6ae-c6eda3a5bd12",
  "timestamp": "2019-06-22T06:55:02.7279559Z",
  "version" : "1",
  "sensors": [
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 15
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 42
        }
      ]
    },
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 20
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 89
        }
      ]
    }
  ]
}

```

## <a name="troubleshooterror-management"></a>Administración de errores y solución de problemas

**Soporte técnico y mecanismo para solución de problemas**

En caso de que el cliente no pueda recibir telemetría o datos de dispositivos en la instancia de FarmBeats especificada, el partner de dispositivo debe proporcionar soporte técnico y un mecanismo para solucionar el problema.

**Retención de datos de telemetría**

Los datos de telemetría también deben conservarse durante un período predefinido, de modo que este pueda ser útil para depurar o volver a enviar la telemetría en caso de error o pérdida de datos.

**Notificación de errores y su administración**

En caso de que se produzca un error que afecte al flujo de datos o a la integración de datos o metadatos de los dispositivos o sensores en el sistema del partner de dispositivo, se debe notificar al cliente. También se debe diseñar e implementar un mecanismo para resolver los errores.

**Lista de comprobación de conexión**

Los partners o fabricantes de dispositivos pueden tener la siguiente lista o prueba de comprobación para asegurarse de que las credenciales proporcionadas por el cliente son correctas.

   - Compruebe si se recibe un token de acceso con las credenciales proporcionadas.
   - Compruebe si una llamada API se realiza correctamente con el token de acceso recibido.
   - Compruebe si se ha establecido la conexión de cliente de EventHub.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la API REST, consulte [API REST](references-for-farmbeats.md#rest-api).
