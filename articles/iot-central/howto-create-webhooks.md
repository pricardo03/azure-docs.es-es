---
title: Creación de webhooks para reglas en Azure IoT Central | Microsoft Docs
description: Cree webhooks en Azure IoT Central para notificar automáticamente a otras aplicaciones cuándo se activan las reglas.
author: viv-liu
ms.author: viviali
ms.date: 09/17/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 896d4e9c775fa0b0c8eb062d11d141901daa7242
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295995"
---
# <a name="create-webhook-actions-on-rules-in-azure-iot-central"></a>Creación de acciones de webhook para reglas en Azure IoT Central

*Este tema se aplica a creadores y administradores.*

Los webhooks permiten conectar la aplicación de IoT Central a otras aplicaciones y servicios para supervisión remota y notificaciones. Los webhooks notifican automáticamente a otras aplicaciones y servicios a los que se conecta si se activa una regla en la aplicación IoT Central. Su aplicación IoT Central enviará una solicitud POST a otro punto de conexión HTTP de la aplicación si se activa una regla. La carga útil contendrá los detalles del dispositivo y de la activación de la regla. 

## <a name="how-to-set-up-the-webhook"></a>Configuración del webhook
En este ejemplo, se conectará a RequestBin para obtener notificaciones mediante webhooks cuando se activan las reglas. 

1. Abra [RequestBin](http://requestbin.net/). 
1. Cree una instancia de RequestBin y copie la **dirección URL de la papelera**. 
1. Cree una [regla de telemetría](howto-create-telemetry-rules.md) o una [regla de evento](howto-create-event-rules.md). Guarde la regla y agregue una nueva acción.
![Pantalla de creación de webhooks](media/howto-create-webhooks/webhookcreate.PNG)
1. Elija la acción de webhook y proporcione un nombre para mostrar y pegue la dirección URL de la papelera como la dirección URL de devolución de llamada. 
1. Guarde la regla.

Ahora, cuando se desencadena la regla, debería ver que aparece una nueva solicitud en RequestBin.

## <a name="payload"></a>Carga
Cuando se desencadena una regla, se realiza una solicitud HTTP POST a la dirección URL de devolución de llamada que contiene una carga útil JSON con los detalles de las medidas, del dispositivo, de la regla y de la aplicación. Para una regla de telemetría, la carga útil puede ser similar a la siguiente:

```json
{
    "id": "ID",
    "timestamp": "date-time",
    "device" : {
        "id":"ID",
        "name":  "Refrigerator1",
        "simulated" : true,
        "deviceId": "deviceID",
        "deviceTemplate":{
            "id": "ID",
            "version":"1.0.0"
        },
        "properties":{
            "device":{
                "firmwareversion":"1.0"
            },
            "cloud":{
                "location":"One Microsoft Way"
            }
        },
        "measurements":{
            "telemetry":{
                "temperature":20,
                "pressure":10
            }
        }

    },
    "rule": {
        "id": "ID",
        "name": "High temperature alert",
        "enabled": true,
        "deviceTemplate": {
            "id":"GUID",
            "version":"1.0.0"
        }
    },
    "application": {
        "id": "ID",
        "name": "Contoso app",
        "subdomain":"contoso-app"
    }
}
```

## <a name="known-limitations"></a>Limitaciones conocidas
Actualmente no hay ningún mecanismo de programación para suscribir o cancelar la suscripción de estos webhooks mediante una API.

Si tiene ideas sobre cómo mejorar esta característica, envíe sus sugerencias a nuestro [foro de UserVoice](https://feedback.azure.com/forums/911455-azure-iot-central).

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido a configurar y usar webhooks, el siguiente paso sugerido es explorar la [creación de flujos de trabajo en Microsoft Flow](howto-add-microsoft-flow.md).
