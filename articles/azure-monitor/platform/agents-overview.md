---
title: Introducción a los agentes de supervisión de Azure | Microsoft Docs
description: En este artículo se proporciona una descripción detallada de los agentes de Azure disponibles que permiten supervisar las máquinas virtuales hospedadas en Azure o el entorno híbrido.
services: azure-monitor
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/15/2019
ms.openlocfilehash: ae799e9a852b8700399ef695c54b3348174b560c
ms.sourcegitcommit: a460fdc19d6d7af6d2b5a4527e1b5c4e0c49942f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77069412"
---
# <a name="overview-of-the-azure-monitor-agents"></a>Introducción a los agentes de Azure Monitor 
Los recursos de proceso, tales como las máquinas virtuales, generan datos para supervisar su rendimiento y disponibilidad, al igual que [otros recursos en la nube](../insights/monitor-azure-resource.md). Los recursos de proceso, sin embargo, tienen también un sistema operativo invitado y cargas de trabajo que deben supervisarse. La recopilación de estos datos de supervisión desde dentro del recurso requiere un agente. En este artículo se describen los agentes utilizados por Azure Monitor y se ayuda a determinar qué se necesita para cumplir con los requisitos del entorno concreto.

## <a name="summary-of-agents"></a>Resumen de los agentes

> [!NOTE]
> Actualmente, Azure Monitor tiene varios agentes, debido a la reciente consolidación de Azure Monitor y Log Analytics. Ambos agentes comparten algunas funcionalidades, mientras que otras son exclusivas de un agente concreto. En función de los requisitos, puede que necesite uno de los agentes o ambos. 

Azure Monitor tiene tres agentes, cada uno de los cuales proporciona una funcionalidad específica. En función de sus requisitos, puede instalar un solo agente o varios en las máquinas virtuales y en otros recursos de proceso.

