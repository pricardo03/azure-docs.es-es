---
title: Esquema de alerta común para las alertas de Azure monitor
description: Descripción del esquema de alerta comunes, ¿por qué debería usar y cómo habilitarlo
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: c18227a491478d0d8010761440a54fd088344b39
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776594"
---
# <a name="common-alert-schema"></a>Esquema de alertas comunes

Este artículo describe lo que es el esquema de alerta comunes, las ventajas de usar y cómo habilitarlo.

## <a name="what-is-the-common-alert-schema"></a>¿Qué es el esquema común de alerta?

El esquema común de alerta estandariza la experiencia de consumo de notificaciones de alerta en Azure hoy mismo. Históricamente, los tres tipos de alerta en Azure hoy en día (métrica, registro y registro de actividad) han tenido sus propias plantillas de correo electrónico, webhook esquemas, etcetera. Con el esquema de alerta comunes, ahora puede recibir notificaciones de alerta con un esquema coherente.

Describe cualquier instancia de alerta **el recurso que se vio afectado** y **la causa de la alerta**, y estas instancias se describen en el esquema común en las secciones siguientes:
* **Essentials**: Un conjunto de **estandarizado campos**común en todos los tipos de alerta, que describen **qué recurso** la alerta se encuentra en junto con metadatos adicionales comunes alerta (por ejemplo, gravedad o descripción). 
* **Contexto de alerta**: Un conjunto de campos que describen la **causa principal de la alerta**, con campos que varían **según el tipo de alerta**. Por ejemplo, una alerta de métrica tendría campos como el nombre de la métrica y el valor de métrica en el contexto de alerta, mientras que una alerta de registro de actividad tendría información sobre el evento que generó la alerta. 

Los escenarios de integración típicos que oímos de nuestros clientes implican el enrutamiento de la instancia de alerta al equipo que se trate según algunos pivot (por ejemplo, grupo de recursos), después de que el equipo responsable empieza a trabajar en ella. Con el esquema común de alerta, puede ha estandarizado lógica de enrutamiento con tipos de alerta mediante el aprovechamiento de los campos esenciales, dejando los campos de contexto para los equipos afectados investigar en profundidad.

Esto significa que puede tener menos de integraciones, lo que el proceso de administrar y mantener ellos un _mucho_ tarea más sencilla. Además, solo se detectarán enriquecimientos de carga de alertas futuras (por ejemplo, personalización, enriquecimiento de diagnóstico, etc.) de en el esquema común.

## <a name="what-enhancements-does-the-common-alert-schema-bring"></a>¿Qué mejoras aporta el esquema común de alerta?

El esquema común de alerta se manifestará principalmente en las notificaciones de alerta. Las mejoras que verá son los siguientes:

| . | Mejoras|
|:---|:---|
| sms | Una plantilla SMS coherente para todos los tipos de alerta. |
| Email | Una plantilla de correo electrónico detalladas y coherente, lo que le permite diagnosticar fácilmente los problemas de un vistazo. Profundo: vínculos incrustados para la instancia de alerta en el portal y los recursos afectados Asegúrese de que puede desplazarse rápidamente en el proceso de corrección. |
| Webhook/Logic App/Azure/automatización de la función Runbook | Una estructura JSON coherente para todos los tipos de alerta, lo que permite crear fácilmente las integraciones entre los diferentes tipos de alerta. |

El nuevo esquema también permitirá una experiencia más enriquecida de alerta de consumo en el portal de Azure y Azure mobile app en el futuro inmediato. 

[Más información sobre las definiciones de esquema Webhooks/Logic Apps o Azure funciones/para Runbooks de Automation.](https://aka.ms/commonAlertSchemaDefinitions)

> [!NOTE]
> El esquema común de alerta no admiten las siguientes acciones: Conector ITSM.

## <a name="how-do-i-enable-the-common-alert-schema"></a>¿Cómo habilito el esquema común de alerta?

Puede participar o dejar de participar en el esquema común de la alerta a través de grupos de acciones, en el portal y la API de REST. El botón de alternancia para cambiar al nuevo esquema existe en el nivel de acción. Por ejemplo, debe optar por separado por una acción de correo electrónico y una acción de webhook.

> [!NOTE]
> 1. Los siguientes tipos de alerta admiten el esquema común de forma predeterminada (no participar en requerido):
>     * Alertas de detección inteligente
> 1. Los siguientes tipos de alerta no admiten actualmente el esquema común:
>     * Las alertas generadas por [Azure Monitor para las máquinas virtuales](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)
>     * Las alertas generadas por [Azure Cost Management](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario)

### <a name="through-the-azure-portal"></a>A través del portal de Azure

![Participar en el esquema común de alerta](media/alerts-common-schema/portal-opt-in.png)

1. Abra cualquier existente o una nueva acción en un grupo de acciones. 
1. Seleccione 'Sí' para que el botón de alternancia habilitar el esquema de alerta comunes, como se muestra.

### <a name="through-the-action-groups-rest-api"></a>A través de la API de REST de grupos de acciones

También puede usar el [API de grupos de acción](https://docs.microsoft.com/rest/api/monitor/actiongroups) para participar en el esquema común de alerta. Al realizar la [crear o actualizar](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) llamada API de REST, puede establecer la marca "useCommonAlertSchema" en 'true' (para participar) o 'false' (para dejar de participar) para cualquiera de las siguientes acciones: runbook de correo electrónico/webhook/lógica de aplicación/Azure/automatización de la función.

Por ejemplo, la solicitud siguiente realizado en el cuerpo del [crear o actualizar](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) API de REST hará lo siguiente:

* Habilitar el esquema común de alerta para la acción de correo electrónico "email John Doe"
* Deshabilitar el esquema común de alerta para la acción de correo electrónico "email Jane Smith"
* Habilitar el esquema común de alerta para la acción de webhook "Webhook de ejemplo"

```json
{
  "properties": {
    "groupShortName": "sample",
    "enabled": true,
    "emailReceivers": [
      {
        "name": "John Doe's email",
        "emailAddress": "johndoe@email.com",
        "useCommonAlertSchema": true
      },
      {
        "name": "Jane Smith's email",
        "emailAddress": "janesmith@email.com",
        "useCommonAlertSchema": false
      }
    ],
    "smsReceivers": [
      {
        "name": "John Doe's mobile",
        "countryCode": "1",
        "phoneNumber": "1234567890"
      },
      {
        "name": "Jane Smith's mobile",
        "countryCode": "1",
        "phoneNumber": "0987654321"
      }
    ],
    "webhookReceivers": [
      {
        "name": "Sample webhook",
        "serviceUri": "http://www.example.com/webhook",
        "useCommonAlertSchema": true
      }
    ]
  },
  "location": "Global",
  "tags": {}
}
```





## <a name="next-steps"></a>Pasos siguientes

- [Definiciones de esquema de alerta comunes Webhooks/Logic Apps o Azure funciones/para Runbooks de Automation.](https://aka.ms/commonAlertSchemaDefinitions)



