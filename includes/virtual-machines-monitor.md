---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 01/27/2019
ms.author: cynthn
ms.openlocfilehash: a13ce8d3dc71be83e05fae5bd07f30f413ce59f2
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55147985"
---
Puede sacar provecho de numerosas oportunidades para supervisar las máquinas virtuales recopilando, viendo y analizando datos de registro y de diagnóstico. Para llevar a cabo una [supervisión](../articles/azure-monitor/overview.md) sencilla de una máquina virtual, puede usar la pantalla Información general correspondiente en Azure Portal. Puede usar [extensiones](../articles/virtual-machines/windows/extensions-features.md) a fin de configurar los diagnósticos en las máquinas virtuales para recopilar datos de métricas adicionales. También puede usar opciones de supervisión más avanzadas, como [Application Insights](../articles/azure-monitor/app/app-insights-overview.md) y [Log Analytics](../articles/azure-monitor/log-query/log-query-overview.md).

## <a name="diagnostics-and-metrics"></a>Diagnósticos y métricas 

Puede configurar y supervisar la recopilación de [datos de diagnóstico](https://docs.microsoft.com/cli/azure/vm/diagnostics) con [métricas](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) en Azure Portal, la CLI de Azure, Azure PowerShell y la programación de Interfaces de programación de aplicaciones (API). Por ejemplo, puede:

- **Observar métricas básicas para la máquina virtual.** En la pantalla Información general de Azure Portal, entre las métricas básicas que se muestran se incluye el uso de CPU, el uso de la red, el total de bytes de disco y las operaciones de disco por segundo.

- **Habilitar y observar la recopilación de diagnósticos de arranque y a través de Azure Portal.** Cuando lleva su propia imagen a Azure o incluso cuando arranca una de las imágenes de la plataforma, puede haber muchas razones por las que una máquina virtual pueda entrar en un estado que no permita el arranque. Puede habilitar fácilmente el diagnóstico de arranque cuando se crea una máquina virtual, haciendo clic en **Habilitado** en Diagnóstico de arranque, en la sección Supervisión de la pantalla Configuración.

    Cuando las máquinas virtuales arrancan, el agente de los diagnósticos de arranque captura la salida del arranque y la almacena en Azure Storage. Estos datos se pueden utilizar para solucionar los problemas de arranque de la máquina virtual. Los diagnósticos de arranque no se habilitan automáticamente al crear una máquina virtual mediante las herramientas de línea de comandos. Antes de habilitar los diagnósticos de arranque, es preciso crear una cuenta de almacenamiento para almacenar los registros de arranque. Si habilita el diagnóstico de arranque en Azure Portal, se crea automáticamente una cuenta de almacenamiento.

    Si no habilitó el diagnóstico de arranque cuando se creó la máquina virtual, siempre puede habilitarlo más adelante con la [CLI de Azure](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmbootdiagnostics) o una [plantilla de Azure Resource Manager](../articles/virtual-machines/windows/extensions-diagnostics-template.md).

- **Habilitar la recopilación de datos de diagnóstico del sistema operativo invitado.** Cuando se crea una máquina virtual, en la pantalla de configuración tiene la oportunidad de habilitar los diagnósticos del sistema operativo invitado. Cuando habilita la recopilación de datos de diagnóstico, la [extensión IaaSDiagnostics para Linux](../articles/virtual-machines/linux/diagnostic-extension.md) o la [extensión IaaSDiagnostics para Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) se agregan a la máquina virtual, lo que le permite recopilar más datos de disco, CPU y memoria.

    Con los datos de diagnóstico recopilados, puede configurar el escalado automático para las máquinas virtuales. También puede configurar registros para almacenar los datos y configurar alertas para informarle cuando el rendimiento no sea correcto.

## <a name="alerts"></a>Alertas

Puede crear [alertas](../articles/azure-monitor/platform/alerts-overview.md) basadas en métricas de rendimiento concretas. Algunos problemas de los que se puede alertar son, por ejemplo, cuando el uso promedio de la CPU supera un umbral concreto o cuando el espacio libre en disco disponible cae por debajo de una cantidad determinada. Es posible configurar alertas en [Azure Portal](../articles/azure-monitor/platform/alerts-classic-portal.md), con [Azure PowerShell](../articles/azure-monitor/platform/alerts-classic-portal.md#with-powershell) o la [CLI de Azure](../articles/azure-monitor/platform/alerts-classic-portal.md#with-azure-cli).

## <a name="azure-service-health"></a>Azure Service Health

[Azure Service Health](../articles/service-health/service-health-overview.md) proporciona soporte técnico e información orientativa personalizada cuando se producen problemas en los servicios de Azure que le afectan, y le ayuda a prepararse para los próximos mantenimientos planeados. Azure Service Health le informa de las alertas a usted y a los equipos por medio de notificaciones específicas y flexibles.

## <a name="azure-resource-health"></a>Azure Resource Health

[Azure Resource health](../articles/service-health/resource-health-overview.md) ayuda a diagnosticar si un problema de Azure afecta a los recursos y, en su caso, a obtener soporte técnico. Le informa acerca del mantenimiento actual y pasado de los recursos y le ayuda a mitigar los problemas. Resource Health le proporciona soporte técnico si necesita ayuda con los problemas de los servicios de Azure.

## <a name="azure-activity-log"></a>Azure Activity Log

[Azure Activity Log](../articles/azure-monitor/platform/activity-logs-overview.md) es un registro de suscripción que proporciona información sobre los eventos de nivel de suscripción que se han producido en Azure. Este registro incluye una serie de datos, desde datos operativos de Azure Resource Manager hasta actualizaciones en eventos de Service Health. Puede hacer clic en Activity Log, en Azure Portal, para ver el registro de la máquina virtual.

Estas son algunas de las tareas que puede hacer con el registro de actividad:

- Crear una [alerta basada en un evento de Activity Log](../articles/azure-monitor/platform/activity-logs-overview.md).
- [Transmitirlo a un centro de eventos ](../articles/azure-monitor/platform/activity-logs-stream-event-hubs.md) para ingestión en un servicio de terceros o una solución de análisis personalizada como PowerBI.
- Analizarlo en PowerBI con el [paquete de contenido de PowerBI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Guardarlo en una cuenta de almacenamiento](../articles/azure-monitor/platform/archive-activity-log.md)para archivarlo o inspeccionarlo manualmente. Puede especificar el tiempo de retención (en días) mediante el perfil de registro.

También puede tener acceso a datos de registro de actividad mediante [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), la [CLI de Azure](https://docs.microsoft.com/cli/azure/monitor) o las [API de REST de Monitor](https://docs.microsoft.com/rest/api/monitor/).

[Azure Diagnostic Logs](../articles/azure-monitor/platform/diagnostic-logs-overview.md) son registros emitidos por una máquina virtual que proporcionan datos exhaustivos y frecuentes acerca de su funcionamiento. Los registros de diagnóstico se diferencian del registro de actividad en que proporcionan una visión general acerca de las operaciones realizadas dentro de la máquina virtual.

Estas son algunas de las cosas que puede hacer con los registros de diagnóstico:

- [Guardarlos en una cuenta de almacenamiento](../articles/azure-monitor/platform/archive-diagnostic-logs.md) para auditarlos o para inspeccionarlos manualmente. Puede especificar el tiempo de retención (en días) mediante Configuración de diagnóstico de recursos.
- [Transmitirlos a Event Hubs](../articles/azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) para la ingestión en un servicio de terceros o una solución de análisis personalizado como PowerBI.
- Analizarlos con [Log Analytics](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Supervisión avanzada

- [Azure Monitor](../articles/azure-monitor/overview.md) es un servicio que supervisa los entornos local y en la nube para mantener su disponibilidad y rendimiento. Ofrece una solución completa para recopilar, analizar y actuar en la telemetría desde los entornos local y en la nube. Esta solución le ayudará a entender cómo funcionan las aplicaciones y le permitirá identificar de manera proactiva los problemas que les afectan y los recursos de los que dependen. Puede instalar una extensión en una [máquina virtual Linux](../articles/virtual-machines/linux/extensions-oms.md) o en una [máquina virtual Windows](../articles/virtual-machines/windows/extensions-oms.md) que instala el agente de Log Analytics para recopilar los datos del registro y almacenarlos en un área de trabajo de Log Analytics.

    En el caso de las máquinas virtuales Windows y Linux, para recopilar registros se recomienda instalar el agente de Log Analytics. Para instalar al agente de Log Analytics en una máquina virtual, lo más sencillo es utilizar la [extensión de VM de Log Analytics](../articles/log-analytics/log-analytics-azure-vm-extension.md). El uso de una extensión simplifica el proceso de instalación y configura automáticamente el agente para enviar datos al área de trabajo de Log Analytics que especifique. El agente también se actualiza automáticamente, garantizando así que disponga de las características y correcciones más recientes.

- [Network Watcher](../articles/network-watcher/network-watcher-monitoring-overview.md) le permite supervisar la máquina virtual y sus recursos asociados, ya que afectan a la red en la que se encuentran. Puede instalar la extensión Network Watcher Agent en una [máquina virtual Linux](../articles/virtual-machines/linux/extensions-nwa.md) o una [máquina virtual Windows](../articles/virtual-machines/windows/extensions-nwa.md).

- La solución [Azure Monitor para VM](../articles/azure-monitor/insights/vminsights-overview.md) supervisa las máquinas virtuales de Azure a escala y analiza el rendimiento y el estado de las máquinas virtuales de Windows y Linux, incluidos los diferentes procesos y las dependencias interconectadas con otros recursos y con procesos externos. 

## <a name="next-steps"></a>Pasos siguientes
- Siga los pasos de [Supervisión de una máquina virtual Windows con Azure PowerShell](../articles/virtual-machines/windows/tutorial-monitoring.md) o de [Supervisión de una máquina Virtual Linux con la CLI de Azure](../articles/virtual-machines/linux/tutorial-monitoring.md).
- Más información sobre las prácticas recomendadas de la [supervisión y el diagnóstico](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
