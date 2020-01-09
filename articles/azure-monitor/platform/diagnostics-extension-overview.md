---
title: Introducción a la extensión Azure Diagnostics
description: Use Diagnósticos de Azure para realizar tareas de depuración, medición de rendimiento, supervisión y análisis de tráfico en servicios en la nube, en máquinas virtuales y en Service Fabric
ms.service: azure-monitor
ms.subservice: diagnostic-extension
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/13/2019
ms.openlocfilehash: 1bdefc6b61e4e5cc5b8648880c5fdd8662af1bc1
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75395364"
---
# <a name="what-is-azure-diagnostics-extension"></a>Qué es la extensión Azure Diagnostics
La extensión Azure Diagnostics es un agente dentro de Azure que habilita la recopilación de datos de diagnóstico en una aplicación implementada. Puede utilizar la extensión de diagnóstico desde un número de orígenes diferentes. Actualmente se admiten los roles de trabajo y web de Azure Cloud Services (clásico), las máquinas virtuales, los conjuntos de escalado de máquinas virtuales y Service Fabric. Otros servicios de Azure tienen métodos de diagnóstico diferentes. Consulte [Introducción a la supervisión en Azure](../../azure-monitor/overview.md).

## <a name="linux-agent"></a>Agente Linux
Una [versión Linux de la extensión](../../virtual-machines/extensions/diagnostics-linux.md) está disponible para máquinas virtuales que ejecutan Linux. Las estadísticas recopiladas y los comportamientos varían frente a la versión Windows.

## <a name="data-you-can-collect"></a>Datos que puede recopilar
La extensión Azure Diagnostics puede recopilar los siguientes tipos de datos:

