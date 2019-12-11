---
title: Ingesta de datos de telemetría históricos
description: Se describe cómo ingerir datos de telemetría históricos.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 27aec53fd2e92e19f1c749e833217fb8b5deae57
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74672569"
---
# <a name="ingest-historical-telemetry-data"></a>Ingesta de datos de telemetría históricos

En este artículo se describe cómo ingerir datos de históricos del sensor en Azure FarmBeats.

Uno de los escenarios comunes de FarmBeats es la ingesta de datos históricos de Internet de las cosas (IoT) en recursos como dispositivos y sensores. Se crean metadatos para dispositivos y sensores y, luego, se ingieren los datos históricos en FarmBeats en formato canónico.

## <a name="before-you-begin"></a>Antes de empezar

Antes de continuar con este artículo, asegúrese de que ha instalado FarmBeats y ha recopilado datos históricos desde IoT.
También tendrá que habilitar el acceso de los asociados como se indica en los pasos siguientes.

## <a name="enable-partner-access"></a>Habilitación del acceso de asociados

Deberá habilitar la integración de asociados en la instancia de Azure FarmBeats. En este paso se crea un cliente que tendrá acceso a su instancia de Azure FarmBeats como asociado de dispositivo, y se proporcionan los siguientes valores que son necesarios en los pasos posteriores.

- Punto de conexión de API: esta es la dirección URL del centro de datos, por ejemplo, https://<datahub>.azurewebsites.net.
- Id. de inquilino
- Id. de cliente
- Secreto del cliente
- Cadena de conexión de EventHub

Siga los pasos que se indican a continuación para generarlos:

>[!NOTE]
> Para realizar los pasos siguientes, debe ser administrador.

