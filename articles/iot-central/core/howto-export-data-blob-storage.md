---
title: Exportación de datos a Azure Blob Storage | Microsoft Docs
description: Exportación de datos desde la aplicación de Azure IoT Central a Azure Blob Storage
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 09/26/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: 79d578c910c7d08355901308e00db5912d1f3baf
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721496"
---
# <a name="export-your-data-to-azure-blob-storage"></a>Exportación de datos a Azure Blob Storage

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

*Este tema se aplica a administradores*.

En este artículo se describe cómo usar la función de exportación continua de datos en Azure IoT Central para exportar periódicamente datos a su **cuenta de almacenamiento de Azure Blob** o a la **cuenta de almacenamiento de Azure Data Lake Storage Gen2** . Puede exportar **medidas**, **dispositivos** y **plantillas de dispositivo** a archivos con el formato JSON o Apache Avro. Los datos exportados se pueden utilizar para realizar análisis en frío de rutas de acceso, como modelos de entrenamiento en Azure Machine Learning o análisis de tendencias a largo plazo en Microsoft Power BI.

> [!Note]
> Al activar la exportación de datos continua, solo obtendrá los datos a partir de ese momento. Actualmente, los datos no se pueden recuperar durante un tiempo cuando la exportación de datos continua estaba desactivada. Para conservar más datos históricos, activa la exportación continua de datos al principio.


## <a name="prerequisites"></a>Prerequisites

- Ser administrador en la aplicación de IoT Central


## <a name="set-up-export-destination"></a>Configuración del destino de exportación

Si no tiene una instancia existente de Storage a la que exportar, siga estos pasos:

