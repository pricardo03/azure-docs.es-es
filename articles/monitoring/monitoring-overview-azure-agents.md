---
title: Introducción a los agentes de supervisión de Azure | Microsoft Docs
description: En este artículo se proporciona una descripción detallada de los agentes de Azure disponibles que permiten supervisar las máquinas virtuales de Azure.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: magoedte
ms.openlocfilehash: 81db6720422de111cc5b390c58e9020d7c19f90a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282040"
---
# <a name="overview-of-the-azure-agents-to-monitor-azure-virtual-machines"></a>Introducción a los agentes de Azure para supervisar máquinas virtuales de Azure
Microsoft Azure ofrece varias maneras de recopilar diferentes tipos de datos de máquinas virtuales hospedadas en Azure u otros proveedores de nube que ejecutan Microsoft Windows y Linux.  Este artículo le ayudará entender las diferencias y las funcionalidades disponibles de cada agente para poder determinar cuál será el adecuado para la administración de servicios y los requisitos de supervisión generales.  

## <a name="comparing-azure-diagnostic-and-log-analytics-agent"></a>Comparación del agente de Azure Diagnostic y Log Analytics
Actualmente, en Azure hay dos tipos de agentes disponibles para supervisar una máquina virtual de Azure: la extensión Azure Diagnostics y el agente de Log Analytics para Linux y Windows.  Estos agentes están diseñados fundamentalmente para recopilar métricas y registros y reenviarlos a un repositorio. Sin embargo, ahí terminan sus similitudes.  

La [extensión Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md), incluida en Azure Cloud Services desde que empezó a estar disponible con carácter general en 2010, es un agente que ofrece una colección simple de datos de diagnóstico de un recurso IaaS de Azure, como una máquina virtual, y almacena los datos en Azure Storage.  Una vez en el almacenamiento, puede verlos con una de las diversas herramientas disponibles, como el [Explorador de servidores en Visual Studio](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) y el [Explorador de Azure Storage](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Ahora puede elegir recopilar:

* Un conjunto predefinido de contadores de rendimiento del sistema operativo y registros de eventos, o bien puede especificar qué desea recopilar. 
* Todas las solicitudes o las solicitudes erróneas a un servidor web IIS
* Registros de salida de seguimiento de aplicación de .NET
* Eventos de Seguimiento de eventos para Windows (ETW) 
* Recopilar eventos de registro de syslog  
* Volcados de memoria 

Los datos también se puede reenviar a [Application Insights](../application-insights/app-insights-cloudservices.md), [Log Analytics](../log-analytics/log-analytics-queries.md) o a servicios que no son de Azure mediante [Event Hubs](../event-hubs/event-hubs-about.md). 

Para realizar una supervisión avanzada en la que deba hacer más que recopilar métricas y un subconjunto de registros, se necesita el agente de Log Analytics para Windows y Linux.  Con este agente, podrá utilizar los servicios de Azure como Automation y Log Analytics, incluido el conjunto completo de características que ofrecen, para realizar una administración integral de las máquinas virtuales de Azure en todo su ciclo de vida. Esto incluye:

* [Azure Automation Update Management](../automation/automation-update-management.md) para las actualizaciones del sistema operativo
* [Azure Automation Desired State Configuration](../automation/automation-dsc-overview.md) para mantener coherente el estado de la configuración
* Realice el seguimiento de los cambios de configuración con [Azure Automation Change Tracking and Inventory](../automation/automation-change-tracking.md)
* Recopilación de registros personalizados del sistema operativo y las aplicaciones hospedadas como [FluentD](../log-analytics/log-analytics-data-sources-json.md), [registros personalizados](../log-analytics/log-analytics-data-sources-custom-logs.md), [MySQL y Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) con Log Analytics
* De forma nativa, los servicios de Azure como [Application Insights](https://docs.microsoft.com/azure/application-insights/) y [Azure Security Center](https://docs.microsoft.com/azure/security-center/) almacenan sus datos directamente en Log Analytics sin necesidad de ninguna configuración.  

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Recopilar datos de equipos en su entorno con Log Analytics](../log-analytics/log-analytics-concept-hybrid.md) para ver los requisitos y métodos disponibles para implementar el agente en equipos de su centro de datos o de otro entorno en la nube.
- Consulte [Recopilación de datos de máquinas virtuales de Azure](../log-analytics/log-analytics-quick-collect-azurevm.md) para configurar la recopilación de datos de máquinas virtuales de Azure. 
