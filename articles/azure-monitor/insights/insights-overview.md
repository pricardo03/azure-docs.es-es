---
title: Información general de Insights en Azure Monitor | Microsoft Docs
description: Insights proporcionan una experiencia de supervisión personalizada en Azure Monitor para determinadas aplicaciones y servicios. En este artículo se proporciona una breve descripción de cada uno de la información que están actualmente disponible.
services: azure-monitor
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: bwren
ms.openlocfilehash: 8cb39a174c570b7019e872d731f49252a9505406
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247235"
---
# <a name="overview-of-insights-in-azure-monitor"></a>Información general de Insights en Azure Monitor
Insights proporcionan una experiencia de supervisión personalizada para los servicios y aplicaciones concretas. Almacenan datos en el [plataforma de datos de Azure Monitor](../platform/data-platform.md) y aprovechan otras características de Azure Monitor para el análisis y alertas, pero puede recopilar datos adicionales y proporcionar una experiencia de usuario único en el portal de Azure. Obtener acceso a información a partir de la **Insights** sección del menú de Azure Monitor en Azure portal.

Las secciones siguientes proporcionan una breve descripción de la información que están actualmente disponibles en Azure Monitor. Consulte la documentación para obtener más información detallada sobre cada.

## <a name="application-insights"></a>Application Insights
Application Insights es un servicio de Application Performance Management (APM) extensible para desarrolladores web en varias plataformas. Úselo para supervisar la aplicación web en directo. Funciona en una amplia variedad de plataformas como. NET, Node.js y Java EE, para las aplicaciones hospedadas en cualquier nube pública, híbrida o en el entorno local. También se integra con el proceso de DevOps y tiene puntos de conexión a una variedad de herramientas de desarrollo.

Consulte [¿qué es Application Insights?](../app/app-insights-overview.md).

![Application Insights](media/insights-overview/app-insights.png)

## <a name="azure-monitor-for-containers"></a>Azure Monitor para contenedores
Azure Monitor para contenedores supervisa el rendimiento de las cargas de trabajo de contenedor implementadas en Azure Container Instances o en clústeres de Kubernetes administrados hospedados en Azure Kubernetes Service (AKS). La supervisión de los contenedores es fundamental, sobre todo cuando se ejecuta un clúster de producción, a escala, con varias aplicaciones.

Consulte [Azure Monitor para la introducción a los contenedores](../insights/container-insights-overview.md).

![Azure Monitor para contenedores](media/insights-overview/container-insights.png)

## <a name="azure-monitor-for-resource-groups-preview"></a>Azure Monitor para grupos de recursos (versión preliminar)
Azure Monitor para grupos de recursos ayuda a evaluar las prioridades y diagnosticar los problemas que encuentre los recursos individuales, al tiempo que ofrece contexto sobre el estado y el rendimiento del grupo de recursos como un todo.

Consulte [supervisar grupos de recursos con Azure Monitor (versión preliminar)](../insights/resource-group-insights.md).

![Azure Monitor para grupos de recursos](media/insights-overview/resource-group-insights.png)

## <a name="azure-monitor-for-vms-preview"></a>Azure Monitor para las máquinas virtuales (versión preliminar)
Azure Monitor para VM supervisa las máquinas virtuales (VM) y los conjuntos de escalado de máquinas virtuales de Azure. El servicio analiza el rendimiento y el estado de las VM Windows y Linux, y supervisa sus procesos y dependencias en otros recursos y procesos externos.

Consulte [¿qué es Azure Monitor para las máquinas virtuales?](vminsights-overview.md)

![Azure Monitor para máquinas virtuales](media/insights-overview/vm-insights.png)

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre la [plataforma de datos de Azure Monitor](../platform/data-platform.md) aprovechar Insights.
* Obtenga información sobre los diferentes [orígenes de datos usados por Azure Monitor](../platform/data-sources.md) y los diferentes tipos de datos recopilados por cada uno de los conocimientos.
