---
title: Exportación de los datos a Azure IoT Central | Microsoft Docs
description: Exportación de datos desde la aplicación de Azure IoT Central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/3/2018
ms.topic: article
ms.prod: azure-iot-central
manager: peterpr
ms.openlocfilehash: 5b9564dfe40f292d289ee9ed680e816771d0b0ed
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282881"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportación de los datos a Azure IoT Central

Use la exportación de datos continua para exportar datos periódicamente a su cuenta de Azure Blob Storage. Elegir exportar **medidas**, **dispositivos** y **plantillas de dispositivo** en archivos con el formato [Apache AVRO](https://avro.apache.org/docs/current/index.html). Use los datos exportados para realizar análisis en frío de rutas de acceso, como modelos de entrenamiento en Azure Machine Learning o análisis de tendencias a largo plazo en Power BI.

> [!Note]
> Al activar la exportación de datos continua, solo obtendrá los datos que lleguen a partir de ese momento. Actualmente no hay ninguna manera de recuperar los datos a partir de la desactivación de la exportación de datos continua. ¡Activar continua de exportación de datos al principio para conservar los datos históricos más!

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de evaluación de 30 días extendida o una aplicación de pago
- Cuenta de Azure con la suscripción a Azure
- La misma cuenta de Azure es un administrador en la aplicación IoT Central
- La misma cuenta de Azure tiene permisos para crear una cuenta de almacenamiento o acceder a una cuenta de almacenamiento existente en la misma suscripción a Azure

## <a name="types-of-data-to-export"></a>Tipos de datos para exportar

### <a name="measurements"></a>Medidas

Las mediciones que los dispositivos envían se exportan a su cuenta de almacenamiento. Las mediciones se exportan aproximadamente una vez por minuto y contienen todos los mensajes nuevos que IoT Central recibió de todos los dispositivos dentro de esa ventana de tiempo. Los archivos AVRO exportados están en el mismo formato que los archivos de mensajes exportados por el [enrutamiento de mensajes de IoT Hub ](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) al almacenamiento de blobs.

> [!NOTE]
> Los dispositivos que envían las mediciones se representan mediante identificadores de dispositivo (ver más adelante). Para obtener los nombres de los dispositivos, debe exportar también las instantáneas de los dispositivos. Puede poner en correlación cada registro de mensaje mediante connectionDeviceId, que coincide con el identificador del dispositivo.

Este es un ejemplo de un registro en el archivo AVRO descodificado.

```json
{
    "EnqueuedTimeUtc": "2018-06-11T00:00:08.2250000Z",
    "Properties": {},
    "SystemProperties": {
        "connectionDeviceId": "2383d8ba-c98c-403a-b4d5-8963859643bb",
        "connectionAuthMethod": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
        "connectionDeviceGenerationId": "636614021491644195",
        "enqueuedTime": "2018-06-11T00:00:08.2250000Z"
    },
    "Body": "{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Dispositivos

Cuando activa por primera vez la exportación de datos continua, se exporta una sola instantánea que contiene todos los dispositivos. Esto incluye:
- Identificadores de dispositivo
- Nombres de dispositivo
- Identificadores de plantilla de dispositivo
- Valores de propiedades
- Valores de configuración

Aproximadamente una vez por minuto, se escribe una nueva instantánea que contiene:

- Los nuevos dispositivos que se agregaron desde la última instantánea
- Dispositivos que tenían propiedades y valores configuración cambiados desde la última instantánea

> [!NOTE]
> Los dispositivos que se eliminaron desde la última instantánea no se exportan. No hay ningún indicador en las instantáneas para los dispositivos que se eliminaron en este momento.

> [!NOTE]
> La plantilla de dispositivo a la que pertenece cada dispositivo se representa mediante un identificador de plantilla de dispositivo. Para obtener el nombre de la plantilla de dispositivo, también debe exportar instantáneas de plantilla de dispositivo.

Cada registro del archivo AVRO descodificado tiene este aspecto:

```json
{
    "id": "2383d8ba-c98c-403a-b4d5-8963859643bb",
    "name": "Refrigerator 2",
    "simulated": true,
    "deviceTemplate": {
        "id": "c318d580-39fc-4aca-b995-843719821049",
        "version": "1.0.0"
    },
    "properties": {
        "cloud": {
            "location": "New York",
            "maintCon": true,
            "tempThresh": 20
        },
        "device": {
            "lastReboot": "2018-02-09T22:22:47.156Z"
        }
    },
    "settings": {
        "device": {
            "fanSpeed": 0
        }
    }
}
```

### <a name="device-templates"></a>Plantillas de dispositivo

Cuando activa por primera vez la exportación de datos continua, se exporta una sola instantánea que contiene todas las plantillas de dispositivo. Esto incluye: 
- Identificadores de plantilla de dispositivo
- Tipos de datos de medición y valores mínimos y máximos
- Tipos de datos de propiedades y valores predeterminados
- Tipos de datos de configuración y valores predeterminados

Aproximadamente una vez por minuto, se escribe una nueva instantánea que contiene:

- Las nuevas plantillas de dispositivo que se agregaron desde la última instantánea
- Plantillas de dispositivo que tenían mediciones, propiedades y definiciones de configuración que cambiaron desde la última instantánea

> [!NOTE]
> Las plantillas de dispositivo que se eliminaron desde la última instantánea no se exportan. No hay ningún indicador en las instantáneas para las plantillas de dispositivo que se eliminaron en este momento.

Cada registro del archivo AVRO descodificado tiene este aspecto:

```json
{
    "id": "c318d580-39fc-4aca-b995-843719821049",
    "name": "Refrigerated Vending Machine",
    "version": "1.0.0",
    "measurements": {
        "telemetry": {
            "humidity": {
                "dataType": "double",
                "name": "Humidity"
            },
            "magnetometerX": {
                "dataType": "double",
                "name": "Magnetometer X"
            },
            "magnetometerY": {
                "dataType": "double",
                "name": "Magnetometer Y"
            },
            "magnetometerZ": {
                "dataType": "double",
                "name": "Magnetometer Z"
            }
        },
        "states": {
            "connectivity": {
                "dataType": "enum",
                "name": "Connectivity"
            }
        },
        "events": {
            "opened": {
                "name": "Door Opened",
                "category": "informational"
            }
        }
    },
    "settings": {
        "device": {
            "fanSpeed": {
                "dataType": "double",
                "name": "Fan Speed",
                "initialValue": 0
            }
        }
    },
    "properties": {
        "cloud": {
            "location": {
                "dataType": "string",
                "name": "Location",
                "initialValue": "Seattle"
            },
            "maintCon": {
                "dataType": "boolean",
                "name": "Maintenance Contract",
                "initialValue": true
            },
            "tempThresh": {
                "dataType": "double",
                "name": "Temperature Alert Threshold",
                "initialValue": 30
            }
        },
        "device": {
            "lastReboot": {
                "dataType": "dateTime",
                "name": "Last Reboot"
            }
        }
    }
}
```

## <a name="how-to-set-up-data-export"></a>Configuración de la exportación de datos

1. Si aún no tiene una, cree una cuenta de Azure Storage **en la suscripción a Azure en la que se encuentra la aplicación**. [Haga clic aquí](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) para saltar a Azure Portal para crear una nueva cuenta de Azure Storage.

    - Elija los tipos de cuenta *Uso general* o *Blob Storage*.
    - Seleccione la suscripción en la que se encuentra la aplicación IoT Central. Si no ve la suscripción, es posible que deba iniciar sesión en una cuenta de Azure diferente o solicitar acceso a la suscripción.
    - Puede elegir un grupo de recursos existente o crear uno. Obtenga más información sobre la [creación de una nueva cuenta de almacenamiento](https://aka.ms/blobdocscreatestorageaccount).

2. Cree un contenedor en su cuenta de almacenamiento en la que exportar los datos de IoT Central. Vaya a Cuenta de almacenamiento -> Examinar blobs y cree un nuevo contenedor. ![Creación de una imagen de contenedor](media/howto-export-data/createcontainer.png)

3. Inicie sesión en su aplicación de IoT Central con la misma cuenta de Azure.
1. Vaya a Administración -> Exportar datos.
![CDE de IoT Central](media/howto-export-data/continuousdataexport.PNG)
1. Mediante las listas desplegables, seleccione la cuenta de almacenamiento y el contenedor. Después, use los conmutadores para activar o desactivar los diferentes tipos de datos que desea exportar.
1. Finalmente, active la exportación de datos continua con el conmutador y presione "Guardar".
1. Espere unos minutos y debería ver que los datos aparecen en la cuenta de almacenamiento. Puede ir a la cuenta de almacenamiento, seleccionar Examinar blobs, seleccionar el contenedor y verá tres carpetas. Las rutas de acceso predeterminadas para los archivos AVRO que contiene los distintos tipos de datos son:
- Mensajes: **{contenedor}/measurements/{nombreDeHub}/{AAAA}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Dispositivos: **{contenedor}/devices/{hubname}/{AAAA}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Plantillas de dispositivo: **{contenedor}/deviceTemplates/{hubname}/{AAAA}/{MM}/{dd}/{hh}/{mm}/00.avro**

## <a name="how-to-read-exported-avro-files"></a>Lectura de archivos AVRO exportados

AVRO es un formato binario, por lo que los archivos no se pueden leer en su estado sin procesar. Se pueden descodificar al formato JSON. Los ejemplos siguientes muestran cómo analizar los archivos AVRO de mediciones, dispositivos y plantillas de dispositivo mediante los ejemplos anteriores.

## <a name="python"></a>Python

### <a name="install-pandas-and-the-pandaavro-package"></a>Instalación del paquete de Pandas y PandaAvro:

```python
pip install pandas
pip install pandavro
```
### <a name="parse-measurements-avro-file"></a>Análisis del archivo AVRO de mediciones

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    measurements = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary with the device id
    # located under the "connectionDeviceId" key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of utf-8 bytes that is stringified and parsed
    # as json. In this example, we pull the "humidity" property off of each one
    # to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, we print the new DataFrame with our device ids and humidities
    print(transformed)

```
### <a name="parse-devices-avro-file"></a>Análisis del archivo AVRO de dispositivos

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    devices = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device id is available directly in the "id" column
    transformed["device_id"] = devices["id"]

    # The template id and version are present in a dictionary under the
    # deviceTemplate column
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)

