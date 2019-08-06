---
title: Métricas de Azure Event Hubs en Azure Monitor | Microsoft Docs
description: En este artículo se proporciona información sobre cómo usar Azure Monitoring para supervisar Azure Event Hubs.
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 4caa9e78d72df37bec2351fa86ae6e60630d6047
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370770"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor"></a>Métricas de Azure Event Hubs en Azure Monitor

Las métricas de Event Hubs le permiten conocer el estado de los recursos de Event Hubs en su suscripción de Azure. Con un amplio conjunto de datos de métricas, puede evaluar el estado general de los centros de eventos, no solo en el nivel de espacio de nombres, sino también en el nivel de entidad. Estas estadísticas pueden ser importantes, ya que le ayudan a supervisar el estado de los centros de eventos. Las métricas también pueden ayudarle a solucionar problemas de causa principal sin necesidad de ponerse en contacto con el soporte técnico de Azure.

Azure Monitor proporciona interfaces de usuario unificadas para la supervisión de distintos servicios de Azure. Para obtener más información, vea [Supervisión de Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) y el ejemplo [Retrieve Azure Monitor metrics with .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) (Recuperación de métricas de Azure Monitor con .NET) en GitHub.

## <a name="access-metrics"></a>Acceso a la métrica

Azure Monitor proporciona varias maneras de tener acceso a las métricas. Puede acceder a las métricas desde [Azure Portal](https://portal.azure.com), o usar las API de Azure Monitor (REST y .NET) y soluciones de análisis como Operation Management Suite y Event Hubs. Para más información, vea [Datos de supervisión recopilados por Azure Monitor](../azure-monitor/platform/data-platform.md).

De forma predeterminada, las métricas están habilitadas y puede acceder a datos de los últimos 30 días. Si es necesario conservar los datos durante un periodo mayor, se pueden archivar en una cuenta de Azure Storage. Esto se configura en la [configuración de diagnóstico](../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings) de Azure Monitor.

## <a name="access-metrics-in-the-portal"></a>Acceso a métricas del portal

Una vez transcurrido un tiempo, las métricas se pueden supervisar en [Azure Portal](https://portal.azure.com). En el ejemplo siguiente se muestra cómo ver las solicitudes correctas y las solicitudes entrantes en el nivel de cuenta:

![Ver las métricas de éxito][1]

También puede tener acceso a las métricas directamente a través del espacio de nombres. Para ello, seleccione el espacio de nombres y, después, haga clic en **Métricas**. Para mostrar las métricas filtradas al ámbito del centro de eventos, seleccione el centro de eventos y, después, haga clic en **Métricas**.

Para ver métricas que admitan las dimensiones, debe filtrar por el valor de la dimensión que desee como se muestra en el siguiente ejemplo:

![Filtrar por valor de dimensión][2]

## <a name="billing"></a>Facturación

Actualmente, el uso de métricas en Azure Monitor es gratuito. Sin embargo, si usa otras soluciones que ingieren datos de métricas, puede que se le facturen dichas soluciones. Por ejemplo, se le facturará por Azure Storage si archiva datos de métricas en una cuenta de Azure Storage. También se le facturará por Azure si transmite datos de métricas a registros de Azure Monitor para realizar análisis avanzados.

Las siguientes métricas ofrecen una visión general del estado de su servicio. 

> [!NOTE]
> Se están dejando de usar algunas métricas a medida que se mueven a otro nombre. Esto podría requerir que actualice las referencias. Las métricas que se marquen con la palabra clave "en desuso" no se admitirán en el futuro.

Los valores de las métricas se envían a Azure Monitor cada minuto. La granularidad de tiempo define el intervalo de tiempo para el que se presentan los valores de las métricas. El intervalo de tiempo compatible para todas las métricas de Event Hubs es 1 minuto.

## <a name="request-metrics"></a>Solicitud de métricas

Cuenta el número de solicitudes de operaciones de datos y administración.

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
| Solicitudes entrantes  | Número de solicitudes realizadas al servicio de Azure Event Hubs durante un período determinado. <br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName |
| Solicitudes correctas    | Número de solicitudes correctas realizadas al servicio de Azure Event Hubs durante un período determinado. <br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName |
| Errores del servidor  | Número de solicitudes no procesadas debido a un error en el servicio de Azure Event Hubs durante un período determinado. <br/><br/>Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName |
|Errores de usuario |Número de solicitudes no procesadas debido a errores de usuario durante un período determinado.<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|
|Cuota de errores superada |El número de solicitudes que superó la cuota disponible. Para más información sobre las cuotas de Event Hubs, vea [este artículo](event-hubs-quotas.md).<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|

## <a name="throughput-metrics"></a>Métricas de procesamiento

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
|Solicitudes limitadas |Número de solicitudes que se han limitado porque se superó el uso de la unidad de procesamiento.<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|

## <a name="message-metrics"></a>Métricas de mensaje

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
|Mensajes entrantes |Número de eventos o mensajes enviados a Event Hubs durante un período determinado.<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|
|Mensajes salientes |Número de eventos o mensajes recuperados de Event Hubs durante un período determinado.<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|
|Bytes entrantes |El número de bytes enviado al servicio de Azure Event Hubs durante un período determinado.<br/><br/> Unidad: Bytes <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|
|Bytes salientes |El número de bytes recuperado del servicio de Azure Event Hubs durante un período determinado.<br/><br/> Unidad: Bytes <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|

## <a name="connection-metrics"></a>Métricas de conexión

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
|ActiveConnections |Número de conexiones activas en un espacio de nombres, así como en una entidad.<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|
|Conexiones abiertas |Número de conexiones abiertas.<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|
|Conexiones cerradas |Número de conexiones cerradas.<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|

## <a name="event-hubs-capture-metrics"></a>Métricas de captura de Event Hubs

Puede supervisar las métricas de captura de Event Hubs cuando se habilita la característica de captura de los centros de eventos. Las métricas siguientes describen lo que puede supervisar con la característica de captura habilitada.

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
|Capture el trabajo pendiente |El número de bytes que se aún se tiene que capturar en el destino elegido.<br/><br/> Unidad: Bytes <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|
|Mensajes capturados |El número de mensajes o eventos que se capturan en el destino elegido durante un período determinado.<br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|
|Bytes capturados |El número de bytes que se capturan en el destino elegido durante un período determinado.<br/><br/> Unidad: Bytes <br/> Tipo de agregación: Total <br/> Dimensión: EntityName|

## <a name="metrics-dimensions"></a>Dimensiones de métricas

Azure Event Hubs admite las siguientes dimensiones para las métricas de Azure Monitor. La adición de dimensiones a las métricas es opcional. Si no agrega dimensiones, las métricas se especifican en el nivel de espacio de nombres. 

| Nombre de métrica | DESCRIPCIÓN |
| ------------------- | ----------------- |
|EntityName| Event Hubs admite las entidades de concentrador de eventos en el espacio de nombres.|

## <a name="next-steps"></a>Pasos siguientes

* Vea [Información general sobre la supervisión en Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md).
* Ejemplo [Retrieve Azure Monitor metrics with .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) (Recuperación de métricas de Azure Monitor con .NET) en GitHub. 

Para obtener más información acerca de Event Hubs, visite los vínculos siguientes:

* Empiece por un [tutorial de Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)
* [Aplicaciones de ejemplo que usan Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



