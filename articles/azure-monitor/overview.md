---
title: Introducción a Azure Monitor | Microsoft Docs
description: Introducción a los servicios y funcionalidades de Microsoft que contribuyen a una estrategia de supervisión completa de los servicios y las aplicaciones de Azure.
author: bwren
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: bwren
ms.openlocfilehash: d1b7b5baeba88ec0209abff60e9327031afa8b66
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52633782"
---
# <a name="azure-monitor-overview"></a>Introducción a Azure Monitor

Azure Monitor maximiza la disponibilidad y el rendimiento de las aplicaciones con una completa solución que permite recopilar, analizar y administrar datos telemétricos tanto en la nube como en entornos locales. Esta solución le ayudará a entender cómo funcionan las aplicaciones y le permitirá identificar de manera proactiva los problemas que les afectan y los recursos de los que dependen.

> [!VIDEO https://www.youtube.com/embed/_hGff5bVtkM]

## <a name="overview"></a>Información general
El siguiente diagrama proporciona una visión general de Azure Monitor. En el centro del diagrama están los almacenes de datos de las métricas y los registros, que son los dos tipos fundamentales de datos que se utilizan en Azure Monitor. En la parte izquierda, están los [orígenes que recopilan datos de telemetría de distintos recursos](../azure-monitor/platform/data-sources.md) y que llenan los [almacenes de datos](../azure-monitor/platform/data-collection.md). En la derecha, puede ver las diferentes funciones que realiza Azure Monitor con los datos recopilados, como la realización de análisis, la elaboración de alertas y la transmisión a sistemas externos.


![Introducción a Azure Monitor](media/overview/overview.png)


## <a name="monitoring-data-platform"></a>Supervisión de la plataforma de datos
Todos los datos recopilados por Azure Monitor pueden clasificarse como uno de los dos tipos fundamentales: [métricas y registros](../azure-monitor/platform/data-collection.md). Las [métricas](../azure-monitor/platform/data-collection.md#metrics) son valores numéricos que describen algún aspecto de un sistema en un momento dado. Las métricas son ligeras y capaces de admitir escenarios de tiempo casi real. Los [registros](../azure-monitor/platform/data-collection.md#logs) contienen distintos tipos de datos organizados en grupos de registros, donde cada tipo tiene diferentes conjuntos de propiedades. Los datos de telemetría, como los eventos y los seguimientos, se almacenan como registros junto con los datos de rendimiento para poder analizarlos de forma combinada.

En muchos recursos de Azure, los datos recopilados por Azure Monitor aparecen directamente en la página de información general de Azure Portal. Eche un vistazo a cualquier máquina virtual, por ejemplo, y verá varios gráficos en los que aparecen métricas de rendimiento. Haga clic en cualquiera de los gráficos para abrir los datos en el [Explorador de métricas](../monitoring-and-diagnostics/monitoring-metric-charts.md) de Azure Portal, lo que le permitirá crear gráficos con los valores de diversas métricas a lo largo del tiempo.  Puede ver los gráficos de forma interactiva o anclarlos a un panel para verlos con otras visualizaciones.

![Métricas](media/overview/metrics.png)

Los datos de registro recopilados por Azure Monitor se guardan en Log Analytics, que cuenta con un [lenguaje de consulta avanzado](../log-analytics/log-analytics-queries.md) para poder recuperar, consolidar y analizar rápidamente los datos recopilados.  Puede crear y probar consultas utilizando la [página de Log Analytics](../log-analytics/log-analytics-log-search-portals.md) de Azure Portal y analizar después los datos directamente mediante estas herramientas o guardar las consultas para usarlas con las [visualizaciones](visualizations.md) o las [reglas de alertas](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

El lenguaje de consulta de Log Analytics es adecuado para realizar búsquedas de registros simples, pero también dispone de funciones avanzadas, como agregaciones, combinaciones y análisis inteligentes. Puede aprender rápidamente el lenguaje de consulta con los [diversos tutoriales](../log-analytics/query-language/get-started-queries.md) disponibles.  Se proporciona orientación concreta a los usuarios que ya están familiarizados con [SQL](../log-analytics/query-language/sql-cheatsheet.md) y [Splunk](../log-analytics/query-language/splunk-cheatsheet.md).

![Registros](media/overview/logs.png)

## <a name="what-data-does-azure-monitor-collect"></a>¿Qué datos recopila Azure Monitor?
Azure Monitor puede recopilar datos de diversos orígenes. Puede pensar en supervisar datos para las aplicaciones en niveles que abarcan desde la aplicación hasta el sistema operativo y los servicios en los que se basa, pasando por la propia plataforma. Azure Monitor recopila datos de cada uno de los siguientes niveles:

- **Datos de supervisión de aplicaciones:** datos sobre el rendimiento y la funcionalidad del código que ha escrito, independientemente de la plataforma.
- **Datos de supervisión del sistema operativo invitado:** datos sobre el sistema operativo en el que se ejecuta la aplicación. La aplicación se puede ejecutar en Azure, en otra nube o en el entorno local. 
- **Datos de supervisión de recursos de Azure:** datos acerca del funcionamiento de un recurso de Azure.
- **Datos de supervisión de la suscripción de Azure:** datos sobre el funcionamiento y la administración de una suscripción de Azure, así como sobre el estado y el funcionamiento del propio Azure. 
- **Datos de supervisión del inquilino de Azure:** datos sobre el funcionamiento de los servicios de Azure en el nivel del inquilino, como Azure Active Directory.

En cuanto crea una suscripción a Azure y empieza a agregar recursos, como máquinas virtuales y aplicaciones web, Azure Monitor comienza a recopilar datos.  Los [registros de actividad](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) registran la creación y modificación de recursos. Las [métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md) indican cómo está funcionando un recurso y los recursos que consume. 

Amplíe los datos que recopila y obtenga información sobre el funcionamiento real de los recursos [habilitando diagnósticos](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) y [agregando un agente](../azure-monitor/platform/agent-windows.md) a los recursos de proceso. De este modo, recopilará datos de telemetría sobre el funcionamiento interno de un recurso y podrá configurar diferentes [orígenes de datos](../azure-monitor/platform/agent-data-sources.md) para recopilar registros y métricas del sistema operativo invitado Windows y Linux. 

[Agregue un paquete de instrumentación a la aplicación](../application-insights/app-insights-azure-web-apps.md) para permitir que Application Insights recopile información detallada acerca de la aplicación, como vistas de página, solicitudes de aplicación y excepciones. Compruebe de forma más exhaustiva la disponibilidad de la aplicación configurando una [prueba de disponibilidad](../application-insights/app-insights-monitor-web-app-availability.md) para simular el tráfico de usuarios.

### <a name="custom-sources"></a>Orígenes personalizados
Azure Monitor puede recopilar datos de registro de cualquier cliente de REST mediante [Data Collector API](../log-analytics/log-analytics-data-collector-api.md). Esto permite crear escenarios de supervisión personalizados y ampliar la supervisión a los recursos que no exponen datos de telemetría en otros orígenes.



## <a name="insights"></a>Información detallada
Los datos de supervisión solo resultan útiles si aportan una mayor visibilidad sobre el funcionamiento del entorno informático. Azure Monitor cuenta con varias características y herramientas que proporcionan valiosa información sobre las aplicaciones y los recursos de los que dependen. Las características y las [soluciones de supervisión](../azure-monitor/insights/solutions.md), como [Application Insights](../application-insights/app-insights-overview.md) y Container Insights, proporcionan información exhaustiva sobre diferentes aspectos de la aplicación y determinados servicios de Azure. 

### <a name="application-insights"></a>Application Insights
[Application Insights](../application-insights/app-insights-overview.md) supervisa la disponibilidad, el rendimiento y el uso de las aplicaciones web, tanto si están hospedadas en la nube como en un entorno local. Esta solución utiliza la eficaz plataforma de análisis de datos de Log Analytics para proporcionarle información exhaustiva sobre las operaciones de la aplicación y diagnosticar errores sin esperar a que un usuario lo notifique. Application Insights incorpora puntos de conexión con una serie de herramientas de desarrollo y se integra con Visual Studio para admitir los procesos de DevOps.

![Detalles de la aplicación](media/overview/app-insights.png)

### <a name="azure-monitor-for-containers"></a>Azure Monitor para contenedores
Azure Monitor para contenedores es una característica diseñada para supervisar el rendimiento de las cargas de trabajo de contenedor implementadas en clústeres de Kubernetes administrados y hospedados en Azure Kubernetes Service (AKS). Proporciona información sobre el rendimiento recopilando métricas del procesador y de la memoria procedentes de los controladores, los nodos y los contenedores disponibles en Kubernetes mediante Metrics API. También se recopilan registros del contenedor.  Una vez habilitada la supervisión de clústeres de Kubernetes, estas métricas y registros se recopilan automáticamente mediante una versión en contenedor del agente de Log Analytics para Linux y se almacenan en Log Analytics.

![Estado de los contenedores](media/overview/container-insights.png)

### <a name="azure-monitor-for-vms"></a>Azure Monitor para máquinas virtuales
La característica VM Insights de Azure Monitor supervisa las máquinas virtuales (VM) de Azure a escala analizando el rendimiento y el estado de las máquinas virtuales de Windows y Linux, incluidos los diferentes procesos y las dependencias interconectadas con otros recursos y con procesos externos. La solución permite supervisar el rendimiento y las dependencias de las aplicaciones en máquinas virtuales hospedadas en el entorno local o en otro proveedor en la nube.  


![VM Insights](media/overview/vm-insights.png)

### <a name="monitoring-solutions"></a>Soluciones de supervisión
Las [soluciones de supervisión](../azure-monitor/insights/solutions.md) de Azure Monitor son conjuntos empaquetados de lógica que proporcionan información sobre una determinada aplicación o servicio. Estas soluciones recopilan datos en Log Analytics junto con otra información de supervisión utilizando [consultas](../log-analytics/log-analytics-queries.md) para los análisis y [vistas](../azure-monitor/platform/view-designer.md). Las soluciones de supervisión están [disponibles en Microsoft](../azure-monitor/insights/solutions-inventory.md) y otros asociados, y ofrecen herramientas de supervisión para distintos servicios de Azure y otras aplicaciones.

![Soluciones de supervisión](media/overview/solutions-overview.png)

## <a name="responding-to-critical-situations"></a>Respuesta a situaciones críticas
Además de permitirle analizar de forma interactiva los datos de supervisión, una solución de supervisión eficaz debe ser capaz de responder proactivamente a condiciones críticas que se den en los datos que recopila. Esto podría hacerse enviando un mensaje o correo a un administrador responsable de investigar un problema. O también podría hacerse iniciando un proceso automatizado que intente corregir una condición de error.


### <a name="alerts"></a>Alertas
Las [alertas de Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-alerts.md) informan de forma proactiva de los estados críticos e intentan aplicar acciones correctivas. Las reglas de alertas basadas en métricas proporcionan avisos casi en tiempo real que se generan en función de unos valores numéricos, mientras que las reglas basadas en registros permiten aplicar una lógica compleja entre datos de diversos orígenes.

Las reglas de alertas de Azure Monitor utilizan [grupos de acciones](../monitoring-and-diagnostics/monitoring-action-groups.md), que contienen diferentes conjuntos de destinatarios y acciones que pueden compartirse entre varias reglas. En función de los requisitos, los grupos de acciones pueden realizar diferentes acciones, como utilizar webhooks para que las alertas inicien acciones externas o se integren con las herramientas de administración de servicios de TI.

![Alertas](media/overview/alerts.png)

### <a name="autoscale"></a>Escalado automático
Gracias al escalado automático, puede ejecutar la cantidad correcta de recursos para administrar la carga de la aplicación. Permite crear reglas que usan las métricas recopiladas por Azure Monitor para determinar cuándo deben agregarse automáticamente recursos que controlen el aumento de la carga y permitan ahorrar dinero quitando recursos inactivos. Tiene que especificar un número mínimo y máximo de instancias y la lógica para decidir cuándo deben aumentar o disminuir los recursos.

![Escalado automático](media/overview/autoscale.png)

## <a name="visualizing-monitoring-data"></a>Visualización de los datos de supervisión
Las [visualizaciones](visualizations.md), como los gráficos y las tablas, son herramientas eficaces para resumir los datos de supervisión y presentarlos a distintos destinatarios. Azure Monitor cuenta con sus propias características para visualizar los datos de supervisión y utiliza otros servicios de Azure para publicarlos ante diferentes destinatarios.

### <a name="dashboards"></a>Paneles
Los [paneles de Azure](../azure-portal/azure-portal-dashboards.md) permiten combinar distintos tipos de datos, como métricas y registros, en un único panel de [Azure Portal](https://portal.azure.com). Si lo desea, también compartir el panel con otros usuarios de Azure. Los elementos que componen Azure Monitor pueden agregarse a un panel de Azure, así como a la salida de cualquier consulta de registro o gráfico de métricas. Por ejemplo, puede crear un panel que contenga diferentes iconos que muestren un gráfico de métricas, una tabla de registros de actividad, un gráfico de uso de Application Insights y la salida de una consulta de Log Analytics.

![panel](media/overview/dashboard.png)

### <a name="views"></a>Vistas
Las [vistas de Azure Monitor](../azure-monitor/platform/view-designer.md) representan visualmente los datos de registro de Log Analytics.  Cada vista contiene un icono que da acceso a una combinación de visualizaciones, como gráficos de barras y líneas, así como a listas en las que resumen los datos críticos.  Las soluciones de supervisión contienen vistas en las que se resumen los datos de una determinada aplicación. Puede crear sus propias vistas para representar los datos de cualquier búsqueda de registros de Log Analytics. Al igual que otros elementos de Azure Monitor, las vistas se pueden agregar a los paneles de Azure.

![Vídeos de Log Analytics](media/overview/view.png)

### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) es un servicio de análisis empresarial que proporciona visualizaciones interactivas en una serie de orígenes de datos y que constituye un mecanismo eficaz para poner los datos a disposición de personas que están dentro y fuera de la organización. Puede configurar Power BI para que [los datos de registro se importen automáticamente desde Azure Monitor](../log-analytics/log-analytics-powerbi.md) y utilizar estas otras visualizaciones.


![Power BI](media/overview/power-bi.png)


## <a name="integrate-and-export-data"></a>Integración y exportación de datos
A menudo, necesitará integrar Azure Monitor con otros sistemas y crear soluciones personalizadas que utilicen los datos de supervisión. Otros servicios de Azure funcionan con Azure Monitor para proporcionar esta integración.

### <a name="event-hub"></a>Centro de eventos
[Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs) es una plataforma de streaming y un servicio de ingesta de eventos que puede transformar y almacenar los datos utilizando cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento de datos. Utilice Event Hubs para [transmitir datos de registro desde Azure Monitor](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) a herramientas de supervisión y administración de eventos e información de seguridad de asociados.

> [!VIDEO https://www.youtube.com/embed/SPHxCgbcvSw]

### <a name="logic-apps"></a>Logic Apps
[Logic Apps](https://azure.microsoft.com/services/logic-apps) es un servicio que le permite automatizar tareas y procesos de negocio mediante flujos de trabajo que se integran con diferentes sistemas y servicios. Hay actividades disponibles que leen y escriben registros y métricas en Azure Monitor, lo que le permite crear flujos de trabajo que se integran con muchos otros sistemas.

![Aplicación lógica](../log-analytics/media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="api"></a>API
Existen varias API para leer y escribir métricas y registros en Azure Monitor, que además proporcionan acceso a las alertas generadas. También puede configurar y recuperar alertas. De este modo, dispone de unas posibilidades prácticamente ilimitadas para crear soluciones personalizadas que se integren con Azure Monitor.

## <a name="next-steps"></a>Pasos siguientes
Más información sobre:

* Las [métricas y registros](../azure-monitor/platform/data-collection.md) de los datos que recopila Azure Monitor.
* Los [orígenes de datos](../azure-monitor/platform/data-sources.md) de la forma en que los distintos componentes de la aplicación envían la telemetría.
* [Log Analytics](../log-analytics/log-analytics-queries.md): para analizar los datos recopilados.