| Origen de datos | Descripción |
| --- | --- |
| Métrica de contadores de rendimiento |Sistema operativo y contadores de rendimiento personalizados |
| Registros de aplicación |Seguimiento de mensajes escritos por la aplicación |
| Registros de eventos de Windows |Información enviada al sistema de registro de eventos de Windows |
| Registros de .NET EventSource |Eventos de escritura de código mediante la clase [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) de .NET |
| Registros IIS |Información sobre los sitios web de IIS |
| [Registros de ETW basado en manifiesto](https://docs.microsoft.com/windows/desktop/etw/about-event-tracing) |Eventos de Seguimiento de eventos para Windows generados por cualquier proceso.(1) |
| Volcados de memoria (registros) |Información sobre el estado del proceso en caso de bloqueo de una aplicación |
| Registros de errores personalizados |Registros creados por su aplicación o servicio |
| Registros de infraestructura de diagnóstico de Azure |Información sobre Azure Diagnostics |

(1) Para obtener una lista de proveedores de ETW, ejecute `c:\Windows\System32\logman.exe query providers` en una ventana de la consola en el equipo del cual quiere recopilar información.

## <a name="data-storage"></a>Almacenamiento de datos
La extensión almacena sus datos en una [cuenta de Azure Storage](diagnostics-extension-to-storage.md) que especifique.

También puede enviarlos a [Application Insights](../../azure-monitor/app/cloudservices.md). 

Otra opción es transmitirlos al [Centro de eventos](../../event-hubs/event-hubs-about.md), que le permite enviarlos a servicios de supervisión que no son de Azure.

También tiene la opción de enviar los datos a la base de datos de series temporales de las métricas de Azure Monitor. En este momento, este receptor solo se puede aplicar a los contadores de rendimiento. Permite enviar los contadores de rendimiento como métricas personalizadas. Esta característica se encuentra en versión preliminar. El receptor de Azure Monitor admite:
* La recuperación de todos los contadores de rendimiento que se envían a Azure Monitor a través de las [API de métricas de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/).
* Alertas en todos los contadores de rendimiento que se envían a Azure Monitor a través de las [alertas de métricas](../../azure-monitor/platform/alerts-overview.md) de Azure Monitor
* El tratamiento del operador comodín de los contadores de rendimiento como la dimensión "Instancia" de la métrica.  Por ejemplo, si recopiló el contador "LogicalDisk(\*)/DiskWrites/sec", debería poder filtrar y dividir en la dimensión "Instancia" para el trazado o una alerta en las escrituras en disco/s para cada disco lógico de la máquina virtual (por ejemplo, C:).

Para más información sobre cómo configurar este receptor, consulte la [documentación del esquema de Azure Diagnostics](diagnostics-extension-schema-1dot3.md).

## <a name="costs"></a>Costos
Cada una de las opciones anteriores puede incurrir en costos. Asegúrese de investigarlas para evitar facturas inesperadas.  Application Insights, Event Hub y Azure Storage tienen costos independientes asociados con la ingesta y el tiempo almacenado. En concreto, Azure Storage conservará los datos para siempre, por lo que es posible que desee purgar los datos más antiguos después de un período de tiempo determinado para mantener sus costos al mínimo.    

## <a name="versioning-and-configuration-schema"></a>Esquema de configuración y control de versiones
Consulte [Historial y versiones del esquema de configuración de la extensión Azure Diagnostics](diagnostics-extension-schema.md).


## <a name="next-steps"></a>Pasos siguientes
Seleccione el servicio sobre el cual intenta recopilar diagnósticos y utilice los siguientes artículos para comenzar. Utilice los vínculos de diagnósticos generales de Azure como referencia para tareas específicas.

## <a name="cloud-services-using-azure-diagnostics"></a>Cloud Services que usan Diagnósticos de Azure
* Si utiliza Visual Studio, consulte [Uso de Visual Studio para realizar el seguimiento de una aplicación de Cloud Services](/visualstudio/azure/vs-azure-tools-debug-cloud-services-virtual-machines) para comenzar. De lo contrario, consulte
* [Supervisión de Cloud Services con Diagnósticos de Azure](../../cloud-services/cloud-services-how-to-monitor.md)
* [Habilitación de diagnósticos en Cloud Services y Virtual Machines de Azure](../../cloud-services/cloud-services-dotnet-diagnostics.md)

Para temas más avanzados, consulte

* [Application Insights para Cloud Services de Azure](../../azure-monitor/app/cloudservices.md)
* [Seguimiento del flujo en una aplicación de Cloud Services  con Diagnósticos de Azure](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Uso de PowerShell para configurar diagnósticos en Cloud Services](../../virtual-machines/extensions/diagnostics-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtual Machines
* Si utiliza Visual Studio, consulte el artículo sobre el [uso de Visual Studio para rastrear Azure Virtual Machines](/visualstudio/azure/vs-azure-tools-debug-cloud-services-virtual-machines) para comenzar. De lo contrario, consulte
* [Set up Azure Diagnostics on an Azure Virtual Machine](/azure/virtual-machines/extensions/diagnostics-windows)

Para temas más avanzados, consulte

* [Uso de PowerShell para habilitar Azure Diagnostics en una máquina virtual con Windows](../../virtual-machines/extensions/diagnostics-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Creación de una máquina virtual de Windows con supervisión y diagnóstico mediante una plantilla del Administrador de recursos de Azure](../../virtual-machines/extensions/diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Empiece por el artículo sobre la [supervisión de una aplicación de Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Cuando acceda a este artículo, verá muchos otros artículos de diagnósticos de Service Fabric disponibles en el árbol de navegación de la izquierda.

## <a name="general-articles"></a>Artículos generales
* Aprenda a [utilizar los contadores de rendimiento en Diagnósticos de Azure](../../cloud-services/diagnostics-performance-counters.md).
* Si tiene problemas al iniciar los diagnósticos o al buscar datos en las tablas de Azure Storage, consulte [Solución de problemas de Azure Diagnostics](diagnostics-extension-troubleshooting.md).

