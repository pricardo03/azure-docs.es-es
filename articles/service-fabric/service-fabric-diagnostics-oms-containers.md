---
title: Supervisión de contenedores en Azure Service Fabric con los registros de Azure Monitor | Microsoft Docs
description: Usar registros de Azure Monitor para supervisar los contenedores que se ejecutan en clústeres de Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: d03d68560502821b9c343be983d9f7b5a83ed977
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60321917"
---
# <a name="monitor-containers-with-azure-monitor-logs"></a>Supervisión de contenedores con los registros de Azure Monitor
 
En este artículo se trata los pasos necesarios para configurar la solución de supervisión de contenedores de Azure Monitor registros para ver los eventos de contenedor. Para configurar el clúster a fin de recopilar eventos de contenedor, consulte este [tutorial paso a paso](service-fabric-tutorial-monitoring-wincontainers.md). 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="set-up-the-container-monitoring-solution"></a>Configuración de la solución de supervisión de contenedores

> [!NOTE]
> Debe tener Azure Monitor registros configuración para el clúster así como el agente de Log Analytics implementado en los nodos. Si no, siga los pasos de [configurar registros de Azure Monitor](service-fabric-diagnostics-oms-setup.md) y [agregar el agente de Log Analytics a un clúster](service-fabric-diagnostics-oms-agent.md) primero.

1. Una vez que el clúster se ha configurado con los registros de Azure Monitor y el agente de Log Analytics, implemente los contenedores. Espere a que los contenedores se implementen antes de pasar al paso siguiente.

2. En Azure Marketplace, busque *Container Monitoring Solution* y haga clic en el recurso **Container Monitoring Solution** que aparece debajo de la categoría Supervisión y administración.

    ![Adición de la solución Containers](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Cree la solución dentro de la misma área de trabajo que ya se ha creado para el clúster. Este cambio desencadena automáticamente el agente para iniciar la recopilación de datos de Docker en los contenedores. En unos 15 minutos, debería ver la solución iluminada con registros y estadísticas entrantes, como se muestra en la imagen siguiente.

    ![Panel básico de Log Analytics](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

El agente habilita la recopilación de varios registros específicos de contenedores que se pueden consultar en los registros de Azure Monitor, o usar para visualizar los indicadores de rendimiento. Los tipos de registro que se recopilan son:

* ContainerInventory: muestra información acerca de la ubicación, nombre e imágenes del contenedor
* ContainerImageInventory: información acerca de las imágenes implementadas, lo que incluye sus identificadores o tamaños
* ContainerLog: registros de error concretos, registros de docker (stdout, etc.) y otras entradas
* ContainerServiceLog: comandos de demonio de docker que se han ejecutado
* Perf: contadores de rendimiento, entre los que se incluyen la cpu del contenedor, la memoria, el tráfico de red, la e/s de disco y métricas personalizadas de los equipos host



## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre [Azure Monitor registra la solución Containers](../azure-monitor/insights/containers.md).
* Obtenga más información sobre la orquestación de contenedores en Service Fabric en [Service Fabric y contenedores](service-fabric-containers-overview.md)
* Familiarícese con la [búsqueda de registros y realizar consultas](../log-analytics/log-analytics-log-searches.md) características se ofrecen como parte de los registros de Azure Monitor
* Configurar registros de Azure Monitor para configurar [alertas automáticas](../log-analytics/log-analytics-alerts.md) reglas para ayudar en la detección y diagnóstico