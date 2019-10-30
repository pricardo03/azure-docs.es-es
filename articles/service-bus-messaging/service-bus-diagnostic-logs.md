---
title: Registros de diagnóstico de Azure Service Bus | Microsoft Docs
description: Aprenda cómo configurar registros de diagnóstico para Service Bus en Azure.
keywords: ''
documentationcenter: .net
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 41e0bdc1f04c9491ebe939f46b59ae4eb2bc7ab6
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72592463"
---
# <a name="enable-diagnostic-logs-for-service-bus"></a>Habilitar los registros de diagnóstico para Service Bus

Cuando empiece a usar el espacio de nombres de Azure Service Bus, puede que desee supervisar cómo y cuándo se crea, elimina o accede al espacio de nombres. En este artículo se proporciona información general de todos los registros operativos y de diagnóstico que están disponibles.

Actualmente, Azure Service Bus admite registros de actividades y registros operativos que capturan **operaciones de administración** realizadas en el espacio de nombres de Azure Service Bus. En concreto, estos registros capturan el tipo de operación, incluida la creación de colas, los recursos usados y el estado de la operación.

## <a name="operational-logs-schema"></a>Esquema de registros operativos

Todos los registros se almacenan en el formato de notación de objetos JavaScript (JSON) en las dos ubicaciones siguientes.

- **AzureActivity**: muestra registros de operaciones o acciones realizadas en el espacio de nombres del portal o mediante implementaciones de plantillas de Azure Resource Manager.
- **AzureDiagnostics**: muestra registros de operaciones o acciones realizadas en el espacio de nombres mediante la API o mediante clientes de administración en el SDK de lenguaje.

Las cadenas JSON de registros operativos incluyen elementos enumerados en la tabla siguiente:

| NOMBRE | Descripción |
| ------- | ------- |
| ActivityId | Identificador interno que se usa para identificar la actividad especificada. |
| EventName | Nombre de la operación |
| ResourceId | El identificador de recursos de Azure Resource Manager |
| SubscriptionId | Id. de suscripción |
| EventTimeString | Hora de la operación |
| EventProperties | Propiedades de la operación |
| Status | Estado de la operación |
| Autor de llamada | Autor de la llamada de la operación (Azure Portal o Management Client) |
| Category | OperationalLogs |

Este es un ejemplo de una cadena JSON de registro operativo:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="what-eventsoperations-are-captured-in-operational-logs"></a>¿Qué eventos y operaciones se capturan en los registros operativos?

Los registros operativos capturan todas las operaciones de administración realizadas en el espacio de nombres de Azure Service Bus. Las operaciones de datos no se capturan debido al elevado volumen de este tipo de operaciones que se realizan en Azure Service Bus.

> [!NOTE]
> Para realizar un seguimiento mejorado de las operaciones de datos, se recomienda usar el seguimiento en el lado cliente.

Las siguientes operaciones de administración se capturan en los registros operativos: 

| Ámbito | Operación|
|-------| -------- |
| Espacio de nombres | <ul> <li> Crear espacio de nombres</li> <li> Actualizar espacio de nombres </li> <li> Eliminar espacio de nombres </li>  </ul> | 
| Cola | <ul> <li> Crear cola</li> <li> Actualizar cola</li> <li> Eliminar cola </li> </ul> | 
| Tema | <ul> <li> Crear tema </li> <li> Actualizar tema </li> <li> Eliminar tema </li> </ul> |
| Subscription | <ul> <li> Creación de suscripción </li> <li> Actualizar suscripción </li> <li> Eliminar suscripción </li> </ul> |

> [!NOTE]
> Actualmente, no se realiza un seguimiento de las operaciones de **lectura** en los registros operativos.

## <a name="how-to-enable-operational-logs"></a>¿Cómo se habilitan los registros operativos?

Los registros operativos están deshabilitados de forma predeterminada. Para habilitarlos, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), vaya al espacio de nombres de Azure Service Bus y en **Supervisión** haga clic en **Configuración de diagnóstico**.

   ![navegación por la hoja a los registros de diagnósticos](./media/service-bus-diagnostic-logs/image1.png)

2. Haga clic en **Agregar configuración de diagnóstico** para configurar los valores de diagnóstico.  

   ![activar los registros de diagnósticos](./media/service-bus-diagnostic-logs/image2.png)

3. Configuración de los valores de diagnóstico
   1. Escriba un **nombre** para identificar la configuración de diagnóstico.
   2. Seleccione un destino para el diagnóstico.
      - Si elige **Cuenta de almacenamiento**, tiene que configurar la cuenta de almacenamiento donde se almacenarán los diagnósticos.
      - Si elige **Centros de eventos**, tiene que configurar el centro de eventos adecuado al que se transmitirá la configuración de diagnóstico.
      - Si elige **Log Analytics**, tiene que especificar a qué instancia de Log Analytics se enviarán los diagnósticos.
    3. Compruebe **OperationalLogs**.

       ![cambiar el estado de los registros de diagnósticos](./media/service-bus-diagnostic-logs/image3.png)

4. Haga clic en **Save**(Guardar).


La nueva configuración surte efecto en unos 10 minutos. Después, los registros aparecen en el destino de archivo configurado, en la hoja **Registros de diagnóstico**.

Para obtener más información sobre el diagnóstico de configuraciones, consulte la [información general sobre los registros de diagnóstico de Azure](../azure-monitor/platform/diagnostic-logs-overview.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes vínculos para más información acerca de Service Bus:

* [Introducción a Service Bus](service-bus-messaging-overview.md)
* [Introducción a Service Bus](service-bus-dotnet-get-started-with-queues.md)
