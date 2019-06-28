---
title: Ejecución de varias acciones de una regla de Azure IoT Central | Microsoft Docs
description: Ejecute varias acciones desde una sola regla de IoT Central y cree grupos de acciones reutilizables que puede ejecutar desde varias reglas.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/19/2019
ms.topic: conceptual
ms.service: iot-central
manager: philmea
ms.openlocfilehash: 857d747fa691d1ec2b386d5931a7edea08b7e609
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60517203"
---
# <a name="group-multiple-actions-to-run-from-one-or-more-rules"></a>Agrupación de varias acciones para ejecutar desde una o varias reglas

*Este artículo se aplica a los compiladores y administradores.*

En Azure IoT Central, cree reglas para que se ejecuten acciones cuando se cumpla una condición. Las reglas se basan en la telemetría de dispositivo o en los eventos. Por ejemplo, puede notificar a un operador cuando la temperatura en un dispositivo supera un umbral. En este artículo se describe cómo usar *grupos de acciones* de [Azure Monitor](../azure-monitor/overview.md) para asociar varias acciones a una regla de IoT Central. Puede asociar un grupo de acciones a varias reglas. Un [grupo de acciones](../azure-monitor/platform/action-groups.md) es una colección de las preferencias de notificación que el propietario de una suscripción de Azure define.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de pago por uso
- Una suscripción y cuenta de Azure para crear y administrar grupos de acciones de Azure Monitor

## <a name="create-action-groups"></a>Creación de grupos de acciones

También puede [crear y administrar grupos de acciones en Azure Portal](../azure-monitor/platform/action-groups.md) o con un [plantilla de Azure Resource Manager](../azure-monitor/platform/action-groups-create-resource-manager-template.md).

Un grupo de acciones puede:

- Enviar notificaciones, como un correo electrónico o SMS, o hacer una llamada de voz.
- Ejecutar de una acción, como llamar a un webhook.

La siguiente captura de pantalla muestra un grupo de acciones que envía notificaciones por correo electrónico y SMS y llama a un webhook:

![Grupo de acciones](media/howto-use-action-groups/actiongroup.png)

Para usar un grupo de acciones en una regla de IoT Central, dicho grupo debe estar en la misma suscripción de Azure que la aplicación IoT Central.

## <a name="use-an-action-group"></a>Uso de un grupo de acciones

Para usar un grupo de acciones en la aplicación IoT Central, primero cree una regla de telemetría o evento. Cuando agregue una acción a la regla, seleccione **Grupos de acciones de Azure Monitor**:

![Elegir acción](media/howto-use-action-groups/chooseaction.png)

Elija un grupo de acciones en su suscripción de Azure:

![Elección de un grupo de acciones](media/howto-use-action-groups/chooseactiongroup.png)

Seleccione **Guardar**. El grupo de acciones ahora aparece en la lista de acciones que se ejecuta cuando se desencadena la regla:

![Grupo de acciones guardado](media/howto-use-action-groups/savedactiongroup.png)

En la tabla siguiente se resume la información enviada a los tipos de acciones admitidas:

| Tipo de acción | Formato de salida |
| ----------- | -------------- |
| Email       | Plantilla de correo electrónico estándar de IoT Central |
| sms         | Alerta de Azure IoT Central: ${applicationName} - "${ruleName}" desencadenada en "${deviceName}" el ${triggerDate} ${triggerTime} |
| Voz       | Alerta de Azure I.O.T Central: la regla "${ruleName}" se desencadenó en el dispositivo "${deviceName}" el ${triggerDate} ${triggerTime}, en la aplicación ${applicationName} |
| webhook     | {"schemaId": "AzureIoTCentralRuleWebhook", "data": {[carga de webhook regular](#payload)}} |

El texto siguiente es un mensaje SMS de ejemplo de un grupo de acciones:

`iotcentral: Azure IoT Central alert: Sample Contoso 22xu4spxjve - "Low pressure alert" triggered on "Refrigerator 2" at March 20, 2019 10:12 UTC`

<a id="payload"></a> El siguiente código JSON muestra una carga útil de acción de webhook de ejemplo:

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

Ahora que ha aprendido a usar grupos de acciones con reglas, el siguiente paso que se recomienda es aprender a [administrar los dispositivos](howto-manage-devices.md).
