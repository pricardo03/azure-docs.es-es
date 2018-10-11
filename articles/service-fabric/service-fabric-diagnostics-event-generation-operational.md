---
title: Lista de eventos de Azure Service Fabric | Microsoft Docs
description: Lista completa de eventos proporcionados por Azure Service Fabric como ayuda para supervisar clústeres.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 03dac03405588ba00a0f8ca5b127956c40853e36
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868520"
---
# <a name="list-of-service-fabric-events"></a>Lista de eventos de Service Fabric 

Service Fabric expone un conjunto principal de eventos de clúster para informarle sobre el estado del clúster como [eventos de Service Fabric](service-fabric-diagnostics-events.md). Estos se basan en las acciones realizadas por Service Fabric en los nodos y el clúster, o las decisiones de administración tomadas por un operador/propietario del clúster. Para tener acceso a estos eventos, puede consultar [EventStore](service-fabric-diagnostics-eventstore.md) en el clúster o a través del canal operativo. En equipos con Windows, el canal operativo también está enlazado a EventLog, por lo que puede ver los eventos de Service Fabric en el Visor de eventos. 

>[!NOTE]
>Para obtener una lista de eventos de Service Fabric para clústeres en versiones anteriores a la 6.2, consulte la sección siguiente. 

Aquí tiene una lista de todos los eventos disponible en la plataforma, ordenada por la entidad a la que están asignados.

## <a name="cluster-events"></a>Eventos de clúster

**Eventos de actualización de clústeres**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStarted | Se ha iniciado una actualización de clúster | CM | Informativo | 1 |
| 29628 | ClusterUpgradeCompleted | Se ha terminado una actualización de clúster| CM | Informativo | 1 |
| 29629 | ClusterUpgradeRollbackStarted | Se ha iniciado la reversión de una actualización de clúster | CM | Informativo | 1 |
| 29630 | ClusterUpgradeRollbackCompleted | Se ha terminado la reversión de una actualización de clúster | CM | Informativo | 1 |
| 29631 | ClusterUpgradeDomainCompleted | Se ha completado una actualización de dominio durante una actualización de clúster | CM | Informativo | 1 |

## <a name="node-events"></a>Eventos de nodos

**Eventos del ciclo de vida de los nodos** 

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 18602 | NodeDeactivateCompleted | Se ha completado la desactivación de un nodo | FM | Informativo | 1 |
| 18603 | NodeUp | El clúster ha detectado que un nodo se ha iniciado | FM | Informativo | 1 |
| 18604 | NodeDown | El clúster ha detectado que un nodo se ha apagado |  FM | Informativo | 1 |
| 18605 | NodeAddedToCluster | Se ha agregado un nuevo nodo al clúster | FM | Informativo | 1 |
| 18606 | NodeRemovedFromCluster | Se ha eliminado un nodo del clúster | FM | Informativo | 1 |
| 18607 | NodeDeactivateStarted | Se ha iniciado la desactivación de un nodo | FM | Informativo | 1 |
| 25620 | NodeOpening | Un nodo se está iniciando. Primera fase del ciclo de vida del nodo | FabricNode | Informativo | 1 |
| 25621 | NodeOpenSucceeded | Un nodo se inició correctamente | FabricNode | Informativo | 1 |
| 25622 | NodeOpenFailed | No se puedo iniciar un nodo | FabricNode | Informativo | 1 |
| 25623 | NodeClosing | Un nodo se está apagando. Inicio de la fase final del ciclo de vida del nodo | FabricNode | Informativo | 1 |
| 25624 | NodeClosed | Un nodo se apagó correctamente | FabricNode | Informativo | 1 |
| 25625 | NodeAborting | Un nodo se empieza a apagar de manera brusca | FabricNode | Informativo | 1 |
| 25626 | NodeAborted | Un nodo se ha apagado de manera brusca | FabricNode | Informativo | 1 |

