---
title: Exportación de datos a Azure Event Hubs y Azure Service Bus | Microsoft Docs
description: Cómo exportar datos desde la aplicación de Azure IoT Central a Azure Event Hubs y Azure Service Bus
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/09/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 732ce570f8235d1f147055af6972c2a8d12599dc
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "71971624"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportación de los datos a Azure IoT Central

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

*Este tema se aplica a administradores*.

En este artículo se describe cómo usar la característica de exportación continua de datos de Azure IoT Central para exportar los datos a sus propias instancias de **Azure Event Hubs** y **Azure Service Bus**. Puede exportar **medidas**, **dispositivos** y **plantillas de dispositivo** a su propio destino para analizar las rutas de acceso activas. Esto incluye el desencadenamiento de reglas personalizadas en Azure Stream Analytics, el desencadenamiento de flujos de trabajo personalizados en Azure Logic Apps o la transformación de datos y su paso a través de Azure Functions. 

> [!Note]
> Una vez más, cuando activa la exportación de datos continua, solo obtiene los datos a partir de ese momento. Actualmente, los datos no se pueden recuperar durante un tiempo cuando la exportación de datos continua estaba desactivada. Para conservar más datos históricos, activa la exportación continua de datos al principio.


## <a name="prerequisites"></a>Requisitos previos

- Ser administrador en la aplicación de IoT Central

## <a name="set-up-export-destination"></a>Configuración del destino de exportación

Si no tiene una instancia existente de Event Hubs o Service Bus a la que exportar, siga estos pasos para crearla:

### <a name="create-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs

