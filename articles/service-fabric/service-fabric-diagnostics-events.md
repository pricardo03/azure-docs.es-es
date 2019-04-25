---
title: Eventos de Azure Service Fabric | Microsoft Docs
description: Obtenga información acerca de los eventos de Service Fabric que le proporcionarán ayuda para supervisar el clúster de Azure Service Fabric.
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
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: b4270b9438a397ec09537c9d6343515ebc21af98
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60393051"
---
# <a name="service-fabric-events"></a>Eventos de Service Fabric 

La plataforma Service Fabric escribe varios eventos estructurados para actividades operativas clave que se producen dentro del clúster. Estas van desde las actualizaciones del clúster hasta las decisiones de ubicación de réplicas. Cada evento que Service Fabric expone se asigna a una de las siguientes entidades en el clúster:
* Clúster
* Application
* Servicio
* Partition
* Réplica 
* Contenedor

Para ver una lista completa de los eventos que expone la plataforma, consulte: [eventos de la lista de Service Fabric](service-fabric-diagnostics-event-generation-operational.md).

Estos son algunos ejemplos de escenarios en los que puede ver los eventos para el clúster. 
* Eventos de ciclo de vida del nodo: a medida que los nodos ascienden, descienden, se reducen o escalan horizontalmente, se activan o desactivan, los eventos que se expondrán le mostrarán qué ha ocurrido, y le ayudarán a identificar si hay algún problema con la propia máquina o si existe una API que se llamó a través de SF para modificar el estado de un nodo.
* Actualización del clúster: cuando el clúster se actualiza (la versión o la configuración de SF cambiará), puede comprobar cómo se inicia la actualización, avanza por cada uno de los dominios de actualización y se completa (o revierte). 
* Actualizaciones de aplicaciones: igual que las actualizaciones de clúster, hay un conjunto completo de eventos a medida que avanza la actualización. Estos eventos pueden ser útiles para entender el momento en que se programa una actualización, el estado actual de la misma y la secuencia general de eventos. Esto es útil para ver qué actualizaciones se han realizado correctamente o si se ha desencadenado una reversión.
* Implementar o eliminar una aplicación o servicio: hay eventos para cada aplicación, servicio y contenedor que se vaya a crear o eliminar. Resultan útiles para reducir o escalar horizontalmente, por ejemplo, para aumentar el número de réplicas.
* Movimientos de partición (reconfiguración): cada vez que se lleva a cabo una reconfiguración (un cambio en el conjunto de réplicas) en una partición con estado, se registra un evento. Esto es útil si desea saber cada cuánto cambia o se conmuta por error el conjunto de réplicas de la partición, o si quiere realizar un seguimiento para saber en cualquier momento qué nodo estaba ejecutando la réplica principal.
* Eventos de caos: al usar el servicio [Chaos](service-fabric-controlled-chaos.md) de Service Fabric, verá los eventos cada vez que se inicie o detenga el servicio, o cuando inserte un error en el sistema.
* Eventos de estado: Service Fabric expone eventos de estado cada vez que se crea una advertencia o un informe de estado de Error, o una entidad vuelve a un estado correcto o un informe de mantenimiento expira. Estos eventos son muy útiles para realizar el seguimiento de las estadísticas de mantenimiento históricas de una entidad. 

## <a name="how-to-access-events"></a>Cómo obtener acceso a eventos

Hay varias maneras mediante las cuales se puede obtener acceso a eventos de Service Fabric:
* Los eventos se registran a través de canales estándares como registros de eventos ETW/Windows y se pueden visualizar mediante la herramienta de supervisión que admite estos como los registros de Azure Monitor. De forma predeterminada, los clústeres creados en el portal tienen activado el diagnóstico y tienen el agente de diagnósticos de Windows Azure que envía los eventos en Azure table storage, pero deberá integrar esto con el recurso de log analytics. Leer más acerca de cómo configurar el [agente de Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) para modificar la configuración de diagnósticos de clúster para seleccionar más registros o contadores de rendimiento y la [integración de registros de Azure Monitor](service-fabric-diagnostics-event-analysis-oms.md)
* Las API de REST del servicio EventStore que le permiten consultar el clúster directamente o a través de la biblioteca cliente de Service Fabric. Consulte [Query EventStore APIs for cluster events](service-fabric-diagnostics-eventstore-query.md) (Consultar las API del servicio EventStore para obtener eventos de clúster).

## <a name="next-steps"></a>Pasos siguientes
* Obtener más información acerca de cómo supervisar el clúster: [Supervisión del clúster y la plataforma](service-fabric-diagnostics-event-generation-infra.md).
* Obtener más información sobre el servicio EventStore: - [EventStore service overview](service-fabric-diagnostics-eventstore.md) (Información general sobre el servicio EventStore).
