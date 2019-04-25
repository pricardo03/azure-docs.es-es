---
title: Introducción a la supervisión de Azure Containers| Microsoft Docs
description: En este artículo se proporciona información general sobre los diferentes métodos disponibles en Azure para supervisar el servicio Containers de Azure a fin de conocer rápidamente el estado de los clústeres y su disponibilidad.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: d137576b4beb5cf36dce99ffb1869049f37b60b2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60494650"
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Introducción a la supervisión de contenedores en Azure
Con Azure, puede supervisar y administrar de manera efectiva las cargas de trabajo implementadas en contenedores de Azure que ejecutan Kubernetes o Docker. Es importante entender qué tal funcionan los contenedores con varias aplicaciones de microservicio a fin de proporcionar un servicio confiable a escala y respaldar el plan de supervisión. En este artículo se proporciona una breve introducción a las funcionalidades de administración y supervisión de Azure para ayudarle a entenderlas y cuáles son adecuadas para sus necesidades.

Con [Azure Monitor para contenedores](container-insights-overview.md), puede ver de un vistazo el rendimiento y el estado de mantenimiento de la infraestructura de contenedores Linux e investigar los problemas rápidamente. La telemetría se almacena en un área de trabajo de Log Analytics y se integra en Azure Portal, donde puede explorar, filtrar y segmentar los datos agregados con paneles para vigilar la carga, el rendimiento y el mantenimiento.  

En el caso de contenedores que se ejecutan fuera del servicio hospedado de Azure Kubernetes, la [solución de contenedores de Windows y Docker](../../azure-monitor/insights/containers.md) de Log Analytics le ayuda a ver y administrar los hosts de contenedor de Windows y Docker. Desde el área de trabajo de Log Analytics, puede ver los detalles de inventario, el rendimiento y los eventos de nodos y contenedores del entorno. Asimismo, puede ver información detallada de auditoría que muestra los comandos usados con los contenedores, y puede ver y buscar registros centralizados para solucionar los problemas de los contenedores, sin necesidad de acceder de forma remota a los hosts de Docker o Windows.

Para conseguir la supervisión holística o de un extremo a otro de la aplicación, es necesario supervisar todas las dependencias con Azure Monitor o Log Analytics, tanto si se trata de un recurso de Azure como de uno local.  La capa de aplicación se debe incluir con el fin de agregar una capa adicional de reconocimiento de mantenimiento, tanto en el nivel de plataforma como de aplicación con Application Insights. En el nivel de plataforma, hay varios SDK de Application Insights para [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/) y [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights). Para aplicaciones de microservicios, hay compatibilidad para [Java](../../azure-monitor/app/java-get-started.md), [Node.js](../../azure-monitor/learn/nodejs-quick-start.md), [.NET](../../azure-monitor/app/asp-net.md), [.NET Core](../../azure-monitor/app/asp-net-core.md), así como un número de otros [lenguajes y marcos](../../azure-monitor/app/platforms.md). 

En caso contrario, no se identificarán los problemas, lo que podría afectar a la disponibilidad de la aplicación y los objetivos de nivel de servicio no se cumplirán.  
