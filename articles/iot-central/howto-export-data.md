---
title: Exportación de los datos a Azure IoT Central | Microsoft Docs
description: Exportación de datos desde la aplicación de Azure IoT Central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 86128abd82ee41459a84fc7d9169042179807793
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034916"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportación de los datos a Azure IoT Central

*Este tema se aplica a los administradores.*

En este artículo se describe cómo usar la característica de exportación de datos continuos en Azure IoT Central para exportar periódicamente datos a la cuenta de Azure Blob Storage. Puede exportar **medidas**, **dispositivos** y **plantillas de dispositivo** a archivos con el formato [Apache AVRO](https://avro.apache.org/docs/current/index.html). Los datos exportados se pueden utilizar para realizar análisis en frío de rutas de acceso, como modelos de entrenamiento en Azure Machine Learning o análisis de tendencias a largo plazo en Microsoft Power BI.

> [!Note]
> Al activar la exportación de datos continua, solo obtendrá los datos a partir de ese momento. Actualmente, los datos no se pueden recuperar durante un tiempo cuando la exportación de datos continua estaba desactivada. Para conservar más datos históricos, activa la exportación continua de datos al principio.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de prueba de IoT Central extendida durante 30 días, o una aplicación de pago.
- Una cuenta de Azure con una suscripción de Azure.
- La misma cuenta de Azure es un administrador en la aplicación IoT Central.
- La misma cuenta de Azure tiene permisos para crear una cuenta de almacenamiento o acceder a una cuenta de almacenamiento existente en la misma suscripción a Azure.

## <a name="types-of-data-to-export"></a>Tipos de datos para exportar

### <a name="measurements"></a>Medidas

Las mediciones que los dispositivos envían se exportan a su cuenta de almacenamiento una vez por minuto. Los datos contienen todos los mensajes nuevos recibidos por IoT Central de todos los dispositivos durante ese tiempo. Los archivos AVRO exportados usan el mismo formato que los archivos de mensajes exportados por el [enrutamiento de mensajes de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) al almacenamiento de blobs.

> [!NOTE]
> Los dispositivos que envían las mediciones se representan mediante identificadores de dispositivo (consulte las secciones siguientes). Para obtener los nombres de los dispositivos, exporte las instantáneas del dispositivo. Correlacione cada registro de mensaje mediante el **connectionDeviceId** que coincide con el **deviceId** del registro del dispositivo.

En el ejemplo siguiente se muestra un registro en un archivo AVRO descodificado:

```json
{
    "EnqueuedTimeUtc": "2018-06-11T00:00:08.2250000Z",
    "Properties": {},
    "SystemProperties": {
        "connectionDeviceId": "<connectionDeviceId>",
        "connectionAuthMethod": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
        "connectionDeviceGenerationId": "<generationId>",
        "enqueuedTime": "2018-06-11T00:00:08.2250000Z"
    },
    "Body": "{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Dispositivos

Cuando se activa por primera vez la exportación de datos continua, se exporta una sola instantánea de todos los dispositivos. Cada dispositivo incluye:
- `id` del dispositivo en IoT Central
- `name` del dispositivo
- `deviceId` del [Device Provisioning Service](https://aka.ms/iotcentraldocsdps)
- Información de la plantilla de dispositivo
- Valores de propiedad
- Valores de configuración

Una nueva instantánea se escribe una vez por minuto. La instantánea incluye:

- Los nuevos dispositivos que se agregaron desde la última instantánea.
- Dispositivos con propiedades y valores de configuración modificados desde la última instantánea.

> [!NOTE]
> Los dispositivos que se eliminaron desde la última instantánea no se exportan. Actualmente, las instantáneas no tienen indicadores para los dispositivos eliminados.
>
> La plantilla de dispositivo a la que pertenece cada dispositivo se representa mediante un identificador de plantilla de dispositivo. Para obtener el nombre de la plantilla de dispositivo, exporte las instantáneas de plantilla de dispositivo.

Un registro del archivo AVRO descodificado puede tener este aspecto:

```json
{
    "id": "<id>",
    "name": "Refrigerator 2",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
        "id": "<template id>",
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

Cuando se activa por primera vez la exportación de datos continua, se exporta una sola instantánea de todas las plantillas de dispositivo. Cada plantilla de dispositivo incluye:
- `id` de la plantilla de dispositivo
- `name` de la plantilla de dispositivo
- `version` de la plantilla de dispositivo
- Tipos de datos de medición y valores mínimos y máximos
- Tipos de datos de propiedad y valores predeterminados
- Tipos de datos de configuración y valores predeterminados

Una nueva instantánea se escribe una vez por minuto. La instantánea incluye:

- Nuevas plantillas de dispositivo que se agregaron desde la última instantánea
- Plantillas de dispositivo con mediciones, propiedades y definiciones de configuración que cambiaron desde la última instantánea

> [!NOTE]
> Las plantillas de dispositivo que se eliminaron desde la última instantánea no se exportan. Actualmente, las instantáneas no tienen indicadores para las plantillas de dispositivos eliminadas.

Un registro del archivo AVRO descodificado puede tener este aspecto:

```json
{
    "id": "<id>",
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

## <a name="set-up-continuous-data-export"></a>Configuración de la exportación continua de datos

1. Si no dispone de una cuenta de almacenamiento de Azure, [cree una](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) en Azure Portal. Cree la cuenta de almacenamiento **en la suscripción a Azure que tenga la aplicación IoT Central**.
    - Para el tipo de cuenta, elija **Uso general** o **Blob Storage**.
    - Seleccione la suscripción que tiene la aplicación IoT Central. Si no ve la suscripción, es posible que deba iniciar sesión en una cuenta de Azure diferente o solicitar acceso a la suscripción.
    - Elija un grupo de recursos existente o cree uno. Obtenga más información sobre la [creación de una nueva cuenta de almacenamiento](https://aka.ms/blobdocscreatestorageaccount).

2. Cree un contenedor en su cuenta de almacenamiento para exportar los datos de IoT Central. Vaya a la cuenta de almacenamiento. En **Blob Service** seleccione **Examinar blobs**. Seleccione **Contenedor** para crear un nuevo contenedor.

   ![Crear un contenedor](media/howto-export-data/createcontainer.png)

3. Inicie sesión en su aplicación de IoT Central con la misma cuenta de Azure.

4. En **Administración**, seleccione **Exportación de datos**.

5. En el cuadro de lista desplegable **Cuenta de almacenamiento**, seleccione una cuenta. En la lista desplegable **Contenedor**, seleccione el contenedor. En **Datos para exportar**, especifique cada tipo de datos para exportar y establezca el tipo en **Activado**.

6. Para activar la exportación continua de datos, establezca **Exportación de datos** en **Activado**. Seleccione **Guardar**.

  ![Configuración de la exportación continua de datos](media/howto-export-data/continuousdataexport.PNG)

7. Después de unos minutos, los datos aparecerán en la cuenta de almacenamiento. Vaya a la cuenta de almacenamiento. Seleccione **Examinar blobs** > el contenedor. Verá tres carpetas para los datos de exportación. Las rutas de acceso predeterminadas para los archivos AVRO con los datos de exportación son las siguientes:
    - Mensajes: {container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro
    - Dispositivos: {container}/devices/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro
    - Plantillas de dispositivo: {container}/deviceTemplates/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro

## <a name="read-exported-avro-files"></a>Lectura de archivos AVRO exportados

AVRO es un formato binario, por lo que los archivos no se pueden leer en su estado sin procesar. Los archivos se pueden descodificar al formato JSON. Los ejemplos siguientes muestran cómo analizar los archivos AVRO de mediciones, dispositivos y plantillas de dispositivo. Los ejemplos corresponden a los ejemplos descritos en la sección anterior.

### <a name="read-avro-files-by-using-python"></a>Lectura de los archivos de AVRO mediante Python

#### <a name="install-pandas-and-the-pandavro-package"></a>Instalación del paquete de Pandas y PandaAvro

```python
pip install pandas
pip install pandavro
```

#### <a name="parse-a-measurements-avro-file"></a>Análisis de un archivo AVRO de mediciones

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    measurements = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary
    # with the device ID located under the connectionDeviceId key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of UTF-8 bytes that is stringified
    # and parsed as JSON. This example pulls the humidity property
    # from each column to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, print the new DataFrame with our device IDs and humidities.
    print(transformed)

```

#### <a name="parse-a-devices-avro-file"></a>Análisis de un archivo AVRO de dispositivos

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    devices = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device ID is available in the id column.
    transformed["device_id"] = devices["deviceId"]

    # The template ID and version are present in a dictionary under
    # the deviceTemplate column.
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)

```

#### <a name="parse-a-device-templates-avro-file"></a>Análisis de un archivo AVRO de plantillas de dispositivo

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    templates = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available in the id and version columns.
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)
```

### <a name="read-avro-files-by-using-c"></a>Lectura de archivos AVRO con C#

#### <a name="install-the-microsofthadoopavro-package"></a>Instalación del paquete Microsoft.Hadoop.Avro

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

#### <a name="parse-a-measurements-avro-file"></a>Análisis de un archivo AVRO de mediciones

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
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
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

#### <a name="parse-a-devices-avro-file"></a>Análisis de un archivo AVRO de dispositivos

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var deviceId = record.GetField<string>("deviceId");

                    // The device template information is stored in a sub-record
                    // under the deviceTemplate field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep.
                    // The first level indicates settings or properties.
                    // The second level indicates the type of setting or property.
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "Device ID: {0}, Template ID: {1}, Template Version: {2}, Fan Speed: {3}",
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

#### <a name="parse-a-device-templates-avro-file"></a>Análisis de un archivo AVRO de plantillas de dispositivo

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep.
                    // The first level indicates settings or properties.
                    // The second level indicates the type of setting or property.
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

### <a name="read-avro-files-by-using-javascript"></a>Lectura de archivos AVRO con Javascript

#### <a name="install-the-avsc-package"></a>Instalación del paquete avsc

```javascript
npm install avsc
```

#### <a name="parse-a-measurements-avro-file"></a>Análisis de un archivo AVRO de mediciones

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device ID and humidity from each record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the system properties.
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the body from a buffer to a string and parse it.
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property from the body.
        const humidity = body.humidity;

        // Log the retrieved device ID and humidity.
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

#### <a name="parse-a-devices-avro-file"></a>Análisis de un archivo AVRO de dispositivos

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the deviceId property.
        const deviceId = record.deviceId;

        // Fetch the template ID and version from the deviceTemplate property.
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device ID and humidity.
        console.log(`deviceID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

#### <a name="parse-a-device-templates-avro-file"></a>Análisis de un archivo AVRO de plantillas de dispositivo

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template ID and version from the id and verison properties.
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device id and humidity.
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo exportar los datos, continúe con el paso siguiente:

> [!div class="nextstepaction"]
> [Procedimiento para visualizar los datos en Power BI](howto-connect-powerbi.md)
