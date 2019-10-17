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
ms.openlocfilehash: 9ac650273a53da715b89e3233b30cf50710cfcfd
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973270"
---
# <a name="export-your-data-to-azure-blob-storage-preview-features"></a>Exportación de datos a Azure Blob Storage (característica de versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

*Este tema se aplica a administradores*.

En este artículo se describe cómo usar la función de exportación continua de datos en Azure IoT Central para exportar periódicamente datos a su **cuenta de almacenamiento de Azure Blob** o a la **cuenta de almacenamiento de Azure Data Lake Storage Gen2** . Puede exportar **datos de telemetría**, **dispositivos** y **plantillas de dispositivo** a archivos con el formato JSON. Los datos exportados se pueden utilizar para realizar análisis en frío de rutas de acceso, como modelos de entrenamiento en Azure Machine Learning o análisis de tendencias a largo plazo en Microsoft Power BI.

> [!Note]
> Al activar la exportación de datos continua, solo obtendrá los datos a partir de ese momento. Actualmente, los datos no se pueden recuperar durante un tiempo cuando la exportación de datos continua estaba desactivada. Para conservar más datos históricos, activa la exportación continua de datos al principio.


## <a name="prerequisites"></a>Requisitos previos

- Ser administrador en la aplicación de IoT Central

## <a name="create-storage-account"></a>Crear cuenta de almacenamiento
Si no tiene una instancia existente de Storage a la que exportar, siga estos pasos:

1. Cree una [cuenta de almacenamiento en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Puede obtener más información acerca de cómo crear nuevas [cuentas de Azure Blob Storage](https://aka.ms/blobdocscreatestorageaccount) o [cuentas de almacenamiento de Azure Data Lake Storage v2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-account).

    > [!Note] 
    > Si decide exportar datos a una cuenta de almacenamiento de ADLS v2, debe elegir **Tipo de cuenta** como **BlobStorage**. 

    > [!Note] 
    > Puede exportar datos a cuentas de almacenamiento en suscripciones que no sean la de su aplicación de IoT Central de pago por uso. En este caso se conectará mediante una cadena de conexión.

2. Cree un contenedor en la cuenta de almacenamiento. Vaya a la cuenta de almacenamiento. En **Blob Service** seleccione **Examinar blobs**. Seleccione **+ Contenedor** en la parte superior para crear un contenedor.


## <a name="set-up-continuous-data-export"></a>Configuración de la exportación continua de datos

Ahora que tiene un destino de Storage al que exportar los datos, siga estos pasos para configurar la exportación de datos continua. 

1. Inicie sesión en su aplicación de IoT Central.

2. En el menú de la izquierda, seleccione **Exportación de datos**.

    > [!Note]
    > Si no ve la exportación de datos en el menú izquierdo, significa que no es un administrador de la aplicación. Hable con un administrador para configurar la exportación de datos.

3. Seleccione el botón **+ Nuevo** en la esquina superior derecha. Elija **Azure Blob Storage** como destino de la exportación. 

    > [!NOTE] 
    > El número máximo de exportaciones por aplicación es cinco. 

    ![Creación de una exportación de datos continua](media/howto-export-data-pnp/export-new2.png)

4. En el cuadro de lista desplegable, seleccione su **espacio de nombres de Storage**. También puede elegir la última opción de la lista que es **Escriba una cadena de conexión**. 

    > [!NOTE] 
    > Solo verá los espacios de nombres de cuentas de Storage, en la **misma suscripción que la de la aplicación de IoT Central**. Si quiere exportar a un destino fuera de esta suscripción, elija **Escriba una cadena de conexión** y vea el paso 5.

    > [!NOTE] 
    > En las aplicaciones de prueba de siete días, la única manera de configurar la exportación de datos continua es mediante una cadena de conexión. El motivo es que estas aplicaciones no tienen una suscripción de Azure asociada.

    ![Crear nueva exportación a Blob](media/howto-export-data-pnp/export-create-blob2.png)

5. (Opcional) Si eligió **Escriba una cadena de conexión**, aparece un nuevo cuadro para que pegue la cadena de conexión. Para obtener la cadena de conexión de la cuenta de almacenamiento, vaya a la cuenta de almacenamiento en Azure Portal:
    - En **Configuración**, seleccione **Claves de acceso**.
    - Copie la cadena de conexión key1 o la cadena de conexión key2.
 
6. Elija un contenedor en el cuadro de lista desplegable. Si no tiene un contenedor, vaya a la cuenta de almacenamiento en Azure Portal:
    - En el **Blob service**, seleccione **Blobs**. Haga clic en **+ Contenedor** y asigne un nombre a su contenedor. Elija un nivel de acceso público para sus datos (cualquiera funcionará con la exportación continua de datos). Puede encontrar más información en los [documentos de Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container).

7.  En **Datos para exportar**, especifique cada tipo de datos para exportar y establezca el tipo en **Activado**.
   
    > [!NOTE] 
    > Los datos se exportan en formato JSON.

8. Para activar la exportación continua de datos, asegúrese de que la tecla de alternancia **Exportación de datos** esté activada. Seleccione **Guardar**.

   ![Configuración de la exportación continua de datos](media/howto-export-data-pnp/export-list-blob2.png)

9. Después de unos minutos, los datos aparecerán en la cuenta de almacenamiento.


## <a name="path-structure"></a>Estructura de la ruta de acceso

Los datos de telemetría, dispositivos y plantillas de dispositivo se exportan a la cuenta de almacenamiento una vez por minuto, y cada archivo contiene el lote de cambios desde el último archivo exportado. Los datos exportados se colocan en tres carpetas con formato JSON. Las rutas de acceso predeterminadas de la cuenta de almacenamiento son:
- Telemetría: {container}/{app-id}/telemetry/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}
- Dispositivos: {container}/{app-id}/devices/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}
- Plantillas de dispositivo: {container}/{app-id}/deviceTemplates/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}

