---
title: Orígenes de datos en Azure Monitor | Microsoft Docs
description: Describe los datos disponibles para supervisar el estado y rendimiento de los recursos de Azure y las aplicaciones que se ejecutan en ellos.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: bwren
ms.openlocfilehash: b77fb3ab5651147c59b9f0afd22a2d6d0159c90e
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357465"
---
# <a name="sources-of-monitoring-data-for-azure-monitor"></a>Orígenes de datos de supervisión de Azure Monitor
Azure Monitor se basa en un [plataforma de datos de supervisión comunes](data-platform.md) que incluye [registros](data-platform-logs.md) y [métricas](data-platform-metrics.md). Recopilación de datos en esta plataforma permite que los datos de varios recursos que analizarse juntos con un conjunto común de herramientas en Azure Monitor. Los datos de supervisión también pueden enviarse a otras ubicaciones para admitir determinados escenarios, y algunos recursos pueden escribir en otras ubicaciones antes de que se pueden recopilar en los registros o métricas.

En este artículo se describe los distintos orígenes de datos recopilados por Azure Monitor, además de los datos de supervisión creados por los recursos de Azure de supervisión. Se proporcionan vínculos a información detallada sobre la configuración necesaria para recopilar estos datos a ubicaciones diferentes.

## <a name="application-tiers"></a>Niveles de aplicación

Orígenes de datos de supervisión de aplicaciones de Azure se pueden organizar en niveles, los niveles más altos que se va a su propia aplicación y los niveles inferiores que se va a componentes de la plataforma Windows Azure. El método de acceso a datos de cada nivel varía. Se resumen los niveles de aplicación en la tabla siguiente, y los orígenes de datos en cada nivel de supervisión se presentan en las secciones siguientes. Consulte [supervisión de las ubicaciones de los datos en Azure](data-locations.md) para obtener una descripción de cada ubicación de los datos y cómo puede tener acceso a sus datos.


![Niveles de supervisión](../media/overview/overview.png)


### <a name="azure"></a>Azure
La siguiente tabla describe brevemente los niveles de aplicación que son específicos de Azure. Siga el vínculo para obtener más detalles sobre cada una de las secciones siguientes.

