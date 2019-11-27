---
title: Exportación de los datos de Azure IoT Central | Microsoft Docs
description: Cómo exportar datos desde la aplicación de Azure IoT Central a Azure Event Hubs, Azure Service Bus y Azure Blob Storage
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 10/15/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: 35415fd1a552328faa8d1ad5812f44d8f4b6d5e4
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73894180"
---
# <a name="export-your-azure-iot-central-data-preview-features"></a>Exportación de los datos de Azure IoT Central (Características en vista previa (GB))

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

*Este tema se aplica a administradores*.

En este artículo se describe cómo usar la característica de exportación continua de datos de Azure IoT Central para exportar los datos a instancias de **Azure Event Hubs**, **Azure Service Bus** o **Azure Blob Storage** Los datos se exportan en formato JSON y pueden incluir telemetría, información del dispositivo e información de la plantilla de dispositivo. Use los datos exportados para:

- Información y análisis de la ruta de acceso activa. Esta opción incluye el desencadenamiento de reglas personalizadas en Azure Stream Analytics, el desencadenamiento de flujos de trabajo personalizados en Azure Logic Apps o su paso a través de Azure Functions para su transformación.
- Análisis en frío de rutas de acceso, como modelos de entrenamiento en Azure Machine Learning o análisis de tendencias a largo plazo en Microsoft Power BI.

> [!Note]
> Al activar la exportación de datos continua, solo obtendrá los datos a partir de ese momento. Actualmente, los datos no se pueden recuperar durante un tiempo cuando la exportación de datos continua estaba desactivada. Para conservar más datos históricos, activa la exportación continua de datos al principio.

## <a name="prerequisites"></a>Requisitos previos

Ser administrador en la aplicación de IoT Central

## <a name="set-up-export-destination"></a>Configuración del destino de exportación

El destino de exportación debe existir antes de configurar la exportación continua de datos.

### <a name="create-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs

Si no tiene un espacio de nombres existente de Event Hubs al que exportar, siga estos pasos:

1. Cree un [espacio de nombres de Event Hubs en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Puede encontrar más información en los [documentos de Azure Event Hubs](../../event-hubs/event-hubs-create.md).

2. Elija una suscripción. Ahora puede exportar datos a otras suscripciones que no estén en la misma suscripción que la aplicación de IoT Central de pago por uso. En este caso se conecta mediante una cadena de conexión.

3. Cree un centro de eventos en el espacio de nombres de Event Hubs Vaya al espacio de nombres y seleccione **+ Centro de eventos** en la parte superior para crear una instancia de centro de eventos.

### <a name="create-service-bus-namespace"></a>Creación de un espacio de nombres de Service Bus

Si no tiene un espacio de nombres existente de Service Bus al que exportar, siga estos pasos:

1. Cree un [nuevo espacio de nombres de Service Bus en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5). Puede encontrar más información en los [documentos de Azure Service Bus](../../service-bus-messaging/service-bus-create-namespace-portal.md).
2. Elija una suscripción. Ahora puede exportar datos a otras suscripciones que no estén en la misma suscripción que la aplicación de IoT Central de pago por uso. En este caso se conecta mediante una cadena de conexión.

3. Vaya al espacio de nombres de Service Bus y seleccione **+ Cola** o **+ Tema** en la parte superior para crear una cola o un tema como destino de exportación.

Al elegir Service Bus como destino de exportación, las colas y los temas no deben tener habilitada la opción de detección de sesiones o de duplicados. Si cualquiera de estas opciones está habilitada, algunos mensajes no llegarán a la cola o tema.

### <a name="create-storage-account"></a>Crear cuenta de almacenamiento

Si no tiene una cuenta existente de Azure Storage a la que exportar, siga estos pasos:

1. Cree una [cuenta de almacenamiento en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Puede obtener más información acerca de cómo crear nuevas [cuentas de Azure Blob Storage](https://aka.ms/blobdocscreatestorageaccount) o [cuentas de almacenamiento de Azure Data Lake Storage v2](../../storage/blobs/data-lake-storage-quickstart-create-account.md).

    - Si decide exportar datos a una cuenta de almacenamiento de Azure Data Lake Storage v2, debe elegir **BlobStorage** como **Tipo de cuenta**.
    - Puede exportar datos a cuentas de almacenamiento en suscripciones que no sean la de su aplicación de IoT Central de pago por uso. En este caso se conectará mediante una cadena de conexión.

2. Cree un contenedor en la cuenta de almacenamiento. Vaya a la cuenta de almacenamiento. En **Blob Service** seleccione **Examinar blobs**. Seleccione **+ Contenedor** en la parte superior para crear un contenedor.

## <a name="set-up-continuous-data-export"></a>Configuración de la exportación continua de datos

Ahora que tiene un destino al que exportar los datos, siga estos pasos para configurar la exportación de datos continua.

1. Inicie sesión en su aplicación de IoT Central.

2. En el panel izquierdo, seleccione **Exportación de datos**.

    > [!Note]
    > Si no ve Exportación de datos en el panel izquierdo, significa que no es un administrador de la aplicación. Hable con un administrador para configurar la exportación de datos.

3. Seleccione el botón **+ Nuevo** en la esquina superior derecha. Elija **Azure Event Hubs**, **Azure Service Bus** o **Azure Blob Storage** como destino de la exportación. El número máximo de exportaciones por aplicación es cinco.

    ![Creación de una exportación de datos continua](media/howto-export-data/export-new2.png)

4. En el cuadro de lista desplegable, seleccione su **espacio de nombres de Event Hubs**, **espacio de nombres de Service Bus**, **espacio de nombres de la cuenta de almacenamiento** o **escriba una cadena de conexión**.

    - Solo verá las cuentas de almacenamiento, los espacios de nombres de Event Hubs y los espacios de nombres de Service Bus de la misma suscripción que la de la aplicación de IoT Central. Si quiere exportar a un destino fuera de esta suscripción, elija **Escriba una cadena de conexión** y vea el paso 5.
    - En las aplicaciones de prueba de siete días, la única manera de configurar la exportación de datos continua es mediante una cadena de conexión. Estas aplicaciones no tienen una suscripción de Azure asociada.

    ![Creación de un centro de eventos](media/howto-export-data/export-eh.png)

5. (Opcional) Si eligió **Escriba una cadena de conexión**, aparece un nuevo cuadro para que pegue la cadena de conexión. Para obtener la cadena de conexión para su:
    - Event Hubs o Service Bus, vaya al espacio de nombres en Azure Portal.
        - En **Configuración**, seleccione **Directivas de acceso compartido**.
        - Elija el valor predeterminado **RootManageSharedAccessKey** o cree uno.
        - Copie la cadena de conexión principal o la secundaria.
    - Cuenta de almacenamiento, vaya a la cuenta de almacenamiento en Azure Portal:
        - En **Configuración**, seleccione **Claves de acceso**.
        - Copie la cadena de conexión key1 o la cadena de conexión key2.

6. Elija un centro de eventos, una cola, un tema o un contenedor del cuadro de lista desplegable.

7. En **Datos para exportar**, elija los tipos de datos para exportar y establezca el tipo en **Activado**.

8. Para activar la exportación continua de datos, asegúrese de que la tecla de alternancia **Exportación de datos** esté **activada**. Seleccione **Guardar**.

9. Transcurridos unos minutos, los datos aparecen en el destino elegido.

## <a name="export-contents-and-format"></a>Exportación del contenido y formato

Los datos de telemetría exportados contienen la totalidad del mensaje que los dispositivos enviaron a IoT Central, no solo los valores de los propios datos de telemetría. Los datos de dispositivo exportados contienen los cambios en las propiedades y los metadatos de todos los dispositivos; las plantillas de dispositivo exportadas contienen los cambios en todas las plantillas de dispositivo.

Para Event Hubs y Service Bus, los datos se exportan casi en tiempo real. Los datos se ubican en la propiedad Body y están en formato JSON (consulte a continuación para ver ejemplos).

Para Blob Storage, los datos se exportan una vez por minuto y cada archivo contiene el lote de cambios desde el último archivo exportado. Los datos exportados se colocan en tres carpetas con formato JSON. Las rutas de acceso predeterminadas de la cuenta de almacenamiento son:

- Telemetría: _{contenedor}/{id. de aplicación}/telemetry/{AAAA}/{MM}/{dd}/{hh}/{mm}/{nombre de archivo}_
- Dispositivos: _{contenedor}/{id. de aplicación}/devices/{AAAA}/{MM}/{dd}/{hh}/{mm}/{nombre de archivo}_
- Plantillas de dispositivo: _{contenedor}/{id. de aplicación}/deviceTemplates/{AAAA}/{MM}/{dd}/{hh}/{mm}/{nombre de archivo}_

Puede examinar los archivos exportados en Azure Portal si va al archivo y elige la pestaña para **editar el blob**.


## <a name="telemetry"></a>Telemetría

Para Event Hubs y Service Bus, se exporta un mensaje nuevo rápidamente después de que IoT Central reciba el mensaje de un dispositivo y cada mensaje exportado contiene el mensaje completo que el dispositivo envió en la propiedad body en formato JSON.

Para Blob Storage, los mensajes se procesan por lotes y se exportan una vez por minuto. Los archivos exportados usan el mismo formato que los archivos de mensajes exportados por el [enrutamiento de mensajes de IoT Hub](../../iot-hub/iot-hub-csharp-csharp-process-d2c.md) a Blob Storage. 

> [!NOTE]
> Para Blob Storage, asegúrese de que los dispositivos estén enviando mensajes que tengan `contentType: application/JSON` y `contentEncoding:utf-8` (o `utf-16`, `utf-32`). Consulte la [documentación de IoT Hub](../../iot-hub/iot-hub-devguide-routing-query-syntax.md#message-routing-query-based-on-message-body) para ver un ejemplo.

El dispositivo que envió la telemetría se representa mediante el identificador del dispositivo (consulte las secciones siguientes). Para obtener los nombres de los dispositivos, exporte los datos del dispositivo y ponga en correlación cada mensaje mediante el objeto **connectionDeviceId** que coincide con el objeto **deviceId** del mensaje del dispositivo.

Este es un mensaje de ejemplo recibido en una cola o tema de Event Hubs o Service Bus.

```json
{
  "body":{
    "temp":67.96099945281145,
    "humid":58.51139305465015,
    "pm25":36.91162432340187
  },
  "annotations":{
    "iothub-connection-device-id":"<deviceId>",
    "iothub-connection-auth-method":"{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id":"<generationId>",
    "iothub-enqueuedtime":1539381029965,
    "iothub-message-source":"Telemetry",
    "x-opt-sequence-number":25325,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539381030200
  },
  "sequenceNumber":25325,
  "enqueuedTimeUtc":"2018-10-12T21:50:30.200Z",
  "offset":"<offset>",
  "properties":{
    "content_type":"application/json",
    "content_encoding":"utf-8"
  }
}
```

Este es un registro de ejemplo exportado al almacenamiento de blobs:

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

## <a name="devices"></a>Dispositivos

Cada mensaje o registro de una instantánea representa uno o más cambios que se han producido en un dispositivo y sus propiedades desde la exportación del último mensaje. Esto incluye:

- `@id` del dispositivo en IoT Central
- `name` del dispositivo
- `deviceId` del [Device Provisioning Service](../core/howto-connect-nodejs.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json)
- Información de la plantilla de dispositivo
- Valores de propiedad

La plantilla de dispositivo a la que pertenece cada dispositivo se representa mediante `instanceOf`. Para obtener el nombre de la plantilla de dispositivo e información adicional sobre ella, asegúrese de exportar también los datos de la plantilla de dispositivo.

Los dispositivos eliminados no se exportan. Actualmente, no hay ningún indicador en los mensajes exportados para los dispositivos eliminados.

En Event Hubs y Service Bus, los mensajes que contienen datos del dispositivo se envían a la cola o tema de Event Hubs o Service Bus casi en tiempo real, tal y como aparece en IoT Central. 

Para Blob Storage, una nueva instantánea que contiene todos los cambios desde la última vez escrita se exporta una vez por minuto.

Este es un mensaje de ejemplo sobre dispositivos y datos de propiedades en una cola o tema de Event Hub o Service Bus:

```json
{
  "body":{
    "@id":"<id>",
    "@type":"Device",
    "displayName":"Airbox - 266d30aedn5",
    "data":{
      "$cloudProperties":{
        "Color":"blue"
      },
      "EnvironmentalSensor":{
        "thsensormodel":{
          "reported":{
            "value":"A1",
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        },
        "pm25sensormodel":{
          "reported":{
            "value":"P1",
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        }
      },
      "urn_azureiot_DeviceManagement_DeviceInformation":{
        "totalStorage":{
          "reported":{
            "value":3088.1959855710156,
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        },
        "totalMemory":{
          "reported":{
            "value":16005.703586477555,
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        }
      }
    },
    "instanceOf":"<templateId>",
    "deviceId":"<deviceId>",
    "simulated":true
  },
  "annotations":{
    "iotcentral-message-source":"devices",
    "x-opt-partition-key":"<partitionKey>",
    "x-opt-sequence-number":39740,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539274959654
  },
  "partitionKey":"<partitionKey>",
  "sequenceNumber":39740,
  "enqueuedTimeUtc":"2019-10-02T18:14:49.3820326Z",
  "offset":"<offset>"
}
```

Esta es una instantánea de ejemplo que contiene los datos de los dispositivos y las propiedades en Blob Storage. Los archivos exportados contienen una sola línea por registro.

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

## <a name="device-templates"></a>Plantillas de dispositivo

Cada mensaje o registro de instantáneas representa uno o más cambios que se han producido en una plantilla de dispositivo desde la exportación del último mensaje. Entre la información enviada en cada mensaje o registro se incluye lo siguiente:

- `@id` de la plantilla de dispositivo que coincide con el objeto `instanceOf` del flujo de dispositivos anterior
- `name` de la plantilla de dispositivo
- `version` de la plantilla de dispositivo
- El dispositivo `capabilityModel`, incluido su `interfaces`, y los datos de telemetría, las propiedades y las definiciones de comandos
- Definiciones de `cloudProperties`
- Reemplazos y valores iniciales, insertados con el `capabilityModel`

Las plantillas de dispositivo eliminadas no se exportan. Actualmente, no hay ningún indicador en los mensajes exportados para las plantillas de dispositivo eliminadas.

En Event Hubs y Service Bus, los mensajes que contienen datos de la plantilla de dispositivo se envían a la cola o tema de Event Hubs o Service Bus casi en tiempo real, tal y como aparece en IoT Central. 

Para Blob Storage, una nueva instantánea que contiene todos los cambios desde la última vez escrita se exporta una vez por minuto.

Este es un mensaje de ejemplo sobre datos de plantilla de dispositivo en una cola o tema de Event Hub o Service Bus:

```json
{
  "body":{
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
    },
    "annotations":{
      "iotcentral-message-source":"deviceTemplates",
      "x-opt-partition-key":"<partitionKey>",
      "x-opt-sequence-number":25315,
      "x-opt-offset":"<offset>",
      "x-opt-enqueued-time":1539274985085
    },
    "partitionKey":"<partitionKey>",
    "sequenceNumber":25315,
    "enqueuedTimeUtc":"2019-10-02T16:23:05.085Z",
    "offset":"<offset>"
  }
}
```

Esta es una instantánea de ejemplo que contiene los datos de los dispositivos y las propiedades en Blob Storage. Los archivos exportados contienen una sola línea por registro.

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

Ahora que sabe cómo exportar los datos a Azure Event Hubs, Azure Service Bus y Azure Blob Storage, vaya al paso siguiente:

> [!div class="nextstepaction"]
> [Desencadenar Azure Functions](../core/howto-trigger-azure-functions.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json)