Puede examinar los archivos exportados en Azure Portal si va al archivo y elige la pestaña para **editar el blob**.

## <a name="data-format"></a>Formato de datos
### <a name="telemetry"></a>Telemetría

Los datos de telemetría exportados contienen todos los mensajes nuevos recibidos por IoT Central de todos los dispositivos durante ese tiempo. Los archivos exportados usan el mismo formato que los archivos de mensajes exportados por el [enrutamiento de mensajes de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) a Blob Storage.

> [!NOTE]
> Asegúrese de que sus dispositivos estén enviando mensajes que tengan `contentType: application/JSON` y `contentEncoding:utf-8` (o `utf-16`, `utf-32`). Consulte la [documentación de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-body) para ver un ejemplo.

> [!NOTE]
> Los dispositivos que envían los datos de telemetría se representan mediante identificadores de dispositivo (consulte las secciones siguientes). Para obtener los nombres de los dispositivos, exporte las instantáneas del dispositivo. Correlacione cada registro de mensaje mediante el **connectionDeviceId** que coincide con el **deviceId** del registro del dispositivo.

En el ejemplo siguiente se muestra un registro en formato JSON.

```json
{ 
  "EnqueuedTimeUtc":"2019-09-26T17:46:09.8870000Z",
  "Properties":{ 

  },
  "SystemProperties":{ 
    "connectionDeviceId":"<deviceid>",
    "connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"637051167384630591",
    "contentType":"application/json",
    "contentEncoding":"utf-8",
    "enqueuedTime":"2019-09-26T17:46:09.8870000Z"
  },
  "Body":{ 
    "temp":49.91322758395974,
    "humid":49.61214852573155,
    "pm25":25.87332214661367
  }
}
```

### <a name="devices"></a>Dispositivos

