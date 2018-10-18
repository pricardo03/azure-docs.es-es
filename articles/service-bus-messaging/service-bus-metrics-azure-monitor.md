---
title: Métricas de Azure Service Bus en Azure Monitor (versión preliminar) | Microsoft Docs
description: Use Azure Monitor para supervisar las entidades de Service Bus.
services: service-bus-messaging
documentationcenter: .NET
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/24/2018
ms.author: spelluru
ms.openlocfilehash: 293cde00e53171e848263df8564ec85f273c1a40
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166340"
---
# <a name="azure-service-bus-metrics-in-azure-monitor-preview"></a>Métricas de Azure Service Bus en Azure Monitor (versión preliminar)

Las métricas de Service Bus le permiten conocer el estado de los recursos de la suscripción de Azure. Con un amplio conjunto de datos de métricas, puede evaluar el estado general de los recursos de Service Bus, no solo en el nivel de espacio de nombres, sino también en el nivel de entidad. Estas estadísticas pueden ser importantes, ya que le ayudan a supervisar el estado de Service Bus. Las métricas también pueden ayudarle a solucionar problemas de causa principal sin necesidad de ponerse en contacto con el soporte técnico de Azure.

Azure Monitor proporciona interfaces de usuario unificadas para la supervisión de distintos servicios de Azure. Para obtener más información, vea [Supervisión de Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) y el ejemplo [Retrieve Azure Monitor metrics with .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) (Recuperación de métricas de Azure Monitor con .NET) en GitHub.

> [!IMPORTANT]
> Cuando no se haya realizado ninguna interacción con una entidad durante 2 horas, las métricas empezarán a mostrar "0" como valor hasta que la entidad deje de estar inactiva.

## <a name="access-metrics"></a>Acceso a la métrica

Azure Monitor proporciona varias maneras de tener acceso a las métricas. Puede acceder a las métricas desde [Azure Portal](https://portal.azure.com), o usar las API de Azure Monitor (REST y .NET) y soluciones de análisis como Log Analytics y Event Hubs. Para más información, vea [Datos de supervisión recopilados por Azure Monitor](../monitoring/monitoring-data-collection.md).

De forma predeterminada, las métricas están habilitadas y puede acceder a datos de los últimos 30 días. Si es necesario conservar los datos durante un periodo mayor, se pueden archivar en una cuenta de Azure Storage. Esto se configura en la [configuración de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) de Azure Monitor.

## <a name="access-metrics-in-the-portal"></a>Acceso a métricas del portal

Una vez transcurrido un tiempo, las métricas se pueden supervisar en [Azure Portal](https://portal.azure.com). En el ejemplo siguiente se muestra cómo ver las solicitudes correctas y las solicitudes entrantes en el nivel de cuenta:

![][1]

También puede tener acceso a las métricas directamente a través del espacio de nombres. Para ello, seleccione el espacio de nombres y haga clic en **Métrica (vista previa)**. Para mostrar las métricas filtradas al ámbito de la entidad, seleccione la entidad y haga clic en **Metrics (Peview)** (Métricas [Previsualización]).

![][2]

Para ver métricas que admitan las dimensiones, debe filtrar por el valor de la dimensión que desee.

## <a name="billing"></a>Facturación

El uso de métricas en Azure Monitor es gratuito mientras se encuentre en la versión preliminar. Sin embargo, si usa otras soluciones que ingieren datos de métricas, puede que se le facturen dichas soluciones. Por ejemplo, se le facturará por Azure Storage si archiva datos de métricas en una cuenta de Azure Storage. También se le facturará por Log Analytics si transmite datos de métricas a Log Analytics para realizar análisis avanzados.

Las siguientes métricas ofrecen una visión general del estado de su servicio. 

> [!NOTE]
> Se están dejando de usar algunas métricas a medida que se mueven a otro nombre. Esto podría requerir que actualice las referencias. Las métricas que se marquen con la palabra clave "en desuso" no se admitirán en el futuro.

Los valores de las métricas se envían a Azure Monitor cada minuto. La granularidad de tiempo define el intervalo de tiempo para el que se presentan los valores de las métricas. El intervalo de tiempo compatible para todas las métricas de Service Bus es 1 minuto.

## <a name="request-metrics"></a>Solicitar métricas

Cuenta el número de solicitudes de operaciones de datos y administración.

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
| Solicitudes entrantes (versión preliminar) | Número de solicitudes realizadas al servicio de Service Bus durante un período determinado. <br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|
|Solicitudes correctas (versión preliminar)|Número de solicitudes correctas realizadas al servicio de Service Bus durante un período determinado.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|
|Errores del servidor (versión preliminar)|Número de solicitudes no procesadas debido a un error en el servicio de Service Bus durante un período determinado.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|
|Errores de usuario (versión preliminar: vea la subsección siguiente)|Número de solicitudes no procesadas debido a errores de usuario durante un período determinado.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|
|Solicitudes limitadas (versión preliminar)|Número de solicitudes que se han limitado porque se superó el uso.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|

### <a name="user-errors"></a>Errores de usuario

Los dos tipos de errores siguientes se clasifican como errores de usuario:

1. Errores en el cliente (en HTTP, serían errores 400).
2. Errores que se producen al procesar los mensajes, como [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception).


## <a name="message-metrics"></a>Métricas de mensaje

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
|Mensajes entrantes (versión preliminar)|Número de eventos o mensajes enviados a Service Bus durante un período determinado.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|
|Mensajes salientes (versión preliminar)|Número de eventos o mensajes recibidos de Service Bus durante un período determinado.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|

## <a name="connection-metrics"></a>Métricas de conexión

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
|ActiveConnections (versión preliminar)|Número de conexiones activas en un espacio de nombres, así como en una entidad.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|
|Conexiones abiertas (versión preliminar)|Número de conexiones abiertas.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName|
|Conexiones cerradas (versión preliminar)|Número de conexiones cerradas.<br/><br/> Unidad: recuento <br/> Tipo de agregación: total <br/> Dimensión: EntityName |

## <a name="resource-usage-metrics"></a>Métricas de uso de recursos

> [!NOTE] 
> Estas métricas están disponibles solo con el nivel **Premium**. 

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
|Uso de CPU por espacio de nombres (versión preliminar)|Porcentaje de uso de CPU del espacio de nombres.<br/><br/> Unidad: porcentaje <br/> Tipo de agregación: máxima <br/> Dimensión: EntityName|
|Uso de tamaño de memoria por espacio de nombres (versión preliminar)|Porcentaje de uso de memoria del espacio de nombres.<br/><br/> Unidad: porcentaje <br/> Tipo de agregación: máxima <br/> Dimensión: EntityName|

## <a name="metrics-dimensions"></a>Dimensiones de métricas

Azure Service Bus admite las siguientes dimensiones para las métricas de Azure Monitor. La adición de dimensiones a las métricas es opcional. Si no agrega dimensiones, las métricas se especifican en el nivel de espacio de nombres. 

|Nombre de dimensión|DESCRIPCIÓN|
| ------------------- | ----------------- |
|EntityName| Service Bus admite las entidades de mensajería en el espacio de nombres.|

## <a name="next-steps"></a>Pasos siguientes

Vea [Información general sobre la supervisión en Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


