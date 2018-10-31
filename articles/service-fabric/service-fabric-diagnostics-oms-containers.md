---
title: Supervisión de contenedores en Azure Service Fabric con Log Analytics | Microsoft Docs
description: Use Log Analytics para supervisar los contenedores que se ejecutan en clústeres de Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: aabdae370c28f8fa633372be4505c00c25254408
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49403257"
---
# <a name="monitor-containers-with-log-analytics"></a>Supervisión de contenedores con Log Analytics
 
En este artículo se describen los pasos necesarios para configurar el contenedor de Azure Log Analytics que supervisa la solución para ver eventos de contenedor. Para configurar el clúster a fin de recopilar eventos de contenedor, consulte este [tutorial paso a paso](service-fabric-tutorial-monitoring-wincontainers.md). 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

## <a name="set-up-the-container-monitoring-solution"></a>Configuración de la solución de supervisión de contenedores

> [!NOTE]
> Log Analytics debe estar configurado en el clúster y el agente de Log Analytics implementado en los nodos. De lo contrario, siga en primer lugar los pasos descritos en [Set-up Log Analytics](service-fabric-diagnostics-oms-setup.md) (Configuración de Log Analytics) y [Add the Log Analytics agent to a cluster](service-fabric-diagnostics-oms-agent.md) (Incorporación del agente de Log Analytics a un clúster).

1. Una vez que el clúster esté configurado con Log Analytics y el agente de Log Analytics, implemente los contenedores. Espere a que los contenedores se implementen antes de pasar al paso siguiente.

2. En Azure Marketplace, busque *Container Monitoring Solution* y haga clic en el recurso **Container Monitoring Solution** que aparece debajo de la categoría Supervisión y administración.

    ![Adición de la solución Containers](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Cree la solución dentro de la misma área de trabajo que ya se ha creado para el clúster. Este cambio desencadena automáticamente el agente para iniciar la recopilación de datos de Docker en los contenedores. En unos 15 minutos, debería ver la solución iluminada con registros y estadísticas entrantes, como se muestra en la imagen siguiente.

    ![Panel básico de Log Analytics](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

El agente habilita la recopilación de varios registros específicos de contenedores que se pueden consultar en Log Analytics o se usan para visualizar indicadores de rendimiento. Los tipos de registro que se recopilan son:

* ContainerInventory: muestra información acerca de la ubicación, nombre e imágenes del contenedor
* ContainerImageInventory: información acerca de las imágenes implementadas, lo que incluye sus identificadores o tamaños
* ContainerLog: registros de error concretos, registros de docker (stdout, etc.) y otras entradas
* ContainerServiceLog: comandos de demonio de docker que se han ejecutado
* Perf: contadores de rendimiento, entre los que se incluyen la cpu del contenedor, la memoria, el tráfico de red, la e/s de disco y métricas personalizadas de los equipos host



## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre la [solución de contenedores de Azure Log Analytics](../log-analytics/log-analytics-containers.md).
* Obtenga más información sobre la orquestación de contenedores en Service Fabric en [Service Fabric y contenedores](service-fabric-containers-overview.md)
* Familiarícese con las funciones de [búsqueda de registros y consulta](../log-analytics/log-analytics-log-searches.md) que se ofrecen como parte de Log Analytics
* Configure en Log Analytics reglas de [alertas automáticas](../log-analytics/log-analytics-alerts.md) que ayuden en la detección y el diagnóstico