1. Cree un [espacio de nombres de Event Hubs en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Puede encontrar más información en los [documentos de Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).
2. Elija una suscripción. 

    > [!Note] 
    > Ahora puede exportar datos a otras suscripciones que **no sean las mismas** que las de la aplicación de IoT Central de pago por uso. En este caso se conectará mediante una cadena de conexión.
3. Cree un centro de eventos en el espacio de nombres de Event Hubs Vaya al espacio de nombres y seleccione **+ Centro de eventos** en la parte superior para crear una instancia de centro de eventos.

### <a name="create-service-bus-namespace"></a>Creación de un espacio de nombres de Service Bus

1. Cree un [espacio de nombres de Service Bus en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5) . Puede encontrar más información en los [documentos de Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal).
2. Elija una suscripción. 

    > [!Note] 
    > Ahora puede exportar datos a otras suscripciones que **no sean las mismas** que las de la aplicación de IoT Central de pago por uso. En este caso se conectará mediante una cadena de conexión.

3. Vaya al espacio de nombres de Service Bus y seleccione **+ Cola** o **+ Tema** en la parte superior para crear una cola o un tema como destino de exportación.


## <a name="set-up-continuous-data-export"></a>Configuración de la exportación continua de datos

Ahora que tiene un destino de Event Hubs o Service Bus al que exportar los datos, siga estos pasos para configurar la exportación de datos continua. 

1. Inicie sesión en su aplicación de IoT Central.

2. En el menú izquierdo, seleccione **Exportación de datos continua**.

    > [!Note]
    > Si no ve la exportación de datos continua en el menú izquierdo, significa que no es un administrador de la aplicación. Hable con un administrador para configurar la exportación de datos.

3. Seleccione el botón **+ Nuevo** en la esquina superior derecha. Elija **Azure Event Hubs** o **Azure Service Bus** como destino de la exportación. 

    > [!NOTE] 
    > El número máximo de exportaciones por aplicación es cinco. 

    ![Creación de una exportación de datos continua](media/howto-export-data/export-new2.png)

4. En el cuadro de lista desplegable, seleccione su **espacio de nombres de Event Hubs o de Service Bus**. También puede elegir la última opción de la lista que es **Escriba una cadena de conexión**. 

    > [!NOTE] 
    > Solo verá las cuentas de almacenamiento, los espacios de nombres de Event Hubs o los espacios de nombres de Service Bus de la **misma suscripción que la de la aplicación de IoT Central**. Si quiere exportar a un destino fuera de esta suscripción, elija **Escriba una cadena de conexión** y vea el paso 5.

    > [!NOTE] 
    > En las aplicaciones de prueba de siete días, la única manera de configurar la exportación de datos continua es mediante una cadena de conexión. El motivo es que estas aplicaciones no tienen una suscripción de Azure asociada.

    ![Creación de un centro de eventos](media/howto-export-data/export-eh.png)

5. (Opcional) Si eligió **Escriba una cadena de conexión**, aparece un nuevo cuadro para que pegue la cadena de conexión. Para obtener la cadena de conexión para su:
    - Event Hubs o Service Bus, vaya al espacio de nombres en Azure Portal.
        - En **Configuración**, seleccione **Directivas de acceso compartido**.
        - Elija el valor predeterminado **RootManageSharedAccessKey** o cree uno.
        - Copie la cadena de conexión principal o la secundaria.
 
6. Elija un centro de eventos, una cola o un tema del cuadro de lista desplegable.

7. En **Datos para exportar**, especifique cada tipo de datos para exportar y establezca el tipo en **Activado**.

8. Para activar la exportación continua de datos, asegúrese de que la tecla de alternancia **Exportación de datos** esté **activada**. Seleccione **Guardar**.

9. Transcurridos unos minutos, los datos aparecen en el destino elegido.


## <a name="data-format"></a>Formato de datos

Los datos de medidas, dispositivos y plantillas de dispositivo se exportan a su centro de eventos o cola o tema de Service Bus casi en tiempo real. Los datos de medidas exportados contienen la totalidad del mensaje que los dispositivos enviaron a IoT Central, no solo los valores de las propias medidas. Los datos de dispositivo exportados contienen los cambios en las propiedades y la configuración de todos los dispositivos; las plantillas de dispositivo exportadas contienen los cambios en todas las plantillas de dispositivo. Los datos exportados se encuentran en la propiedad "body" y están en formato JSON.

> [!NOTE]
> Al elegir una instancia de Service Bus como destino de exportación, las colas y los temas **no deben tener habilitada la opción de detección de sesiones o de duplicados**. Si cualquiera de estas opciones está habilitada, algunos mensajes no llegarán a la cola o tema.

### <a name="measurements"></a>Medidas

Un nuevo mensaje se exporta rápidamente después de que IoT Central recibe el mensaje de un dispositivo. Cada mensaje exportado en Event Hubs y Service Bus contiene el mensaje completo que el dispositivo ha enviado en la propiedad "body" en formato JSON. 

> [!NOTE]
> Los dispositivos que envían las mediciones se representan mediante identificadores de dispositivo (consulte las secciones siguientes). Para obtener los nombres de los dispositivos, exporte los datos del dispositivo y ponga en correlación cada mensaje mediante el identificador **connectionDeviceId** que coincide con el identificador **deviceId** del mensaje del dispositivo.

El ejemplo siguiente muestra un mensaje acerca de los datos de las medidas que se ha recibido en Event Hubs o en la cola o tema de Service Bus.

```json
{
  "body": {
    "humidity": 29.06963648666288,
    "temp": 8.4503795661685,
    "pressure": 1075.8334910110093,
    "magnetometerX": 408.6966458887116,
    "magnetometerY": -532.8809796603962,
    "magnetometerZ": 174.70653875528205,
    "accelerometerX": 1481.546749013788,
    "accelerometerY": -1139.4316656437406,
    "accelerometerZ": 811.6928695575307,
    "gyroscopeX": 442.19879163299856,
    "gyroscopeY": 123.23710975717177,
    "gyroscopeZ": 708.5397575786151,
    "deviceState": "DANGER"
  },
  "annotations": {
    "iothub-connection-device-id": "<connectionDeviceId>",
    "iothub-connection-auth-method": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id": "<generationId>",
    "iothub-enqueuedtime": 1539381029965,
    "iothub-message-source": "Telemetry",
    "x-opt-sequence-number": 25325,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539381030200
  },
  "sequenceNumber": 25325,
  "enqueuedTimeUtc": "2018-10-02T21:50:30.200Z",
  "offset": "<offset>",
  "properties": {
    "content_type": "application/json",
    "content_encoding": "utf-8"
  }
}
```

### <a name="devices"></a>Dispositivos

Los mensajes que contienen los datos del dispositivo se envían a Event Hubs o a la cola o tema de Service Bus cada pocos minutos. Esto significa que cada pocos minutos, un lote de mensajes llegará con datos acerca de
- Nuevos dispositivos que se han agregado
- Dispositivos con propiedades y valores de configuración modificados

Cada mensaje representa uno o más cambios que se han producido en un dispositivo desde la exportación del último mensaje. La información que se enviará en cada mensaje incluye:
- `id` del dispositivo en IoT Central
- `name` del dispositivo
- `deviceId` del [Device Provisioning Service](https://aka.ms/iotcentraldocsdps)
- Información de la plantilla de dispositivo
- Valores de propiedad
- Valores de configuración

> [!NOTE]
> Los dispositivos que se eliminaron desde el último procesamiento por lotes no se exportan. Actualmente, no hay ningún indicador en los mensajes exportados para los dispositivos eliminados.
>
> La plantilla de dispositivo a la que pertenece cada dispositivo se representa mediante un identificador de plantilla de dispositivo. Para obtener el nombre de la plantilla de dispositivo, asegúrese de exportar también los datos de esta.

El ejemplo siguiente muestra un mensaje acerca de los datos de dispositivo en Event Hubs o en la cola o tema de Service Bus:


```json
{
  "body": {
    "id": "<id>",
    "name": "<deviceName>",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
      "id": "<templateId>",
      "version": "1.0.0"
    },
    "properties": {
      "cloud": {
        "location": "Seattle"
      },
      "device": {
        "dieNumber": 5445.5862873026645
      }
    },
    "settings": {
      "device": {
        "fanSpeed": 0
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "devices",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 39740,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274959654
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 39740,
  "enqueuedTimeUtc": "2018-10-02T16:22:39.654Z",
  "offset": "<offset>",
}
```

### <a name="device-templates"></a>Plantillas de dispositivo

Los mensajes que contienen los datos de las plantillas del dispositivo se envían a Event Hubs o a la cola o tema de Service Bus cada pocos minutos. Esto significa que cada pocos minutos, un lote de mensajes llegará con datos acerca de
- Nuevas plantillas de dispositivo que se han agregado
- Plantillas de dispositivo con mediciones, propiedades y definiciones de configuración que cambiaron

Cada mensaje representa uno o más cambios que se han producido en una plantilla de dispositivo desde la exportación del último mensaje. La información que se enviará en cada mensaje incluye:
- `id` de la plantilla de dispositivo
- `name` de la plantilla de dispositivo
- `version` de la plantilla de dispositivo
- Tipos de datos de medición y valores mínimos y máximos
- Tipos de datos de propiedad y valores predeterminados
- Tipos de datos de configuración y valores predeterminados

> [!NOTE]
> Las plantillas de dispositivo que se eliminaron desde el último procesamiento por lotes no se exportan. Actualmente, no hay ningún indicador en los mensajes exportados para las plantillas de dispositivo eliminadas.

El ejemplo siguiente muestra un mensaje acerca de los datos de las plantillas de dispositivo en Event Hubs o en la cola o tema de Service Bus:

```json
{ 
  "body":{ 
    "id":"<id>",
    "version":"1.0.0",
    "name":"<templateName>",
    "measurements":{ 
      "telemetry":{ 
        "humidity":{ 
          "dataType":"double",
          "name":"humidity"
        },
        "pressure":{ 
          "dataType":"double",
          "name":"pressure"
        },
        "temp":{ 
          "dataType":"double",
          "name":"temperature"
        }
      }
    },
    "properties":{ 
      "cloud":{ 
        "location":{ 
          "dataType":"string",
          "name":"Location"
        }
      },
      "device":{ 
        "dieNumber":{ 
          "dataType":"double",
          "name":"Die Number"
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
    }
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
  "enqueuedTimeUtc":"2018-10-02T16:23:05.085Z",
  "offset":"<offset>"
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo exportar los datos a Azure Event Hubs y Azure Service Bus, vaya al paso siguiente:

> [!div class="nextstepaction"]
> [Desencadenar Azure Functions](howto-trigger-azure-functions.md)
