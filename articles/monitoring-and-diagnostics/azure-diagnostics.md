---
title: Introducción a la extensión Azure Diagnostics
description: Use Diagnósticos de Azure para realizar tareas de depuración, medición de rendimiento, supervisión y análisis de tráfico en servicios en la nube, en máquinas virtuales y en Service Fabric
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: ef1422db799db6d635ad9f03908e3a34f312e408
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974245"
---
# <a name="what-is-azure-diagnostics-extension"></a>Qué es la extensión Azure Diagnostics
La extensión Azure Diagnostics es un agente dentro de Azure que habilita la recopilación de datos de diagnóstico en una aplicación implementada. Puede utilizar la extensión de diagnóstico desde un número de orígenes diferentes. Actualmente se admiten los roles web y de trabajo de Azure Cloud Services, las máquinas virtuales, los conjuntos de escalado de máquinas virtuales y Service Fabric. Otros servicios de Azure tienen métodos de diagnóstico diferentes. Consulte [Introducción a la supervisión en Azure](monitoring-overview.md).

## <a name="linux-agent"></a>Agente Linux
Una [versión Linux de la extensión](../virtual-machines/linux/diagnostic-extension.md) está disponible para máquinas virtuales que ejecutan Linux. Las estadísticas recopiladas y los comportamientos varían frente a la versión Windows.

## <a name="data-you-can-collect"></a>Datos que puede recopilar
La extensión Azure Diagnostics puede recopilar los siguientes tipos de datos:

| Origen de datos | DESCRIPCIÓN |
| --- | --- |
| contadores de rendimiento |Sistema operativo y contadores de rendimiento personalizados |
| Registros de aplicación |Seguimiento de mensajes escritos por la aplicación |
| Registros de eventos de Windows |Información enviada al sistema de registro de eventos de Windows |
| Origen de eventos de .NET |Eventos de escritura de código mediante la clase [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) de .NET |
| Registros IIS |Información sobre los sitios web de IIS |
| ETW basado en manifiesto |Eventos de Seguimiento de eventos para Windows generados por cualquier proceso.(1) |
| Volcados de memoria |Información sobre el estado del proceso en caso de bloqueo de una aplicación |
| Registros de errores personalizados |Registros creados por su aplicación o servicio |
| Registros de infraestructura de diagnóstico de Azure |Información sobre Diagnósticos |

(1) Para obtener una lista de proveedores de ETW, ejecute `c:\Windows\System32\logman.exe query providers` en una ventana de la consola en el equipo del cual quiere recopilar información.

## <a name="data-storage"></a>Almacenamiento de datos
La extensión almacena sus datos en una [cuenta de Azure Storage](azure-diagnostics-storage.md) que especifique.

También puede enviarlos a [Application Insights](../application-insights/app-insights-cloudservices.md). Otra opción es transmitirlos al [Centro de eventos](../event-hubs/event-hubs-what-is-event-hubs.md), que le permite enviarlos a servicios de supervisión que no son de Azure.

### <a name="azure-monitor"></a>Azure Monitor
También tiene la opción de enviar los datos a Azure Monitor. En este momento, este receptor solo se puede aplicar a los contadores de rendimiento. Permite enviar a Azure Monitor los contadores de rendimiento recopilados en la máquina virtual, VMSS o en el servicio en la nube como métricas personalizadas. El receptor de Azure Monitor admite:
* La recuperación de todos los contadores de rendimiento que se envían a Azure Monitor a través de las [API de métricas de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/).
* Las alertas de todos los contadores de rendimiento que se envían a Azure Monitor a través de la nueva [experiencia unificada de alertas](monitoring-overview-unified-alerts.md) de Azure Monitor.
* El tratamiento del operador comodín de los contadores de rendimiento como la dimensión "Instancia" de la métrica.  Por ejemplo, si recopiló el contador de "LogicalDisk(\*)/DiskWrites/sec", debería poder filtrar y dividir en la dimensión "Instancia" para el trazado o una alerta en las escrituras en disco/s para cada disco lógico de la máquina virtual (C:, D:, etc.).

Para obtener más información sobre cómo configurar este receptor, consulte la [documentación del esquema de diagnósticos de Azure](azure-diagnostics-schema-1dot3-and-later.md).

## <a name="versioning-and-configuration-schema"></a>Esquema de configuración y control de versiones
Consulte [Historial y versiones del esquema de configuración de la extensión Azure Diagnostics](azure-diagnostics-versioning-history.md).


## <a name="next-steps"></a>Pasos siguientes
Seleccione el servicio sobre el cual intenta recopilar diagnósticos y utilice los siguientes artículos para comenzar. Utilice los vínculos de diagnósticos generales de Azure como referencia para tareas específicas.

## <a name="cloud-services-using-azure-diagnostics"></a>Cloud Services que usan Diagnósticos de Azure
* Si utiliza Visual Studio, consulte el artículo sobre el [uso de Visual Studio para rastrear una aplicación de Cloud Services](../vs-azure-tools-debug-cloud-services-virtual-machines.md) para comenzar. De lo contrario, consulte
* [Supervisión de Cloud Services con Diagnósticos de Azure](../cloud-services/cloud-services-how-to-monitor.md)
* [Configuración de Diagnósticos de Azure una aplicación de Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md)

Para temas más avanzados, consulte

* [Uso de Diagnósticos de Azure con Application Insights para Cloud Services](../application-insights/app-insights-cloudservices.md)
* [Seguimiento del flujo en una aplicación de Cloud Services con Diagnósticos de Azure](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Uso de PowerShell para configurar los diagnósticos en Cloud Services](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtual Machines
* Si utiliza Visual Studio, consulte el artículo sobre el [uso de Visual Studio para rastrear Azure Virtual Machines](../vs-azure-tools-debug-cloud-services-virtual-machines.md) para comenzar. De lo contrario, consulte
* [Configuración de Diagnósticos de Azure en Azure Virtual Machine](../virtual-machines-dotnet-diagnostics.md)

Para temas más avanzados, consulte

* [Uso de PowerShell para configurar los diagnósticos en Azure Virtual Machines](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Creación de una máquina virtual de Windows con supervisión y diagnóstico mediante una plantilla de Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Empiece por el artículo sobre la [supervisión de una aplicación de Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Cuando acceda a este artículo, verá muchos otros artículos de diagnósticos de Service Fabric disponibles en el árbol de navegación de la izquierda.

## <a name="general-articles"></a>Artículos generales
* Aprenda a [utilizar los contadores de rendimiento en Diagnósticos de Azure](../cloud-services/diagnostics-performance-counters.md).
* Si tiene problemas al iniciar los diagnósticos o al buscar datos en las tablas de Azure Storage, consulte [Solución de problemas de Azure Diagnostics](azure-diagnostics-troubleshooting.md).
