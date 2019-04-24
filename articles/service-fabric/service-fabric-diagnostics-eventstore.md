---
title: Almacén de eventos de Azure Service Fabric | Microsoft Docs
description: Información acerca de EventStore de Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2019
ms.author: srrengar
ms.openlocfilehash: 36d01a9e6e55ae54377ba3f983f779dbc692c49a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392931"
---
# <a name="eventstore-service-overview"></a>Información general del servicio EventStore

>[!NOTE]
>A partir de la versión 6.4 de Service Fabric, las API de EventStore solo se encuentran disponibles para los clústeres Windows que se ejecutan en Azure. Se está trabajando para portar esta funcionalidad a Linux, así como nuestros clústeres independientes.

## <a name="overview"></a>Información general

El servicio EventStore, que se introdujo en la versión 6.2, es una opción de supervisión de Service Fabric. EventStore proporciona una manera de comprender el estado del clúster o las cargas de trabajo en un momento dado. EventStore es un servicio de Service Fabric con estado que mantiene los eventos del clúster. El evento se expone a través de las API, de REST y de Service Fabric Explorer. EventStore consulta el clúster directamente para obtener datos de diagnóstico sobre cualquier entidad del clúster y debe usarse como ayuda para lo siguiente:

* Diagnosticar problemas en el desarrollo, prueba o allí donde use una canalización de supervisión
* Confirmar que el clúster procesa correctamente las acciones de administración que realiza
* Obtener una "instantánea" de cómo Service Fabric está interactuando con una entidad determinada

![EventStore](media/service-fabric-diagnostics-eventstore/eventstore.png)

Para ver una lista completa de los eventos disponibles en EventStore, consulte [Eventos de Service Fabric](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>A partir de Service Fabric versión 6.2., las API de EventStore se encuentran en versión preliminar solo para los clústeres de Windows que se ejecutan en Azure. Se está trabajando para portar esta funcionalidad a Linux, así como nuestros clústeres independientes.

Se puede consultar el servicio EventStore sobre los eventos que están disponibles para cada entidad y tipo de entidad del clúster. Esto significa que puede consultar los eventos en los siguientes niveles:
* Clúster: eventos específicos del clúster (por ejemplo, la actualización del clúster)
* Nodos: eventos de todos los niveles de nodo
* Nodo: eventos específicos de un nodo que se identifica por `nodeName`
* Aplicaciones: eventos de todos los niveles de aplicación
* Aplicación: eventos específicos de una aplicación que se identifica por `applicationId`
* Servicios: eventos de todos los servicios de los clústeres
* Servicio: eventos de un servicio específico que se identifica por `serviceId`
* Particiones: eventos de todas las particiones
* Partición: eventos de una partición específica que se identifica por `partitionId`
* Réplicas de partición: eventos de todas las réplicas o instancias dentro de una partición específica que se identifica por `partitionId`
* Réplica de partición: eventos de una réplica o instancia específica que se identifica por `replicaId` y `partitionId`

Para más información sobre la API, consulte [Referencia de EventStore API](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore).

El servicio EventStore también tiene la capacidad de correlacionar los eventos del clúster. El examen de los eventos que se escribieron al mismo tiempo desde distintas entidades y que pueden haberse afectado entre sí permite al servicio EventStore vincular estos eventos para identificar las causas de las actividades del clúster. Por ejemplo, si una de las aplicaciones pasa a un estado incorrecto sin realizar ningún cambio inducido, EventStore también examinará otros eventos expuestos por la plataforma y podría correlacionarlos con un evento `Error` o `Warning`. Esto ayuda a detectar los errores y analizar las causas raíz más rápidamente.

## <a name="enable-eventstore-on-your-cluster"></a>Habilitación de EventStore en el clúster

### <a name="local-cluster"></a>Clúster local

En el archivo [fabricSettings.json del clúster](service-fabric-cluster-fabric-settings.md), agregue EventStoreService como una característica complementaria y realice una actualización del clúster.

```json
    "addOnFeatures": [
        "EventStoreService"
    ],
```

### <a name="azure-cluster"></a>Clúster de Azure

En la plantilla del clúster Azure Resource Manager, puede activar el servicio EventStore mediante la realización de una [actualización de configuración de clúster](service-fabric-cluster-config-upgrade-azure.md) y agregue el código siguiente, se puede usar PlacementConstraints para colocar las réplicas de EventStore servicio en un tipo de nodo específico, por ejemplo, un tipo de nodo dedicado para los servicios del sistema. La sección `upgradeDescription` configura la actualización de la configuración para desencadenar un reinicio en los nodos. Puede quitar la sección en otra actualización.

```json
    "fabricSettings": [
          …
          …
          …,
         {
            "name": "EventStoreService",
            "parameters": [
              {
                "name": "TargetReplicaSetSize",
                "value": "3"
              },
              {
                "name": "MinReplicaSetSize",
                "value": "1"
              }
              {
                "name": "PlacementConstraints",
                "value": "(NodeType==<node_type_name_here>)"
              }
            ]
          }
        ],
        "upgradeDescription": {
          "forceRestart": true,
          "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807",
          "healthCheckWaitDuration": "00:01:00",
          "healthCheckStableDuration": "00:01:00",
          "healthCheckRetryTimeout": "00:5:00",
          "upgradeTimeout": "1:00:00",
          "upgradeDomainTimeout": "00:10:00",
          "healthPolicy": {
            "maxPercentUnhealthyNodes": 100,
            "maxPercentUnhealthyApplications": 100
          },
          "deltaHealthPolicy": {
            "maxPercentDeltaUnhealthyNodes": 0,
            "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0,
            "maxPercentDeltaUnhealthyApplications": 0
          }
        }
```


## <a name="next-steps"></a>Pasos siguientes
* Introducción a la API de EventStore: [artículo sobre el uso de las API de EventStore en los clústeres de Azure Service Fabric](service-fabric-diagnostics-eventstore-query.md)
* Más información sobre la lista de eventos que ofrece EventStore: [artículo sobre los eventos de Service Fabric](service-fabric-diagnostics-event-generation-operational.md)
* Información general de supervisión y diagnóstico de Service Fabric: [Supervisión y diagnóstico para Azure Service Fabric](service-fabric-diagnostics-overview.md)
* Lista completa de las llamadas de API: [referencia de la API de REST de EventStore](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore)
* Más información sobre cómo supervisar el clúster: [Supervisión del clúster y la plataforma](service-fabric-diagnostics-event-generation-infra.md).