Cuando se activa por primera vez la exportación de datos continua, se exporta una sola instantánea de todos los dispositivos. Cada dispositivo incluye:
- `@id` del dispositivo en IoT Central
- `name` del dispositivo
- `deviceId` del [Device Provisioning Service](https://aka.ms/iotcentraldocsdps)
- Información de la plantilla de dispositivo
- Valores de propiedad

Una nueva instantánea se escribe una vez por minuto. La instantánea incluye:

- Los nuevos dispositivos que se agregaron desde la última instantánea.
- Dispositivos con valores de propiedad modificados desde la última instantánea.

> [!NOTE]
> Los dispositivos que se eliminaron desde la última instantánea no se exportan. Actualmente, las instantáneas no tienen indicadores para los dispositivos eliminados.
>
> La plantilla de dispositivo a la que pertenece cada dispositivo se representa mediante el campo `instanceOf`. Para obtener el nombre de la plantilla de dispositivo, exporte las instantáneas de plantilla de dispositivo.

Los archivos exportados contienen una sola línea por registro. En el ejemplo siguiente se muestra un registro en formato JSON.

```json
{ 
  "@id":"<id-value>",
  "@type":"Device",
  "displayName":"Airbox",
  "data":{ 
    "$cloudProperties":{ 
        "Color":"blue"
    },
    "EnvironmentalSensor":{ 
      "thsensormodel":{ 
        "reported":{ 
          "value":"Neque quia et voluptatem veritatis assumenda consequuntur quod.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "pm25sensormodel":{ 
        "reported":{ 
          "value":"Aut alias odio.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    },
    "urn_azureiot_DeviceManagement_DeviceInformation":{ 
      "totalStorage":{ 
        "reported":{ 
          "value":27900.9730905171,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "totalMemory":{ 
        "reported":{ 
          "value":4667.82916715811,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    }
  },
  "instanceOf":"<template-id>",
  "deviceId":"<device-id>",
  "simulated":true
}
```

### <a name="device-templates"></a>Plantillas de dispositivo

Cuando se activa por primera vez la exportación de datos continua, se exporta una sola instantánea de todas las plantillas de dispositivo. Cada plantilla de dispositivo incluye:
- `@id` de la plantilla de dispositivo
- `name` de la plantilla de dispositivo
- `version` de la plantilla de dispositivo
- El dispositivo `capabilityModel`, incluido su `interfaces`, y los datos de telemetría, las propiedades y las definiciones de comandos
- Definiciones de `cloudProperties`
- Reemplazos y valores iniciales, insertados con el `capabilityModel`

Una nueva instantánea se escribe una vez por minuto. La instantánea incluye:

- Nuevas plantillas de dispositivo que se agregaron desde la última instantánea Esto incluye las nuevas versiones de las plantillas de dispositivo.
- Plantillas de dispositivo con los reemplazos, valores iniciales y definiciones de propiedades de la nube modificados desde la última instantánea.

> [!NOTE]
> Las plantillas de dispositivo que se eliminaron desde la última instantánea no se exportan. Actualmente, las instantáneas no tienen indicadores para las plantillas de dispositivos eliminadas.

Los archivos exportados contienen una sola línea por registro. En el ejemplo siguiente se muestra un registro en formato JSON.

```json
{ 
  "@id":"<template-id>",
  "@type":"DeviceModelDefinition",
  "displayName":"Airbox",
  "capabilityModel":{ 
    "@id":"<id>",
    "@type":"CapabilityModel",
    "implements":[ 
      { 
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"EnvironmentalSensor",
        "schema":{ 
          "@id":"<id>",
          "@type":"Interface",
          "comment":"Requires temperature and humidity sensors.",
          "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
          "displayName":"Environmental Sensor",
          "contents":[ 
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current temperature on the device",
              "displayName":"Temperature",
              "name":"temp",
              "schema":"double",
              "unit":"Units/Temperature/celsius",
              "valueDetail":{ 
                "@id":"<id>",
                "@type":"ValueDetail/NumberValueDetail",
                "minValue":{ 
                  "@value":"50"
                }
              },
              "visualizationDetail":{ 
                "@id":"<id>",
                "@type":"VisualizationDetail"
              }
            },
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current humidity on the device",
              "displayName":"Humidity",
              "name":"humid",
              "schema":"integer"
            },
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current PM2.5 on the device",
              "displayName":"PM2.5",
              "name":"pm25",
              "schema":"integer"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "description":"T&H Sensor Model Name",
              "displayName":"T&H Sensor Model",
              "name":"thsensormodel",
              "schema":"string"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "description":"PM2.5 Sensor Model Name",
              "displayName":"PM2.5 Sensor Model",
              "name":"pm25sensormodel",
              "schema":"string"
            }
          ]
        }
      },
      { 
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"urn_azureiot_DeviceManagement_DeviceInformation",
        "schema":{ 
          "@id":"<id>",
          "@type":"Interface",
          "displayName":"Device information",
          "contents":[ 
            { 
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
              "displayName":"Total storage",
              "name":"totalStorage",
              "displayUnit":"kilobytes",
              "schema":"long"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
              "displayName":"Total memory",
              "name":"totalMemory",
              "displayUnit":"kilobytes",
              "schema":"long"
            }
          ]
        }
      }
    ],
    "displayName":"AAEONAirbox52"
  },
  "solutionModel":{ 
    "@id":"<id>",
    "@type":"SolutionModel",
    "cloudProperties":[ 
      { 
        "@id":"<id>",
        "@type":"CloudProperty",
        "displayName":"Color",
        "name":"Color",
        "schema":"string",
        "valueDetail":{ 
          "@id":"<id>",
          "@type":"ValueDetail/StringValueDetail"
        },
        "visualizationDetail":{ 
          "@id":"<id>",
          "@type":"VisualizationDetail"
        }
      }
    ]
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo exportar los datos, continúe con el paso siguiente:

> [!div class="nextstepaction"]
> [Uso del puente de dispositivos de IoT Central para conectar otras nubes de IoT](howto-build-iotc-device-bridge.md)