1. Cree una [cuenta de almacenamiento en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Puede obtener más información sobre cómo crear nuevas [cuentas de almacenamiento de Azure Blob ](https://aka.ms/blobdocscreatestorageaccount) o [cuentas de almacenamiento de Azure Data Lake Storage v2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-account).

    > [!Note] 
    > Si decide exportar datos a una cuenta de almacenamiento de ADLS v2, debe elegir **Tipo de cuenta** como **BlobStorage**. 

    > [!Note] 
    > Puede exportar datos a cuentas de almacenamiento en suscripciones que no sean la de su aplicación de IoT Central de pago por uso. En este caso se conectará mediante una cadena de conexión.

2. Cree un contenedor en la cuenta de almacenamiento. Vaya a la cuenta de almacenamiento. En **Blob Service** seleccione **Examinar blobs**. Seleccione **+ Contenedor** en la parte superior para crear un contenedor.

## <a name="set-up-continuous-data-export"></a>Configuración de la exportación continua de datos

Ahora que tiene un destino de Storage al que exportar los datos, siga estos pasos para configurar la exportación de datos continua. 

1. Inicie sesión en su aplicación de IoT Central.

2. En el panel izquierdo, seleccione **Exportación de datos**.

    > [!Note]
    > Si no ve Exportación de datos en el panel izquierdo, significa que no es un administrador de la aplicación. Hable con un administrador para configurar la exportación de datos.

3. Seleccione el botón **+ Nuevo** en la esquina superior derecha. Elija **Azure Blob Storage** como destino de la exportación. 

    > [!NOTE] 
    > El número máximo de exportaciones por aplicación es cinco. 

    ![Creación de una exportación de datos continua](media/howto-export-data/export-new2.png)

4. En el cuadro de lista desplegable, seleccione su **espacio de nombres de Storage**. También puede elegir la última opción de la lista que es **Escriba una cadena de conexión**. 

    > [!NOTE] 
    > Solo verá los espacios de nombres de cuentas de Storage, en la **misma suscripción que la de la aplicación de IoT Central**. Si quiere exportar a un destino fuera de esta suscripción, elija **Escriba una cadena de conexión** y vea el paso 5.

    > [!NOTE] 
    > En las aplicaciones de prueba de siete días, la única manera de configurar la exportación de datos continua es mediante una cadena de conexión. El motivo es que estas aplicaciones no tienen una suscripción de Azure asociada.

    ![Crear nueva exportación a Blob](media/howto-export-data/export-create-blob2.png)

5. (Opcional) Si eligió **Escriba una cadena de conexión**, aparece un nuevo cuadro para que pegue la cadena de conexión. Para obtener la cadena de conexión de su cuenta de almacenamiento, vaya a la cuenta de almacenamiento en Azure Portal: en la **configuración**, seleccione **Claves de acceso** y copie la cadena de conexión "key1" o "key2".
 
6. Elija un contenedor en el cuadro de lista desplegable. Si no tiene un contenedor, vaya a la cuenta de almacenamiento en Azure Portal:
    - En el **Blob service**, seleccione **Blobs**. Haga clic en **+ Contenedor** y asigne un nombre a su contenedor. Elija un nivel de acceso público para sus datos (cualquiera funcionará con la exportación continua de datos). Puede encontrar más información en los [documentos de Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container).

7. Elija el **formato de datos** que prefiera: puede ser JSON o [Apache Avro](https://avro.apache.org/docs/current/index.html).

8. En **Datos para exportar**, especifique cada tipo de datos para exportar y establezca el tipo en **Activado**.

9. Para activar la exportación continua de datos, asegúrese de que la tecla de alternancia **Exportación de datos** esté **activada**. Seleccione **Guardar**.

   ![Configuración de la exportación continua de datos](media/howto-export-data/export-list-blob2.png)

10. Después de unos minutos, los datos aparecerán en la cuenta de almacenamiento.


## <a name="path-structure"></a>Estructura de la ruta de acceso

Los datos de medidas, dispositivos y plantillas de dispositivo se exportan a la cuenta de almacenamiento una vez por minuto, y cada archivo contiene el lote de cambios desde el último archivo exportado. Los datos exportados se colocan en tres carpetas con formato JSON o Avro. Las rutas de acceso predeterminadas de la cuenta de almacenamiento son:
- Mensajes: {container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}
- Dispositivos: {container}/devices/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}
- Plantillas de dispositivo: {container}/deviceTemplates/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}

Puede examinar los archivos exportados en Azure Portal si va al archivo y elige la pestaña para **editar el blob**.

## <a name="data-format"></a>Formato de datos 

### <a name="measurements"></a>Medidas

Los datos de medidas exportados contienen todos los mensajes nuevos recibidos por IoT Central de todos los dispositivos durante ese tiempo. Los archivos exportados usan el mismo formato que los archivos de mensajes exportados por el [enrutamiento de mensajes de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) a Blob Storage.

> [!NOTE]
> Asegúrese de que sus dispositivos estén enviando mensajes que tengan `contentType: application/JSON` y `contentEncoding:utf-8` (o `utf-16`, `utf-32`). Consulte la [documentación de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-body) para ver un ejemplo.

> [!NOTE]
> Los dispositivos que envían las mediciones se representan mediante identificadores de dispositivo (consulte las secciones siguientes). Para obtener los nombres de los dispositivos, exporte las instantáneas del dispositivo. Correlacione cada registro de mensaje mediante el **connectionDeviceId** que coincide con el **deviceId** del registro del dispositivo.

En el ejemplo siguiente se muestra un registro en un archivo Avro descodificado:

```json
{ 
  "EnqueuedTimeUtc":"2019-06-11T00:00:08.2250000Z",
  "Properties":{},
  "SystemProperties":{ 
    "connectionDeviceId":"<deviceId>",
    "connectionAuthMethod":"{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"<generationId>",
    "enqueuedTime":"2019-06-11T00:00:08.2250000Z"
  },
  "Body":"{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Dispositivos

Cuando se activa por primera vez la exportación de datos continua, se exporta una sola instantánea de todos los dispositivos. Cada dispositivo incluye:
- `id` del dispositivo en IoT Central
- `name` del dispositivo
- `deviceId` del [Device Provisioning Service](/azure/iot-central/core/howto-connect-nodejs)
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

Los archivos exportados contienen una sola línea por registro. En el ejemplo siguiente se muestra un registro en formato Avro que está descodificado:

```json
{ 
  "id":"<id>",
  "name":"Refrigerator 2",
  "simulated":true,
  "deviceId":"<deviceId>",
  "deviceTemplate":{ 
    "id":"<template id>",
    "version":"1.0.0"
  },
  "properties":{ 
    "cloud":{ 
      "location":"New York",
      "maintCon":true,
      "tempThresh":20
    },
    "device":{ 
      "lastReboot":"2018-02-09T22:22:47.156Z"
    }
  },
  "settings":{ 
    "device":{ 
      "fanSpeed":0
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

Los archivos exportados contienen una sola línea por registro. En el ejemplo siguiente se muestra un registro en formato Avro que está descodificado:

```json
{ 
  "id":"<id>",
  "name":"Refrigerated Vending Machine",
  "version":"1.0.0",
  "measurements":{ 
    "telemetry":{ 
      "humidity":{ 
        "dataType":"double",
        "name":"Humidity"
      },
      "magnetometerX":{ 
        "dataType":"double",
        "name":"Magnetometer X"
      },
      "magnetometerY":{ 
        "dataType":"double",
        "name":"Magnetometer Y"
      },
      "magnetometerZ":{ 
        "dataType":"double",
        "name":"Magnetometer Z"
      }
    },
    "states":{ 
      "connectivity":{ 
        "dataType":"enum",
        "name":"Connectivity"
      }
    },
    "events":{ 
      "opened":{ 
        "name":"Door Opened",
        "category":"informational"
      }
    }
  },
  "settings":{ 
    "device":{ 
      "fanSpeed":{ 
        "dataType":"double",
        "name":"Fan Speed",
        "initialValue":0
      }
    }
  },
  "properties":{ 
    "cloud":{ 
      "location":{ 
        "dataType":"string",
        "name":"Location",
        "initialValue":"Seattle"
      },
      "maintCon":{ 
        "dataType":"boolean",
        "name":"Maintenance Contract",
        "initialValue":true
      },
      "tempThresh":{ 
        "dataType":"double",
        "name":"Temperature Alert Threshold",
        "initialValue":30
      }
    },
    "device":{ 
      "lastReboot":{ 
        "dataType":"dateTime",
        "name":"Last Reboot"
      }
    }
  }
}
```

## <a name="read-exported-avro-files"></a>Lectura de archivos Avro exportados

AVRO es un formato binario, por lo que los archivos no se pueden leer en su estado sin procesar. Los archivos se pueden descodificar al formato JSON. Los ejemplos siguientes muestran cómo analizar los archivos Avro de medidas, dispositivos y plantillas de dispositivo. Los ejemplos corresponden a los ejemplos descritos en la sección anterior.

### <a name="read-avro-files-by-using-python"></a>Lectura de los archivos de Avro mediante Python

#### <a name="install-pandas-and-the-pandavro-package"></a>Instalación del paquete de Pandas y PandaAvro

```python
pip install pandas
pip install pandavro
```

#### <a name="parse-a-measurements-avro-file"></a>Análisis de un archivo Avro de medidas

```python
import json
import pandavro as pdx
import pandas as pd


def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
    # where each record is a single row.
    measurements = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary
    # with the device ID located under the connectionDeviceId key.
    transformed["device_id"] = measurements["SystemProperties"].apply(
        lambda x: x["connectionDeviceId"])

    # The Body column is a series of UTF-8 bytes that is stringified
    # and parsed as JSON. This example pulls the humidity property
    # from each column to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(
        lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, print the new DataFrame with our device IDs and humidities.
    print(transformed)
```

#### <a name="parse-a-devices-avro-file"></a>Análisis de un archivo Avro de dispositivos

```python
import json
import pandavro as pdx
import pandas as pd


def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
    # where each record is a single row.
    devices = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device ID is available in the id column.
    transformed["device_id"] = devices["deviceId"]

    # The template ID and version are present in a dictionary under
    # the deviceTemplate column.
    transformed["template_id"] = devices["deviceTemplate"].apply(
        lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(
        lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = devices["settings"].apply(
        lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)
```

#### <a name="parse-a-device-templates-avro-file"></a>Análisis de un archivo Avro de plantillas de dispositivos

```python
import json
import pandavro as pdx
import pandas as pd


def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
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
    transformed["fan_speed"] = templates["settings"].apply(
        lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)
```

### <a name="read-avro-files-by-using-c"></a>Lectura de archivos AVRO con C#

#### <a name="install-the-microsofthadoopavro-package"></a>Instalación del paquete Microsoft.Hadoop.Avro

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

#### <a name="parse-a-measurements-avro-file"></a>Análisis de un archivo Avro de medidas

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
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
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

#### <a name="parse-a-devices-avro-file"></a>Análisis de un archivo Avro de dispositivos

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
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

#### <a name="parse-a-device-templates-avro-file"></a>Análisis de un archivo Avro de plantillas de dispositivos

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
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

#### <a name="parse-a-measurements-avro-file"></a>Análisis de un archivo Avro de medidas

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device ID and humidity from each record.
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

#### <a name="parse-a-devices-avro-file"></a>Análisis de un archivo Avro de dispositivos

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device and template identification
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

#### <a name="parse-a-device-templates-avro-file"></a>Análisis de un archivo Avro de plantillas de dispositivos

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template ID and version from the id and version properties.
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