| Nivel: | DESCRIPCIÓN | Método de colección |
|:---|:---|:---|
| [Azure Tenant](#azure-tenant) | datos sobre el funcionamiento de los servicios de Azure en el nivel del inquilino, como Azure Active Directory. | Ver datos AAD en el portal o configurar la recopilación de uso de una configuración de diagnóstico del inquilino de Azure Monitor. |
| [Suscripción de Azure](#azure-subscription) | Datos relacionados con el mantenimiento y administración de servicios entre recursos en su suscripción de Azure como administrador de recursos y el estado del servicio. | Ver en el portal o configurar la recopilación de supervisión de Azure con un perfil de registro. |
| [Recursos de Azure](#azure-resources) |  Datos sobre el funcionamiento y el rendimiento de cada recurso de Azure. | Automáticamente, vista de métricas recopilados en el Explorador de métricas.<br>Configurar opciones de diagnóstico para recopilar registros en Azure Monitor.<br>Supervisión de soluciones y perspectivas disponibles para la supervisión más detallada para determinados tipos de recursos. |

### <a name="azure-other-cloud-or-on-premises"></a>Azure, otra en la nube o local 
La siguiente tabla describe brevemente los niveles de aplicación que pueden estar en Azure, otro en la nube o local. Siga el vínculo para obtener más detalles sobre cada una de las secciones siguientes.

| Nivel: | DESCRIPCIÓN | Método de colección |
|:---|:---|:---|
| [Sistema operativo (invitado)](#operating-system-guest) | Datos sobre el sistema operativo en los recursos de proceso. | Instale el agente de Log Analytics para recopilar orígenes de datos de cliente en Azure Monitor y dependencia del agente para recopilar las dependencias que admiten a Azure Monitor para las máquinas virtuales.<br>Máquinas virtuales de Azure, instale la extensión de diagnóstico de Azure para recopilar registros y métricas en Azure Monitor. |
| [Código de la aplicación](#application-code) | Datos sobre el rendimiento y la funcionalidad de la aplicación real y el código, incluidos los seguimientos de rendimiento, registros de aplicaciones y telemetría de usuario. | Instrumentar el código para recopilar datos en Application Insights. |
| [Orígenes personalizados](#custom-sources) | Datos de servicios externos u otros componentes o dispositivos. | Recopilar datos de registro o las métricas en Azure Monitor desde cualquier cliente REST. |

## <a name="azure-tenant"></a>Inquilino de Azure
Los datos de telemetría relacionados con el inquilino de Azure se recopilan desde los servicios de todos los inquilinos, como Azure Active Directory.

![Recopilación del inquilino de Azure](media/data-sources/tenant.png)

### <a name="azure-active-directory-audit-logs"></a>Registros de auditoría de Azure Active Directory
Los [informes de Azure Active Directory](../../active-directory/reports-monitoring/overview-reports.md), contienen el historial de actividad de inicio de sesión y la traza de auditoría de los cambios realizados en un inquilino determinado. 

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Registros de Azure Monitor | Configurar registros de Azure AD que se recopilan en Azure Monitor para analizarlas con otros datos de supervisión. | [Integrar los registros de Azure AD con registros de Azure Monitor (versión preliminar)](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure Storage | Exportar registros de Azure AD en Azure Storage para archivar. | [Tutorial: Archivar los registros de Azure AD en una cuenta de Azure storage (versión preliminar)](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Centro de eventos | Los registros de Azure AD a otras ubicaciones mediante Event Hubs de Stream. | [Tutorial: Registros de Azure Active Directory a un centro de eventos de Azure (versión preliminar) de Stream](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md). |



## <a name="azure-subscription"></a>Suscripción de Azure
Telemetría relacionada con el estado y el funcionamiento de su suscripción de Azure.

![Suscripción de Azure](media/data-sources/azure-subscription.png)

### <a name="azure-activity-log"></a>Registro de actividad de Azure 
El [registro de actividad de Azure](activity-logs-overview.md) incluye registros de mantenimiento del servicio junto con los registros en los cambios de configuración realizados en los recursos en su suscripción de Azure. El registro de actividad está disponible para todos los recursos de Azure y representa su vista _externa_.

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|
| Registro de actividades | El registro de actividad se recopila en su propio almacén de datos que se puede ver en el menú de Azure Monitor o utilizar para crear alertas del registro de actividad. | [Consulta el registro de actividad en el portal de Azure](activity-log-view.md#azure-portal) |
| Registros de Azure Monitor | Configurar registros de Azure Monitor para recopilar el registro de actividad para analizarlo con otros datos de supervisión. | [Recopilar y analizar los registros de actividad de Azure en el área de trabajo de Log Analytics en Azure Monitor](activity-log-collect.md) |
| Azure Storage | Exportar el registro de actividad en Azure Storage para archivar. | [Archivar el registro de actividad](activity-log-export.md#archive-activity-log)  |
| Event Hubs | Stream el registro de actividad a otras ubicaciones mediante Event Hubs | [Registro de actividad Stream al centro de eventos](activity-log-export.md#stream-activity-log-to-event-hub). |

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](../../service-health/service-health-overview.md) proporciona información sobre el estado de los servicios de Azure de la suscripción de los que dependen la aplicación y los recursos.

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Registro de actividades<br>Registros de Azure Monitor | Registros de estado del servicio se almacenan en el registro de actividad de Azure, para que pueda verlos en el portal de Azure o realice otras actividades que puede realizar con el registro de actividad. | [Visualización de notificaciones de mantenimiento del servicio mediante Azure Portal](service-notifications.md) |


## <a name="azure-resources"></a>Recursos de Azure
Los registros de diagnóstico de nivel de recursos y las métricas proporcionan información sobre el funcionamiento _interno_ de recursos de Azure. Están disponibles para servicios de Azure más y supervisión de soluciones y insights recopilan datos adicionales para determinados servicios.

![Colección de recursos de Azure](media/data-sources/azure-resources.png)


### <a name="platform-metrics"></a>Métricas de la plataforma 
Servicios de Azure más enviará [métricas de la plataforma](data-platform-metrics.md) que reflejen su rendimiento y funcionamiento directamente a la base de datos de métricas. Las [métricas específicas varían en función del tipo de recurso](metrics-supported.md). 

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Métricas en Microsoft Azure | Métricas de la plataforma se escribirán en la base de datos de métricas de Azure Monitor sin ninguna configuración. Métricas de la plataforma de acceso desde el Explorador de métricas.  | [Introducción al explorador de métricas de Azure](metrics-getting-started.md)<br>[Métricas compatibles con Azure Monitor](metrics-supported.md) |
| Registros de Azure Monitor | Copie métricas de la plataforma en los registros para el análisis de tendencias y otros con Log Analytics. | [Diagnósticos de Azure directos a Log Analytics](collect-azure-metrics-logs.md#azure-diagnostics-direct-to-log-analytics) |
| Event Hubs | Métricas de Stream a otras ubicaciones mediante Event Hubs. |[Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](stream-monitoring-data-event-hubs.md) |

### <a name="diagnostic-logs"></a>Registros de diagnóstico
[Los registros de diagnóstico](diagnostic-logs-overview.md) proporcionan información sobre la _interno_ operación un recurso de Azure.  Los registros de diagnóstico no están habilitados de forma predeterminada. Debe habilitarlas y especificar un destino para cada recurso. 

Los requisitos de configuración y el contenido de los registros de diagnóstico varían según el tipo de recurso, y no todos los servicios crean registros de diagnóstico. Consulte [compatible de servicios, esquemas y categorías para los registros de diagnóstico de Azure](diagnostic-logs-schema.md) para obtener más información sobre cada servicio y vínculos a procedimientos de configuración detallados. Si el servicio no aparece en este artículo, a continuación, ese servicio actualmente no escribe en los registros de diagnóstico.

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Registros de Azure Monitor | Enviar registros de diagnóstico para registros de Azure Monitor para su análisis con otros datos de registro recopilados. Algunos recursos pueden escribir directamente en Azure Monitor mientras que otras personas escribir en una cuenta de almacenamiento antes de que se importan en un área de trabajo de Log Analytics. | [Registros de diagnóstico de Azure Stream al área de trabajo de Log Analytics en Azure Monitor](diagnostic-logs-stream-log-store.md)<br>[Usar el portal de Azure para recopilar registros de Azure Storage](azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage)  |
| Almacenamiento | Diagnóstico de enviar registros al almacenamiento de Azure para el archivado. | [Archivar registros de diagnóstico de Azure](archive-diagnostic-logs.md) |
| Event Hubs | Los registros de diagnóstico de Stream a otras ubicaciones mediante Event Hubs. |[Registros de diagnóstico de Azure Stream a un centro de eventos](diagnostic-logs-stream-event-hubs.md) |

## <a name="operating-system-guest"></a>Sistema operativo (invitado)
Los recursos de proceso en Azure, en otras nubes y en el entorno local tienen un sistema operativo invitado para supervisar. Con la instalación de uno o más agentes, puede recopilar datos de telemetría desde el invitado en Azure Monitor para analizarlos con las mismas herramientas de supervisión que los servicios de Azure.

![Colección de recursos de proceso de Azure](media/data-sources/compute-resources.png)

### <a name="azure-diagnostic-extension"></a>Extensión de diagnóstico de Azure
Habilitar la extensión de diagnósticos de Azure para máquinas virtuales de Azure le permite para recopilar registros y métricas del sistema operativo invitado de Azure compute recursos de máquina virtual de Azure Cloud Service (classic) Web y Roles de trabajo, las máquinas virtuales, conjuntos de escalado y Service Fabric.

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Almacenamiento | Cuando se habilita la extensión de diagnósticos, escribirá en una cuenta de almacenamiento de forma predeterminada. | [Almacenamiento y visualización de los datos de diagnóstico en Azure Storage](diagnostics-extension-to-storage.md) |
| Métricas en Microsoft Azure | Al configurar la extensión de diagnósticos para recopilar los contadores de rendimiento, que se escriben en la base de datos de métricas de Azure Monitor. | [Enviar métricas de SO invitado a la métrica de Azure Monitor almacenan mediante una plantilla de Resource Manager para una máquina virtual de Windows](collect-custom-metrics-guestos-resource-manager-vm.md) |
| Registros de Application Insights | Recopilar registros y contadores de rendimiento del recurso de proceso que respaldan la aplicación que se analice con otros datos de aplicación. | [Enviar datos de diagnóstico de servicio en la nube, máquinas virtuales o Service Fabric a Application Insights](diagnostics-extension-to-application-insights.md) |
| Event Hubs | Configurar la extensión de diagnósticos para transmitir los datos a otras ubicaciones mediante Event Hubs.  | [Transmisión de datos de diagnóstico de Azure en la ruta de acceso activa mediante Event Hubs](diagnostics-extension-stream-event-hubs.md) |

### <a name="log-analytics-agent"></a>Agente de Log Analytics 
Instalar al agente de Log Analytics para supervisión completa y la administración de las máquinas virtuales Windows o Linux. La máquina virtual se puede ejecutar en Azure, en otra nube o en el entorno local.

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Registros de Azure Monitor | El agente de Log Analytics se conecta a Azure supervisar directamente o mediante System Center Operations Manager y le permite recopilar datos de orígenes de datos que se configuración o de soluciones que proporcionan información adicional sobre las aplicaciones de supervisión se está ejecutando en la máquina virtual. | [Orígenes de datos del agente de Azure Monitor](agent-data-sources.md)<br>[Conectar Operations Manager a Azure Monitor](om-agents.md) |


### <a name="azure-monitor-for-vms"></a>Azure Monitor para máquinas virtuales 
[Azure Monitor para las máquinas virtuales](../insights/vminsights-overview.md) proporciona una experiencia de supervisión personalizada para las máquinas virtuales que proporciona las características más allá de la funcionalidad de Azure Monitor principal, incluido el estado del servicio y el estado de la máquina virtual. Requiere a un agente de dependencia en máquinas virtuales Windows y Linux que se integra con el agente de Log Analytics para recopilar datos detectados sobre procesos en ejecución en la máquina virtual y las dependencias de procesos externos.

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Registros de Azure Monitor | Almacena los datos sobre los procesos y dependencias en el agente. | [Mediante Azure Monitor para las máquinas virtuales se asignan a comprender los componentes de la aplicación (versión preliminar)](../insights/vminsights-maps.md) |
| Almacenamiento de máquina virtual | Azure Monitor para las máquinas virtuales almacena información de estado de mantenimiento en una ubicación personalizada. Esto solo está disponible en Azure Monitor para las máquinas virtuales en el portal de Azure además el [API de REST de Azure Resource health](/rest/api/resourcehealth/). | [Conocer el estado de las máquinas virtuales de Azure](../insights/vminsights-health.md)<br>[API de REST de Azure Resource health](https://docs.microsoft.com/rest/api/resourcehealth/) |



## <a name="application-code"></a>Código de la aplicación
Se realiza la supervisión detallada de la aplicación en Azure Monitor con [Application Insights](https://docs.microsoft.com/azure/application-insights/) que recopila datos de aplicaciones que se ejecutan en una variedad de plataformas. La aplicación se puede ejecutar en Azure, en otra nube o en el entorno local.

![Recopilación de datos de aplicación](media/data-sources/applications.png)


### <a name="application-data"></a>Datos de aplicación
Cuando se habilita Application Insights para una aplicación mediante la instalación de un paquete de instrumentación, recopila métricas y registros relacionados con el rendimiento y el funcionamiento de la aplicación. Application Insights almacena los datos que recopila en la misma plataforma de datos de Azure Monitor utilizada por otros orígenes de datos. Incluye numerosas herramientas para analizar estos datos, pero también puede analizar con datos de otros orígenes con herramientas como explorador de métricas y Log Analytics.

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Registros de Azure Monitor | Datos operativos sobre la aplicación como vistas de página, las solicitudes de aplicación, las excepciones y seguimientos. | [Analizar datos de registro en Azure Monitor](../log-query/log-query-overview.md) |
|                    | Información de dependencia entre los componentes de aplicación para admitir la asignación de aplicaciones y la correlación de telemetría. | [Correlación de telemetría en Application Insights](../app/correlation.md) <br> [Mapa de aplicación](../app/app-map.md) |
|            | Resultados de pruebas de disponibilidad que probar la disponibilidad y capacidad de respuesta de la aplicación desde diferentes ubicaciones en la red Internet pública. | [Supervisión de la disponibilidad y la capacidad de respuesta de cualquier sitio web](../app/monitor-web-app-availability.md) |
| Métricas en Microsoft Azure | Application Insights recopila las métricas que describen el rendimiento y el funcionamiento de la aplicación, además de las métricas personalizadas que definen en la aplicación en la base de datos de métricas de Azure Monitor. | [Basado en registro y previamente agregadas de métricas en Application Insights](../app/pre-aggregated-metrics-log-metrics.md)<br>[API de Application Insights para eventos y métricas personalizados](../app/api-custom-events-metrics.md) |
| Azure Storage | Enviar datos de la aplicación en Azure Storage para archivar. | [Exportación de datos de telemetría desde Application Insights](../app/export-telemetry.md) |
|            | Detalles de las pruebas de disponibilidad se almacenan en el almacenamiento de Azure. Usar Application Insights en Azure portal para descargar para su análisis local. Resultados de pruebas de disponibilidad se almacenan en los registros de Azure Monitor. | [Supervisión de la disponibilidad y la capacidad de respuesta de cualquier sitio web](../app/monitor-web-app-availability.md) |
|            | Datos de seguimiento de Profiler se almacenan en el almacenamiento de Azure. Usar Application Insights en Azure portal para descargar para su análisis local.  | [Generar perfiles de aplicaciones de producción en Azure con Application Insights](../app/profiler-overview.md) 
|            | Depurar instantánea de datos que se capturaron para obtener un subconjunto de las excepciones se almacenan en el almacenamiento de Azure. Usar Application Insights en Azure portal para descargar para su análisis local.  | [Funcionamiento de las instantáneas](../app/snapshot-debugger.md#how-snapshots-work) |

## <a name="monitoring-solutions-and-insights"></a>Soluciones de supervisión y perspectivas
[Las soluciones de supervisión](../insights/solutions.md) y [Insights](../insights/insights-overview.md) recopilar datos para proporcionar información adicional sobre el funcionamiento de una aplicación o servicio determinado. Pueden abordar los recursos en distintos niveles de aplicación e incluso varios niveles.

### <a name="monitoring-solutions"></a>Soluciones de supervisión

| Destino | DESCRIPCIÓN | Referencia
|:---|:---|:---|
| Registros de Azure Monitor | Soluciones de supervisión de recopilan datos en los registros de Azure Monitor donde puede ser analizado mediante el lenguaje de consulta o [vistas](view-designer.md) que normalmente se incluyen en la solución. | [Detalles de la recopilación de datos para la supervisión de soluciones en Azure](../insights/solutions-inventory.md) |


### <a name="azure-monitor-for-containers"></a>Azure Monitor para contenedores
[Azure Monitor para contenedores](../insights/container-insights-overview.md) proporciona una experiencia de supervisión personalizada para [Azure Kubernetes Service (AKS)](/azure/aks/). Recopila los datos adicionales acerca de estos recursos que se describe en la tabla siguiente.

| Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|
| Registros de Azure Monitor | Almacenes de datos de supervisión para AKS como inventario, registros y eventos. Datos de métrica también se almacenan en los registros para poder aprovechar su funcionalidad de análisis en el portal. | [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](../insights/container-insights-analyze.md) |
| Métricas en Microsoft Azure | Datos de métricas se almacenan en la base de datos de métrica para alertas y visualización de la unidad. | [Visualización de las métricas de contenedor en el Explorador de métricas](../insights/container-insights-analyze.md#view-container-metrics-in-metrics-explorer) |
| Azure Kubernetes Service | En orden para una experiencia casi de tiempo real, Azure Monitor para contenedores presenta los datos directamente desde el servicio de Azure Kubernetes en Azure portal. | [Vista de los registros de contenedor en tiempo real con Azure Monitor para contenedores (versión preliminar)](../insights/container-insights-live-logs.md) |

### <a name="azure-monitor-for-vms"></a>Azure Monitor para máquinas virtuales
[Azure Monitor para las máquinas virtuales](../insights/vminsights-overview.md) proporciona una experiencia personalizada para la supervisión de máquinas virtuales. Se incluye una descripción de los datos recopilados por Azure Monitor para las máquinas virtuales en el [del sistema operativo (invitado)](#operating-system-guest) sección anterior.

## <a name="custom-sources"></a>Orígenes personalizados
Además de los niveles estándar de una aplicación, puede que necesite supervisar otros recursos que tengan datos de telemetría y que no puedan recopilarse con los otros orígenes de datos. Para estos recursos, escribir estos datos en las métricas o registros mediante una API de Azure Monitor.

![Colección personalizada](media/data-sources/custom.png)

| Destino | Método | DESCRIPCIÓN | Referencia |
|:---|:---|:---|:---|
| Registros de Azure Monitor | API de recopilador de datos | Recopilar datos de registro desde cualquier cliente REST y almacenar en el área de trabajo de Log Analytics. | [Enviar datos de registro a Azure Monitor con HTTP Data Collector API (versión preliminar)](data-collector-api.md) |
| Métricas en Microsoft Azure | API de métricas personalizadas | Recopilar datos de métricas desde cualquier cliente de REST y almacenarlo en la base de datos de métricas de Azure Monitor. | [Enviar métricas personalizadas para un recurso de Azure en el almacén de métricas de Azure Monitor mediante una API de REST](metrics-store-custom-rest-api.md) |


## <a name="other-services"></a>Otros servicios
Otros servicios de Azure escriben datos en la plataforma de datos de Azure Monitor. Esto le permite analizar los datos recopilados por estos servicios con los datos recopilados por Azure Monitor y aprovechar el mismo análisis y herramientas de visualización.

| Servicio | Destino | DESCRIPCIÓN | Referencia |
|:---|:---|:---|:---|
| [Azure Security Center](/azure/security-center/) | Registros de Azure Monitor | Azure Security Center almacena los datos de seguridad que se recopila en un área de trabajo de Log Analytics que le permite analizar con otros datos de registro recopilados por Azure Monitor.  | [Recopilación de datos en Azure Security Center](../../security-center/security-center-enable-data-collection.md) |
| [Azure Sentinel](/azure/sentinel/) | Registros de Azure Monitor | Sentinel Azure almacena los datos que recopila de orígenes de datos diferentes en un área de trabajo de Log Analytics que le permite analizar con otros datos de registro recopilados por Azure Monitor.  | [Conectarse a orígenes de datos](/azure/sentinel/quickstart-onboard) |


## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [tipos de datos de supervisión recopilados por Azure Monitor](data-platform.md) y cómo ver y analizar estos datos.
- Lista los [dónde almacenan los datos en los recursos de Azure de diferentes ubicaciones](data-locations.md) y cómo se tiene acceso. 