## <a name="application-events"></a>Eventos de aplicación

**Eventos del ciclo de vida de aplicaciones**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 29620 | ApplicationCreated | Se ha creado una nueva aplicación | CM | Informativo | 1 |
| 29625 | ApplicationDeleted | Se ha eliminado una aplicación existente | CM | Informativo | 1 |
| 23083 | ApplicationProcessExited | Un proceso de una aplicación ha salido | Hospedaje | Informativo | 1 |

**Eventos de actualización de aplicaciones**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 29621 | ApplicationUpgradeStarted | Se ha iniciado una actualización de aplicación | CM | Informativo | 1 |
| 29622 | ApplicationUpgradeCompleted | Se ha terminado una actualización de aplicación | CM | Informativo | 1 |
| 29623 | ApplicationUpgradeRollbackStarted | Se ha iniciado la reversión de una actualización de aplicación |CM | Informativo | 1 |
| 29624 | ApplicationUpgradeRollbackCompleted | Se ha terminado la reversión de una actualización de aplicación | CM | Informativo | 1 |
| 29626 | ApplicationUpgradeDomainCompleted | Se ha completado una actualización de dominio durante una actualización de aplicación | CM | Informativo | 1 |

## <a name="service-events"></a>Eventos de servicios

**Eventos de ciclo de vida de servicios**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | Se ha creado un nuevo servicio | FM | Informativo | 1 |
| 18658 | ServiceDeleted | Se ha eliminado un servicio existente | FM | Informativo | 1 |

## <a name="partition-events"></a>Eventos de particiones

**Eventos de movimiento de particiones**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | Se ha terminado la reconfiguración de una partición | RA | Informativo | 1 |

## <a name="container-events"></a>Eventos de contenedores

**Eventos de ciclo de vida de contenedores** 

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Se ha iniciado un contenedor | Hospedaje | Informativo | 1 |
| 23075 | ContainerDeactivated | Se ha detenido un contenedor | Hospedaje | Informativo | 1 |
| 23082 | ContainerExited | Ha salido un contenedor. Compruebe la marca UnexpectedTermination | Hospedaje | Informativo | 1 |

## <a name="health-reports"></a>Informes de mantenimiento

**Eventos de informe de estado de clústeres**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Hay un nuevo informe de estado de clúster disponible | HM | Informativo | 1 |
| 54437 | ClusterHealthReportExpired | Un informe de estado de clúster existente ha expirado | HM | Informativo | 1 |

**Eventos de informe de estado de nodos**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Hay un nuevo informe de estado de nodo disponible | HM | Informativo | 1 |
| 54432 | NodeHealthReportExpired | Un informe de estado de nodo existente ha expirado | HM | Informativo | 1 |

**Eventos de informe de estado de aplicaciones**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Se ha creado un informe de estado de aplicación. Para aplicaciones no implementadas. | HM | Informativo | 1 |
| 54426 | DeployedApplicationNewHealthReport | Se ha creado un informe de estado de aplicación implementada | HM | Informativo | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Se ha creado un informe de estado de servicio implementado | HM | Informativo | 1 |
| 54434 | ApplicationHealthReportExpired | Un informe de estado de aplicación existente ha expirado | HM | Informativo | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Un informe de estado de aplicación implementada existente ha expirado | HM | Informativo | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Un informe de estado de servicio implementado existente ha expirado | HM | Informativo | 1 |

**Eventos de informe de estado de servicios**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Se ha creado un informe de estado de servicio | HM | Informativo | 1 |
| 54433 | ServiceHealthReportExpired | Un informe de estado de servicio existente ha expirado | HM | Informativo | 1 |

**Eventos de informe de estado de particiones**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Se ha creado un informe de estado de partición | HM | Informativo | 1 |
| 54431 | PartitionHealthReportExpired | Un informe de estado de partición existente ha expirado | HM | Informativo | 1 |

