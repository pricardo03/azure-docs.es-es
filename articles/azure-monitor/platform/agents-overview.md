---
title: Introducción a los agentes de supervisión de Azure | Microsoft Docs
description: En este artículo se proporciona una descripción detallada de los agentes de Azure disponibles que permiten supervisar las máquinas virtuales hospedadas en Azure o el entorno híbrido.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: magoedte
ms.openlocfilehash: d1e896aee4ba699704ce01e0cff8210d53700993
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57899815"
---
# <a name="overview-of-the-azure-monitoring-agents"></a>Introducción a los agentes de supervisión de Azure 
Microsoft Azure ofrece varias maneras de recopilar diferentes tipos de datos de máquinas virtuales hospedadas en Azure, en su centro de datos u otros proveedores de servicios en la nube que ejecutan Microsoft Windows y Linux. Los tres tipos de agentes disponibles para supervisar una VM son:

* Extensiones Azure Diagnostics
* Agente de Log Analytics para Linux y Windows
* Dependency Agent

En este artículo se describen las diferencias entre ellos y sus funcionalidades para poder determinar cuál será el adecuado para la administración de servicios de TI y los requisitos de supervisión generales.  

## <a name="azure-diagnostic-extension"></a>Extensión Azure Diagnostics
La [extensión Azure Diagnostics](../../azure-monitor/platform/diagnostics-extension-overview.md) [a la que normalmente se hace referencia como extensión Azure Diagnostics para Windows (WAD) o Azure Diagnostics para Linux (LAD)], incluida en Azure Cloud Services desde que empezó a estar disponible con carácter general en 2010, es un agente que ofrece una colección simple de datos de diagnóstico de un recurso de procesos de Azure, como una VM, y almacena los datos en Azure Storage. Una vez en el almacenamiento, puede verlos con una de las diversas herramientas disponibles, como el [Explorador de servidores en Visual Studio](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) y el [Explorador de Azure Storage](../../vs-azure-tools-storage-manage-with-storage-explorer.md).

Ahora puede elegir recopilar:

* Un conjunto predefinido de contadores de rendimiento del sistema operativo y registros de eventos, o bien puede especificar qué desea recopilar. 
* Todas las solicitudes o las solicitudes erróneas a un servidor web IIS
* Registros de salida de seguimiento de aplicación de .NET
* Eventos de Seguimiento de eventos para Windows (ETW) 
* Recopilar eventos de registro de syslog  
* Volcados de memoria 

El agente de Azure Diagnostics se debe usar cuando quiera realizar alguna de las siguientes acciones:

* Archivar registros y métricas en Azure Storage.
* Integrar datos de supervisión con herramientas de terceros. Estas herramientas usan una variedad de métodos, incluidas las consultas de la cuenta de almacenamiento, que se reenvían a [Event Hubs](../../event-hubs/event-hubs-about.md), o consultas con la [API REST de supervisión de Azure](../../azure-monitor/platform/rest-api-walkthrough.md).
* Cargar datos en Azure Monitor para crear gráficos de métricas en Azure Portal o [alertas de métricas](../../azure-monitor/platform/alerts-metric-overview.md) casi en tiempo real. 
* Escalar automáticamente conjuntos de escalado de máquinas virtuales y Cloud Services clásicos en función de las métricas de SO invitado.
* Investigar los problemas de arranque de VM con [diagnósticos de arranque](../../virtual-machines/troubleshooting/boot-diagnostics.md).
* Comprender cómo funcionan las aplicaciones e identificar de manera proactiva los problemas que les afectan con [Application Insights](../../azure-monitor/overview.md).
* Configurar Log Analytics para importar las métricas y registrar los datos recopilados de Cloud Services, las VM clásicas y los nodos de Service Fabric que se almacenan en una cuenta de almacenamiento de Azure.

## <a name="log-analytics-agent"></a>Agente de Log Analytics
Para la supervisión avanzada donde tiene que recopilar más de un subconjunto de registros y métricas, se necesita el agente de Log Analytics para Windows (también conocido como Microsoft Monitoring Agent (MMA)) y Linux. El agente de Log Analytics se desarrolló para la administración integral en máquinas virtuales y físicas en el entorno local, equipos supervisados por System Center Operations Manager y VM hospedadas en otras nubes. Los agentes de Windows y Linux se conectan a un área de trabajo de Log Analytics para recopilar datos basados en soluciones de supervisión, así como los orígenes de datos personalizados que configure.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

El agente de Log Analytics se debe usar cuando quiera realizar alguna de las siguientes acciones:

* Recopilar datos de diversos orígenes, tanto en Azure como en otros proveedores de servicios en la nube o recursos locales. 
* Mediante una de las soluciones de supervisión de Azure Monitor, como [Azure Monitor para VM](../insights/vminsights-overview.md), [Azure Monitor para contenedores](../insights/container-insights-overview.md), etc.  
* Usar uno de los otros servicios de administración de Azure, como [Azure Security Center](../../security-center/security-center-intro.md), [Azure Automation](../../automation/automation-intro.md), etc.

Anteriormente, se agruparon varios servicios de Azure como *Operations Management Suite*, y, como resultado, el agente de Log Analytics se compartió entre los servicios, como Azure Security Center y Azure Automation.  Esto incluye el conjunto completo de características que proporcionan, ofreciendo la administración integral de las VM de Azure a través de su ciclo de vida.  A continuación se indican algunos ejemplos:

* [Azure Automation Update Management](../../automation/automation-update-management.md) para las actualizaciones del sistema operativo.
* [Azure Automation Desired State Configuration](../../automation/automation-dsc-overview.md) para mantener coherente el estado de la configuración.
* Realice un seguimiento de los cambios de configuración con [Azure Automation Change Tracking and Inventory](../../automation/automation-change-tracking.md).
* Servicios de Azure como [Application Insights](https://docs.microsoft.com/azure/application-insights/) y [Azure Security Center](https://docs.microsoft.com/azure/security-center/), que almacenan de forma nativa sus datos directamente en Log Analytics sin necesidad de ninguna configuración.  

## <a name="dependency-agent"></a>Dependency Agent
Dependency Agent se desarrolló como parte de la solución de Service Map, desarrollada originalmente de forma externa a Microsoft. [Service Map](../insights/service-map.md) y [Azure Monitor para VM](../insights/vminsights-overview.md) requieren un agente de Dependency Agent en las máquinas virtuales de Windows y Linux que se integra con el agente de Log Analytics para recopilar datos detectados acerca de los procesos que se ejecutan en la máquina virtual y en las dependencias de procesos externos. Almacena estos datos en Log Analytics y visualiza los componentes interconectados detectados.

Puede que necesite una combinación de estos agentes para supervisar la VM. Los agentes se pueden instalar en paralelo como extensiones de Azure, sin embargo, en Linux, el agente de Log Analytics *debe* instalarse primero o la instalación producirá un error. 

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Introducción al agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md) para revisar los requisitos y los métodos admitidos para implementar el agente en máquinas hospedadas en Azure, en su centro de datos o en otro entorno en la nube.