```

### <a name="parse-device-templates-avro-file"></a>Análisis del archivo AVRO de plantillas de dispositivo

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    templates = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available directly in the "id" and "version"
    # columns, respectively
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)
```

## <a name="c"></a>C#

### <a name="install-microsofthadoopavro"></a>Instalación de Microsoft.Hadoop.Avro

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

### <a name="parse-measurements-avro-file"></a>Análisis del archivo AVRO de mediciones

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;
using Newtonsoft.Json;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contain multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the fields
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    var systemProperties = record.GetField<IDictionary<string, object>>("SystemProperties");
                    var deviceId = systemProperties["connectionDeviceId"] as string;
                    Console.WriteLine("Device ID: {0}", deviceId);

                    using (var stream = new MemoryStream(record.GetField<byte[]>("Body")))
                    {
                        using (var streamReader = new StreamReader(stream, Encoding.UTF8))
                        {
                            var body = JsonSerializer.Create().Deserialize(streamReader, typeof(IDictionary<string, dynamic>)) as IDictionary<string, dynamic>;
                            var humidity = body["humidity"];
                            Console.WriteLine("Humidity: {0}", humidity);
                        }
                    }
                }
            }
        }
    }
}
```

### <a name="parse-devices-avro-file"></a>Análisis del archivo AVRO de dispositivos

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var deviceId = record.GetField<string>("id");

                    // The device template information is stored in a sub-record
                    // under the "deviceTemplate" field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {0}, Template ID: {1}, Template Version: {2}, Fan Speed: {3}",
                        deviceId,
                        templateId,
                        templateVersion,
                        fanSpeed
                    );
                }
            }
        }
    }
}

```
### <a name="parse-device-templates-avro-file"></a>Análisis del archivo AVRO de plantillas de dispositivo

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {1}, Version: {2}, Fan Speed: {3}",
                        id,
                        version,
                        fanSpeed
                    );
                }
            }
        }
    }
}
```

## <a name="javascript"></a>Javascript

### <a name="install-avsc"></a>Instalación de avsc

```javascript
npm install avsc
```

### <a name="parse-measurements-avro-file"></a>Análisis del archivo AVRO de mediciones

```javascript
const avro = require('avsc');

// Read the avro file and parse the device id and humidity from each record
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the system properties
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the Body from a Buffer to a string and parse it
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property off of the body
        const humidity = body.humidity;

        // Log the device id and humidity we found
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-devices-avro-file"></a>Análisis del archivo AVRO de dispositivos

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the id property
        const deviceId = record.id;

        // Fetch the tempalte id and version from the deviceTemplate property
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`ID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-device-templates-avro-file"></a>Análisis del archivo AVRO de plantillas de dispositivo

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template id and version from the id and verison properties
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a exportar los datos, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Procedimiento para visualizar los datos en Power BI](howto-connect-powerbi.md)