**Eventos de informe de estado de réplicas**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Se ha creado un informe de estado de réplica con estado | HM | Informativo | 1 |
| 54430 | StatelessInstanceNewHealthReport | Se ha creado un informe de estado de instancia sin estado | HM | Informativo | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Un informe de estado de réplica con estado existente ha expirado | HM | Informativo | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Un informe de estado de instancia sin estado existente ha expirado | HM | Informativo | 1 |

## <a name="chaos-testing-events"></a>Eventos de pruebas de caos 

**Eventos de sesión de caos**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Se ha iniciado una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |
| 50023 | ChaosStopped | Se ha detenido una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |

**Eventos de nodos de Chaos**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Se ha programado el reinicio de un nodo como parte de una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |
| 50087 | ChaosNodeRestartCompleted | Un nodo ha terminado el reinicio como parte de una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |

**Eventos de aplicación Chaos**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Se ha programado un reinicio del paquete de código durante una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Se ha completado el reinicio del paquete de código durante una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |

**Eventos de particiones de Chaos**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Se ha programado la migración de una partición principal como parte de una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | Se ha programado la migración de una partición secundaria como parte de una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | El análisis en profundidad de la migración de la partición principal está disponible | Capacidad de prueba | Informativo | 1 |

**Eventos de réplicas de Chaos**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Se ha programado un reinicio de una réplica como parte de una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |
| 50051 | ChaosReplicaRemovalScheduled | Se ha programado una eliminación de réplica como parte de una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Se ha terminado una eliminación de réplica como parte de una sesión de pruebas de caos | Capacidad de prueba | Informativo | 1 |

## <a name="other-events"></a>Otros eventos

**Eventos de correlaciones**

| EventId | NOMBRE | DESCRIPCIÓN |Origen (tarea) | Nivel | Versión |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Se ha detectado una correlación | Capacidad de prueba | Informativo | 1 |

## <a name="events-prior-to-version-62"></a>Eventos anteriores a la versión 6.2

A continuación figura una lista completa de los eventos proporcionados por Service Fabric antes de la versión 6.2.

| EventId | NOMBRE | Origen (tarea) | Nivel |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Informativo |
| 25621 | NodeOpenedSuccess | FabricNode | Informativo |
| 25622 | NodeOpenedFailed | FabricNode | Informativo |
| 25623 | NodeClosing | FabricNode | Informativo |
| 25624 | NodeClosed | FabricNode | Informativo |
| 25625 | NodeAborting | FabricNode | Informativo |
| 25626 | NodeAborted | FabricNode | Informativo |
| 29627 | ClusterUpgradeStart | CM | Informativo |
| 29628 | ClusterUpgradeComplete | CM | Informativo |
| 29629 | ClusterUpgradeRollback | CM | Informativo |
| 29630 | ClusterUpgradeRollbackComplete | CM | Informativo |
| 29631 | ClusterUpgradeDomainComplete | CM | Informativo |
| 23074 | ContainerActivated | Hospedaje | Informativo |
| 23075 | ContainerDeactivated | Hospedaje | Informativo |
| 29620 | ApplicationCreated | CM | Informativo |
| 29621 | ApplicationUpgradeStart | CM | Informativo |
| 29622 | ApplicationUpgradeComplete | CM | Informativo |
| 29623 | ApplicationUpgradeRollback | CM | Informativo |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Informativo |
| 29625 | ApplicationDeleted | CM | Informativo |
| 29626 | ApplicationUpgradeDomainComplete | CM | Informativo |
| 18566 | ServiceCreated | FM | Informativo |
| 18567 | ServiceDeleted | FM | Informativo |

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre la [generación de eventos general en el nivel de plataforma ](service-fabric-diagnostics-event-generation-infra.md) en Service Fabric
* Modificación de la configuración de [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) para recopilar más registros
* [Configuración de Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) para ver los registros de canal operativo
