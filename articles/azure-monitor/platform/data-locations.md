---
title: Supervisión de las ubicaciones de los datos en Azure Monitor | Microsoft Docs
description: Describe las diferentes ubicaciones donde los datos de supervisión se almacenan en Azure e incluye la plataforma de datos de Azure Monitor.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: bwren
ms.openlocfilehash: 61f34277a03deb312e93920e3bd76ce5297f020f
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357538"
---
# <a name="monitoring-data-locations-in-azure-monitor"></a>Supervisión de las ubicaciones de los datos en Azure Monitor

Azure Monitor se basa en un [plataforma de datos](data-platform.md) de [registros](data-platform-logs.md) y [métricas](data-platform-metrics.md) como se describe en [plataforma de datos de Azure Monitor](data-platform.md). Datos de supervisión de recursos de Azure pueden escribirse en otras ubicaciones, sin embargo, ya sea antes de que se copian en Azure Monitor o para admitir escenarios adicionales. 

## <a name="monitoring-data-locations"></a>Ubicaciones de los datos de supervisión

En la tabla siguiente identifica las diferentes ubicaciones donde se envían datos de supervisión en Azure y los distintos métodos para tener acceso a él.

| Ubicación | DESCRIPCIÓN | Métodos de acceso |
|:---|:---|:---|:--|
| Métricas en Microsoft Azure | Base de datos de serie temporal que está optimizado para analizar los datos con marca de tiempo. | [Explorador de métricas](metrics-getting-started.md)<br>[API de métricas de Azure Monitor](/rest/api/monitor/metrics) |
| Registros de Azure Monitor    | Área de trabajo de análisis que se basa en el Explorador de datos de Azure que proporciona un lenguaje de consulta enriquecidas y el motor de análisis eficaz de registros. | [Log Analytics](../log-query/portals.md)<br>[API de log Analytics](https://dev.loganalytics.io/)<br>[API de Application Insights](https://dev.applicationinsights.io/reference/get-query) |
| Registro de actividades | Datos del registro de actividad están muy útiles cuando se envía a los registros de Azure Monitor para analizarlo con otros datos, pero también se recopilan por sí solo por lo que pueden verse directamente en el portal de Azure. | [Azure Portal](activity-log-view.md#azure-portal)<br>[API de eventos de Azure Monitor](/rest/api/monitor/eventcategories) |
| Azure Storage | Algunos orígenes de datos escribirá directamente en Azure storage y requieren una configuración para mover datos a los registros. También puede enviar datos al almacenamiento de Azure para archivar y para la integración con sistemas externos.  | [Storage Analytics](/rest/api/storageservices/storage-analytics)<br>[Explorador de servidores](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)<br>[Explorador de Storage](/visualstudio/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) |
| Event Hubs | Enviar datos a Azure Event Hubs para transmitirlo a otras ubicaciones. | [Captura de almacenamiento](../../event-hubs/event-hubs-capture-overview.md)  |
| Azure Monitor para máquinas virtuales | Azure Monitor para las máquinas virtuales almacena los datos de estado de carga de trabajo en una ubicación personalizada que usa su experiencia de supervisión en Azure portal. | [Azure Portal](../insights/vminsights-overview.md)<br>[API de REST de monitor de carga de trabajo](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components)<br>[API de REST de Azure Resource health](https://docs.microsoft.com/rest/api/resourcehealth/)  |
| Alertas | Alertas creadas por Azure Monitor. | [Azure Portal](alerts-managing-alert-instances.md)<br>[API de REST de administración de alertas](https://docs.microsoft.com/rest/api/monitor/alertsmanagement/alerts) |



## <a name="next-steps"></a>Pasos siguientes

- Vea las diferentes fuentes de [datos de supervisión recopilados por Azure Monitor](data-sources.md).
- Obtenga información sobre la [tipos de datos de supervisión recopilados por Azure Monitor](data-platform.md) y cómo ver y analizar los datos.
