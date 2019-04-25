---
title: Ejecutar varias acciones de una regla de Azure IoT Central | Microsoft Docs
description: Ejecutar varias acciones de una sola regla IoT Central y crear grupos reutilizables de acciones que se pueden ejecutar desde varias reglas.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/19/2019
ms.topic: conceptual
ms.service: iot-central
manager: philmea
ms.openlocfilehash: 857d747fa691d1ec2b386d5931a7edea08b7e609
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60517203"
---
# <a name="group-multiple-actions-to-run-from-one-or-more-rules"></a>Agrupar varias acciones para ejecutar desde una o varias reglas

*En este artículo se aplica a los creadores y administradores.*

En Azure IoT Central, cree reglas para ejecutar acciones cuando se cumple una condición. Las reglas se basan en la telemetría de dispositivo o eventos. Por ejemplo, puede notificar a un operador cuando la temperatura en un dispositivo supera un umbral. En este artículo se describe cómo usar [Azure Monitor](../azure-monitor/overview.md) *grupos de acciones* para asociar múltiples acciones a una regla de IoT Central. Puede asociar un grupo de acciones para varias reglas. Un [grupo de acciones](../azure-monitor/platform/action-groups.md) es una colección de las preferencias de notificación definido por el propietario de una suscripción de Azure.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de pago por uso
- Una cuenta de Azure y una suscripción para crear y administrar grupos de acciones de Azure Monitor

## <a name="create-action-groups"></a>Creación de grupos de acciones

También puede [crear y administrar grupos de acciones en el portal de Azure](../azure-monitor/platform/action-groups.md) o con un [plantilla de Azure Resource Manager](../azure-monitor/platform/action-groups-create-resource-manager-template.md).

Un grupo de acciones puede:

- Enviar notificaciones, como un correo electrónico, SMS, o llamar a una voz.
- Ejecución de una acción, como llamar a un webhook.

Captura de pantalla siguiente muestra un grupo de acciones que envía notificaciones de SMS y correo electrónico y llama a un webhook:

![Grupo de acciones](media/howto-use-action-groups/actiongroup.png)

Para usar un grupo de acciones en una regla de IoT Central, debe ser el grupo de acciones en la misma suscripción de Azure que la aplicación IoT Central.

## <a name="use-an-action-group"></a>Usar un grupo de acciones

Para usar un grupo de acciones en la aplicación IoT Central, en primer lugar cree una regla de evento o datos de telemetría. Al agregar una acción a la regla, seleccione **grupos de acciones de Azure Monitor**:

![Elegir acción](media/howto-use-action-groups/chooseaction.png)

Elija un grupo de acciones en su suscripción de Azure:

![Elija el grupo de acciones](media/howto-use-action-groups/chooseactiongroup.png)

Seleccione **Guardar**. El grupo de acciones aparece ahora en la lista de acciones que se ejecuta cuando se desencadena la regla:

![Guarda el grupo de acciones](media/howto-use-action-groups/savedactiongroup.png)

En la tabla siguiente se resume la información enviada a los tipos de acciones admitidas:

| Tipo de acción | Formato de salida |
| ----------- | -------------- |
| Email       | Plantilla de correo electrónico estándar de IoT Central |
| sms         | Alerta de Azure IoT Central: ${applicationName} - "${ruleName}" se desencadena en "${deviceName}" en {triggerDate} ${triggerTime} |
| Voz       | Alerta de Azure I.O.T Central: regla de "${ruleName}" se desencadena en el dispositivo "${deviceName}" en {triggerDate} ${triggerTime}, en la aplicación ${applicationName} |
| webhook     | {"schemaId": "AzureIoTCentralRuleWebhook", "data": {[carga de webhook regular](#payload)}} |

El texto siguiente es un mensaje SMS de ejemplo de un grupo de acciones:

`iotcentral: Azure IoT Central alert: Sample Contoso 22xu4spxjve - "Low pressure alert" triggered on "Refrigerator 2" at March 20, 2019 10:12 UTC`

<a id="payload"></a> El siguiente JSON muestra una carga de acción de webhook de ejemplo:

```json
{
  "schemaId":"AzureIoTCentralRuleWebhook",
  "data":{
    "id":"97ae27c4-17c5-4e13-9248-65c7a2c57a1b",
    "timestamp":"2019-03-20T10:53:17.059Z",
    "rule":{
      "id":"031b660e-528d-47bb-b33d-f1158d7e31bf",
      "name":"Low pressure alert",
      "enabled":true,
      "deviceTemplate":{
        "id":"c318d580-39fc-4aca-b995-843719821049",
        "version":"1.0.0"
      }
    },
    "device":{
      "id":"2383d8ba-c98c-403a-b4d5-8963859643bb",
      "name":"Refrigerator 2",
      "simulated":true,
      "deviceId":"2383d8ba-c98c-403a-b4d5-8963859643bb",
      "deviceTemplate":{
        "id":"c318d580-39fc-4aca-b995-843719821049",
        "version":"1.0.0"
      },
      "measurements":{
        "telemetry":{
           "pressure":343.269190673549
        }
      }
    },
    "application":{
      "id":"8e70742b-0d5c-4a1d-84f1-4dfd42e61c7b",
      "name":"Sample Contoso",
      "subdomain":"sample-contoso"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar grupos de acciones con las reglas, el siguiente paso sugerido es obtener información sobre cómo [administrar los dispositivos](howto-manage-devices.md).