1. Descargue este [script](https://aka.ms/farmbeatspartnerscript) y extráigalo en la unidad local. Encontrará dos archivos dentro del archivo ZIP.
2. Inicie sesión en [Azure Portal](https://portal.azure.com/) y abra Cloud Shell (esta opción está disponible en la barra superior derecha del portal).  

    ![Proyecto de FarmBeats](./media/for-tutorials/navigation-bar-1.png)

3. Asegúrese de que el entorno esté establecido en **PowerShell**.

    ![Proyecto de FarmBeats](./media/for-tutorials/power-shell-new-1.png)

4. Cargue los dos archivos que descargó (del paso 1 anterior) en Cloud Shell.  

    ![Proyecto de FarmBeats](./media/for-tutorials/power-shell-two-1.png)

5. Vaya al directorio donde se cargaron los archivos.

   >[!NOTE]
   > De forma predeterminada, el archivo se carga en el directorio particular /home/username/.
6. Ejecute el script mediante el comando:  

    ```azurepowershell-interactive
    ./generateCredentials.ps1
    ```

7. Siga las instrucciones que aparecen en pantalla para completar el procedimiento.

## <a name="create-devicesensor-metadata"></a>Creación de metadatos de dispositivo o sensor

 Ahora que tiene las credenciales necesarias, puede definir el dispositivo y los sensores mediante la creación de los metadatos con las API de FarmBeats.

 El centro de datos de FarmBeats tiene las siguientes API que permiten la creación y administración de los metadatos de dispositivos y sensores.   

- /**DeviceModel**: el modelo de dispositivo, que se corresponde con los metadatos del dispositivo, por ejemplo, el fabricante o el tipo de dispositivo (puerta de enlace o nodo).  
- /**Device**: el dispositivo, que se corresponde con un dispositivo físico presente en la granja.  
- /**SensorModel**: el modelo de sensor, que se corresponde con los metadatos del sensor, por ejemplo, el fabricante, el tipo de sensor (analógico o digital), la medida de sensor (como temperatura ambiente, presión, etc.).
- /**Sensor**: el sensor, que se corresponde con un sensor físico que registra valores. Un sensor normalmente se conecta a un dispositivo con un identificador de dispositivo.  


|        Modelo del dispositivo   |  Sugerencias   |
| ------- | -------             |
|     Type (Node, Gateway)        |          1 estrella      |
|          Fabricante            |         2 estrellas     |
|  ProductCode                    |  Código de producto del dispositivo o nombre o número de modelo. Por ejemplo, EnviroMonitor#6800.  |
|            Puertos          |     Nombre y tipo de puerto (digital o analógico)
|     NOMBRE                 |  Nombre para identificar el recurso. Por ejemplo, nombre del modelo o nombre del producto.
      DESCRIPCIÓN     | Proporciona una descripción significativa del modelo.
|    properties (Propiedades)          |    Propiedades adicionales del fabricante.   |
|    **Dispositivo**             |                      |
|   DeviceModelId     |     Identificador del modelo de dispositivo asociado.  |
|  HardwareId          | Identificador único del dispositivo, como la dirección MAC, etc.
|  ReportingInterval        |   Intervalo de informes en segundos.
|  Location            |  Latitud (de-90 a +90), longitud (de -180 a 180) y elevación (en metros) del dispositivo.   
|ParentDeviceId       |    Identificador del dispositivo primario al que está conectado este dispositivo. Por ejemplo, un nodo conectado a una puerta de enlace. Un nodo tendrá parentDeviceId como puerta de enlace.  |
|    NOMBRE            | Nombre para identificar el recurso. Los asociados de dispositivo deben enviar un nombre que sea coherente con el nombre del dispositivo del asociado. Si el nombre del dispositivo del asociado está definido por el usuario, este mismo nombre se debe propagar a FarmBeats.|
|     DESCRIPCIÓN       |      Proporcione una descripción significativa.  |
|     properties (Propiedades)    |  Propiedades adicionales del fabricante.
|     **Modelo del sensor**        |          |
|       Type (Analog, Digital)          |      Tipo de sensor, ya sea analógico o digital.       |
|          Fabricante            |       Fabricante del sensor.     |
|     ProductCode| Código de producto o nombre o número de modelo. Por ejemplo, RS-CO2-N01. |
|       SensorMeasures > Name       | Nombre de la medida del sensor. Solo se admiten minúsculas. Para medir desde diferentes profundidades, especifique la profundidad. Por ejemplo, soil_moisture_15cm. Este nombre debe ser coherente con los datos de telemetría.  |
|          SensorMeasures > DataType       |Tipo de datos de telemetría. Actualmente se admite doble.|
|    SensorMeasures > Type    |Tipo de medida de los datos de telemetría del sensor. A continuación se indican los tipos definidos por el sistema: AmbientTemperature, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Phosphate, PointInTime, Potassium, Pressure, RainGauge, RelativeHumidity, Salinity, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun, WindSpeed, Evapotranspiration y PAR. Para agregar más, consulte /ExtendedType API.|
|        SensorMeasures > Unit              | Unidad de datos de telemetría del sensor. A continuación se indican las unidades definidas por el sistema: NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, MilliMeter, CentiMeter, Meter, Inch, Feet, Mile, KiloMeter, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Degree, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, PartsPerMillion, MicroMol, MicroMolesPerLiter, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, Liter, MilliLiter, Seconds, UnixTimestamp, MicroMolPerMeterSquaredPerSecond, InchesPerHour Para agregar más, consulte /ExtendedType API.|
|    SensorMeasures > aggregationType    |  Los valores pueden ser ninguno, promedio, máximo, mínimo o StandardDeviation.  |
|          NOMBRE            | Nombre para identificar el recurso. Por ejemplo, nombre del modelo o nombre del producto.  |
|    DESCRIPCIÓN        | Proporciona una descripción significativa del modelo.  |
|   properties (Propiedades)       |  Propiedades adicionales del fabricante.  |
|    **Sensor**      |          |
| HardwareId          |   Identificador único del sensor establecido por el fabricante. |
|  SensorModelId     |    Identificador del modelo de sensor asociado.   |
| location          |  Latitud (de -90 a +90), longitud (de -180 a 180) y elevación (en metros) del sensor.|
|   Port > name        |  Nombre y tipo de puerto al que está conectado el sensor en el dispositivo. Debe ser el mismo nombre que el definido en el modelo de dispositivo. |
|    DeviceID  |    Identificador del dispositivo al que está conectado el sensor.     |
| NOMBRE            |   Nombre para identificar el recurso. Por ejemplo, el nombre del sensor o del producto, y el número de modelo o el código del producto.|
|    DESCRIPCIÓN      | Proporcione una descripción significativa. |
|    properties (Propiedades)        |Propiedades adicionales del fabricante. |

Para más información sobre los objetos, consulte [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

**Solicitud de API para crear metadatos**

Para realizar una solicitud de API, se combina el método HTTP (POST), la dirección URL al servicio de API y el URI a un recurso para consultar y enviar datos para crear o eliminar una solicitud y agregar uno o varios encabezados de solicitud HTTP. La dirección URL al servicio de API es el punto de conexión de API, es decir, la dirección URL del centro de datos (https://<yourdatahub>.azurewebsites.net)  

**Autenticación**:

el centro de datos de FarmBeats usa la autenticación del portador, que necesita las siguientes credenciales que se generaron en la sección anterior.

- Id. de cliente
- Secreto del cliente
- Id. de inquilino  

Con las credenciales anteriores, el autor de la llamada puede solicitar un token de acceso, que debe enviarse en las posteriores solicitudes de API en la sección de encabezado de la siguiente manera:

```
headers = *{"Authorization": "Bearer " + access_token, …}*
```

**Encabezados de solicitud HTTP**:

estos son los encabezados de solicitud más comunes que deben especificarse al realizar una llamada API al centro de datos de FarmBeats:

- Content-Type: application/json
- Autorización: Bearer <Access-Token>
- Accept: application/json

**Carga de entrada para crear metadatos**:

**DeviceModel**


```json
{
  "type": "Node",
  "manufacturer": "string",
  "productCode": "string",
  "ports": [
    {
      "name": "string",
      "type": "Analog"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

Dispositivo

```json
{
  "deviceModelId": "string",
  "hardwareId": "string",
  "farmId": "string",
  "reportingInterval": 0,
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "parentDeviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

SensorModel

```json
{
  "type": "Analog",
  "manufacturer": "string",
  "productCode": "string",
  "sensorMeasures": [
    {
      "name": "string",
      "dataType": "Double",
      "type": "string",
      "unit": "string",
      "aggregationType": "None",
      "depth": 0,
      "description": "string"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}

```
Sensor

```json
{
  "hardwareId": "string",
  "sensorModelId": "string",
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "depth": 0,
  "port": {
    "name": "string",
    "type": "Analog"
  },
  "deviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```
La siguiente solicitud de ejemplo es para crear un dispositivo (contiene código JSON de entrada como carga con el cuerpo de la solicitud).  

```bash
curl -X POST "https://<datahub>.azurewebsites.net/Device" -H  
"accept: application/json" -H  "Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  
\"reportingInterval\": 900,  \"name\": \"Device123\",  
\"description\": \"Test Device 123\"}" *
```

> [!NOTE]
> Las API devuelven identificadores únicos para cada instancia creada. Estos identificadores se deben conservar para enviar los mensajes de telemetría correspondientes.

**Envío de telemetría**

Ahora que ha creado los dispositivos y sensores en FarmBeats, puede enviar los mensajes de telemetría asociados.  

**Creación de reglas de telemetría**

La telemetría se debe enviar al centro de eventos de Azure, donde se procesará. Azure EventHub es un servicio que permite la ingesta de datos en tiempo real (telemetría) de aplicaciones y dispositivos conectados. Para enviar datos de telemetría a FarmBeats, debe crear un cliente que envíe mensajes a un centro de eventos en FarmBeats. Para más información sobre el envío de telemetría, consulte [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send).

**Envío de mensajes de telemetría como cliente**

Una vez que tenga establecida una conexión como cliente de EventHub, puede enviar mensajes a EventHub como JSON.  
Convierta el formato de datos históricos del sensor en un formato canónico que entienda Azure FarmBeats. El formato de mensaje canónico es así:  

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
        }
      ]
    }
 ]
}
```

Después de agregar los dispositivos y sensores correspondientes, obtenga los valores de devideid y sensorid del mensaje de telemetría, como se describe en la sección anterior.

Mensaje de telemetría de ejemplo:


 ```json
{
  "deviceid": "7f9b4b92-ba45-4a1d-a6ae-c6eda3a5bd12",
  "timestamp": "2019-06-22T06:55:02.7279559Z",
  "version": "1",
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


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la integración basada en la API REST, consulte [API REST](references-for-farmbeats.md#rest-api).