* [Extensión de Diagnósticos de Azure](#azure-diagnostic-extension)
* [Agente de Log Analytics](#log-analytics-agent)
* [Dependency Agent](#dependency-agent)

En la tabla siguiente se muestra una comparación rápida de los distintos agentes. Consulte en el resto de este artículo más información sobre cada uno de ellos.

| | Extensión Azure Diagnostics | Agente de Log Analytics | Dependency Agent |
|:---|:---|:---|:---|
| Entornos compatibles | Azure | Azure<br>Otra nube<br>Local | Azure<br>Otra nube<br>Local |
| Sistemas operativos | Windows<br>Linux | Windows<br>Linux | Windows<br>Linux
| Dependencias de agente  | None | None | Requiere el agente de Log Analytics |
| Datos recopilados | Registros de eventos<br>Eventos de ETW<br>syslog<br>Rendimiento<br>Registros IIS<br>Registros de salida de seguimiento de aplicación de .NET<br>Volcados de memoria | Registros de eventos<br>syslog<br>Rendimiento<br>Registros IIS<br>Registros personalizados<br>Datos de soluciones | Detalles y dependencias de proceso<br>Métricas de conexión de red |
| Envío de datos | Azure Storage<br>Métricas de Azure Monitor<br>Centro de eventos | Registros de Azure Monitor | Registros de Azure Monitor |



## <a name="azure-diagnostic-extension"></a>Extensión de diagnóstico de Azure
La [extensión Azure Diagnostics](../../azure-monitor/platform/diagnostics-extension-overview.md) recopila datos de supervisión del sistema operativo invitado y de las cargas de trabajo de los recursos de proceso de Azure. Recopila los datos principalmente en Azure Storage. Puede configurar Azure Monitor para que copie los datos de Storage a un área de trabajo de Log Analytics. También puede recopilar datos de rendimiento del sistema operativo invitado en métricas de Azure Monitor.

La extensión Azure Diagnostics se conoce a menudo como la extensión Azure Diagnostics para Windows (WAD) o Azure Diagnostics para Linux (LAD).


### <a name="scenarios-supported"></a>Escenarios admitidos

Entre los escenarios admitidos por la extensión Azure Diagnostics se incluyen los siguientes:

* Recopilar registros y datos de rendimiento de los recursos de proceso de Azure.
* Archivar los registros y los datos de rendimiento del sistema operativo invitado en Azure Storage.
* Ver los datos de supervisión en Storage mediante una herramienta como el [Explorador de Azure Storage](../../vs-azure-tools-storage-manage-with-storage-explorer.md).
* Recopilar datos de rendimiento en una base de datos de métricas, para aprovechar las características compatibles con las [métricas de Azure Monitor](data-platform-metrics.md), como las [alertas de métricas](../../azure-monitor/platform/alerts-metric-overview.md) casi en tiempo real y el [escalado automático](autoscale-overview.md). 
* Recopilar datos de supervisión de [Storage en un área de trabajo de Log Analytics](azure-storage-iis-table.md), para aprovechar las características compatibles con los [registros de Azure Monitor](data-platform-logs.md#what-can-you-do-with-azure-monitor-logs), como las [consultas de registro](../log-query/log-query-overview.md).
* Enviar datos de supervisión a herramientas de terceros mediante [Azure Event Hubs](diagnostics-extension-stream-event-hubs.md).
* Investigar los problemas de arranque de VM con [diagnósticos de arranque](../../virtual-machines/troubleshooting/boot-diagnostics.md).
* Copiar datos de las aplicaciones que se ejecutan en la máquina virtual [en Application Insights](diagnostics-extension-to-application-insights.md), para integrarlos con la supervisión de otras aplicaciones.

## <a name="log-analytics-agent"></a>Agente de Log Analytics
El [agente de Log Analytics](log-analytics-agent.md) recopila datos de supervisión del sistema operativo invitado y de las cargas de trabajo de las máquinas virtuales de Azure, otros proveedores de nube y el entorno local. Recopila los datos en un área de trabajo de Log Analytics.

El agente de Log Analytics es el mismo agente que se usa en System Center Operations Manager y en los equipos de agente de host múltiple para comunicarse con el grupo de administración y Azure Monitor simultáneamente. Este agente también es necesario en ciertas soluciones de Azure Monitor.

El agente de Log Analytics para Windows a menudo se conoce como Agente de administración de Microsoft (MMA). El agente de Log Analytics para Linux se conoce a menudo como agente de OMS.


### <a name="scenarios-supported"></a>Escenarios admitidos

Entre los escenarios admitidos por el agente de Log Analytics se incluyen los siguientes:

* Recopilar registros y datos de rendimiento de máquinas virtuales en Azure, otros proveedores en la nube y entornos locales. 
* Recopilar datos de supervisión en un área de trabajo de Log Analytics, para aprovechar las características compatibles con los [registros de Azure Monitor](data-platform-logs.md#what-can-you-do-with-azure-monitor-logs), como las [consultas de registro](../log-query/log-query-overview.md).
* Usar las soluciones de supervisión de Azure Monitor, como [Azure Monitor para VM](../insights/vminsights-overview.md), [Azure Monitor para contenedores](../insights/container-insights-overview.md), etc.  
* Administrar la seguridad de las máquinas virtuales mediante [Azure Sentinel](../../sentinel/overview.md), que requiere el agente de Log Analytics.
* Administrar la seguridad de las máquinas virtuales mediante [Azure Security Center](../../security-center/security-center-intro.md), que requiere el agente de Log Analytics.
* Usar las características de [Azure Automation](../../automation/automation-intro.md) para ofrecer una administración completa de las máquinas virtuales de Azure en todo su ciclo de vida.  Entre los ejemplos de características que requieren el agente de Log Analytics se incluyen:
  * [Azure Automation Update Management](../../automation/automation-update-management.md) para las actualizaciones del sistema operativo.
  * [Azure Automation State Configuration](../../automation/automation-dsc-overview.md) para mantener un estado coherente de la configuración.
  * Realice un seguimiento de los cambios de configuración con [Azure Automation Change Tracking and Inventory](../../automation/change-tracking.md).

## <a name="dependency-agent"></a>Dependency Agent
Dependency Agent recopila datos detectados acerca de los procesos que se ejecutan en las dependencias de las máquinas virtuales y los procesos externos. Este agente es necesario para [Service Map](../insights/service-map.md) y para la característica Mapa de [Azure Monitor para VM](../insights/vminsights-overview.md). Dependency Agent requiere el agente de Log Analytics y escribe los datos en un área de trabajo de Log Analytics en Azure Monitor.


## <a name="using-multiple-agents"></a>Uso de varios agentes
Puede tener requisitos específicos que determinen el uso de la extensión Azure Diagnostics o del agente de Log Analytics para una máquina virtual determinada. Por ejemplo, puede que desee usar alertas de métricas, que requieren la extensión Azure Diagnostics. Pero es posible que también desee usar la característica Mapa de Azure Monitor para VM, que requiere Dependency Agent y el agente de Log Analytics. En este caso, puede usar varios agentes; se trata de un escenario común en clientes que requieren funcionalidad de cada uno de ellos.

### <a name="considerations"></a>Consideraciones

- Dependency Agent requiere que el agente de Log Analytics se instale en la misma máquina virtual.
- En las máquinas virtuales de Linux, el agente de Log Analytics debe instalarse antes que la extensión Azure Diagnostics.


## <a name="next-steps"></a>Pasos siguientes

- Consulte [Introducción al agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md) para revisar los requisitos y los métodos admitidos para implementar el agente en máquinas hospedadas en Azure, en su centro de datos o en otro entorno en la nube.

