---
title: Consulta de eventos de clúster con las API del servicio EventStore en clústeres de Azure Service Fabric | Microsoft Docs
description: Aprenda a usar las API del servicio EventStore de Azure Service Fabric para consultar los eventos de la plataforma
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
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: facbcd6def7451ca83bdf00fe9b7c7cac2c74945
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879954"
---
# <a name="query-eventstore-apis-for-cluster-events"></a>Consulta de las API del servicio EventStore para obtener eventos de clúster

En este artículo se describe cómo consultar las APIs de EventStore que están disponibles en Service Fabric versión 6.2 y posteriores; si desea obtener más información sobre el servicio EventStore, consulte [EventStore service overview](service-fabric-diagnostics-eventstore.md) (Introducción al servicio EventStore). Actualmente, el servicio EventStore solo puede acceder a datos para los últimos 7 días (esto se basa en la directiva de retención de datos de diagnóstico de su clúster).

>[!NOTE]
>Las API de EventStore se ofrecen con carácter general a partir de la versión 6.4 de Service Fabric para solamente los clústeres Windows que se ejecutan en Azure.

Puede tener acceso a las API de EventStore directamente a través de un punto de conexión REST o mediante programación. Según la consulta, hay varios parámetros que se necesitan para recopilar los datos correctos. Estos parámetros suelen incluir lo siguiente:
* `api-version`: la versión de las APIs de EventStore que usa
* `StartTimeUtc`: define el comienzo del período que le interese examinar
* `EndTimeUtc`: final del período de tiempo

Además de estos parámetros, hay algunos opcionales disponibles, por ejemplo:
* `timeout`: reemplace 60 segundo tiempo de espera predeterminado para realizar la operación de solicitud
* `eventstypesfilter`: Esto le ofrece la opción para filtrar tipos de evento específico
* `ExcludeAnalysisEvents`: no se devuelven los eventos de "Análisis". De forma predeterminada, las consultas de EventStore devolverán eventos de "análisis" siempre que sea posible. Los eventos de análisis son los eventos de canal operativo más enriquecidos que contienen contexto adicional o información más allá de un evento de Service Fabric normal, y proporcionan mayor profundidad.
* `SkipCorrelationLookup`: no busca posibles eventos correlacionados en el clúster. De forma predeterminada, EventStore intentará correlacionar los eventos en un clúster, y vincular los eventos cuando sea posible. 

Cada entidad en un clúster pueden ser consultas de eventos. También puede consultar eventos de todas las entidades del tipo. Por ejemplo, puede consultar eventos para un nodo específico o para todos los nodos del clúster. El conjunto de entidades para el que puede consultar los eventos actuales (con cómo se debe estructurar la consulta) es:
* Clúster: `/EventsStore/Cluster/Events`
* nodos: `/EventsStore/Nodes/Events`
* Nodo: `/EventsStore/Nodes/<NodeName>/$/Events`
* Aplicaciones: `/EventsStore/Applications/Events`
* Aplicación: `/EventsStore/Applications/<AppName>/$/Events`
* Servicios: `/EventsStore/Services/Events`
* Servicio: `/EventsStore/Services/<ServiceName>/$/Events`
* Particiones: `/EventsStore/Partitions/Events`
* Partición: `/EventsStore/Partitions/<PartitionID>/$/Events`
* Réplicas: `/EventsStore/Partitions/<PartitionID>/$/Replicas/Events`
* Réplica: `/EventsStore/Partitions/<PartitionID>/$/Replicas/<ReplicaID>/$/Events`

>[!NOTE]
>Cuando se hace referencia al nombre de una aplicación o servicio, la consulta no tiene que incluir el prefijo "fabric: /". Además, si los nombres de aplicación o servicio incluyen un "/", cámbielo a un "~" para mantener la consulta en funcionamiento. Por ejemplo, si la aplicación se muestra como "fabric:/App1/FrontendApp", las consultas específicas de la aplicación se estructurarían como `/EventsStore/Applications/App1~FrontendApp/$/Events`.
>Además, los informes de mantenimiento para los servicios hoy en día se muestran en la aplicación correspondiente, por lo que consultaría los eventos `DeployedServiceHealthReportCreated` para la entidad de la aplicación correcta. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>Consulta de EventStore a través de puntos de conexión de API de REST

Puede consultar EventStore directamente a través de un punto de conexión de REST, mediante solicitudes de `GET` a: `<your cluster address>/EventsStore/<entity>/Events/`.

Por ejemplo, para consultar todos los eventos Cluster entre `2018-04-03T18:00:00Z` y `2018-04-04T18:00:00Z`, la solicitud se vería como sigue:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.4&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Es posible que esta acción no devuelva ningún evento ni la lista de eventos que se devuelven en formato JSON:

```json
Response: 200
Body:
[
  {
    "Kind": "ClusterUpgradeStart",
    "CurrentClusterVersion": "0.0.0.0:",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeType": "Rolling",
    "RollingUpgradeMode": "UnmonitoredAuto",
    "FailureAction": "Manual",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:18:59.4313064Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(0 1 2)",
    "UpgradeDomainElapsedTimeInMs": "78.5288",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:19:59.5729953Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(3 4)",
    "UpgradeDomainElapsedTimeInMs": "0",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.6271949Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeComplete",
    "TargetClusterVersion": "6.2:1.0",
    "OverallUpgradeElapsedTimeInMs": "120196.5212",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.8134457Z",
    "HasCorrelatedEvents": false
  }
]
```

Aquí podemos verlo entre `2018-04-03T18:00:00Z` y `2018-04-04T18:00:00Z`, este clúster completó correctamente la primera actualización al realizarse por primera vez, de `"CurrentClusterVersion": "0.0.0.0:"` a `"TargetClusterVersion": "6.2:1.0"`, en `"OverallUpgradeElapsedTimeInMs": "120196.5212"`.

## <a name="query-the-eventstore-programmatically"></a>Consulta de EventStore mediante programación

También puede consultar EventStore mediante programación, a través de la [biblioteca de clientes de Service Fabric](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

Una vez que tenga que configurar el cliente de Service Fabric, puede consultar eventos mediante el acceso a la eventstore como sigue:
`sfhttpClient.EventStore.<request>`

Esta es una solicitud de ejemplo para todos los eventos de clúster entre `2018-04-03T18:00:00Z` y `2018-04-04T18:00:00Z`, a través de la función `GetClusterEventListAsync`.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

Este es otro ejemplo que consulta el estado del clúster y todos los eventos del nodo en septiembre de 2018 e imprime los resultados.

```csharp
  const int timeoutSecs = 60;
  var clusterUrl = new Uri(@"http://localhost:19080"); // This example is for a Local cluster
  var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl);

  var clusterHealth = sfhttpClient.Cluster.GetClusterHealthAsync().GetAwaiter().GetResult();
  Console.WriteLine("Cluster Health: {0}", clusterHealth.AggregatedHealthState.Value.ToString());

  
  Console.WriteLine("Querying for node events...");
  var nodesEvents = sfhttpClient.EventsStore.GetNodesEventListAsync(
      "2018-09-01T00:00:00Z",
      "2018-09-30T23:59:59Z",
      timeoutSecs,
      "NodeDown,NodeUp")
      .GetAwaiter()
      .GetResult()
      .ToList();
  Console.WriteLine("Result Count: {0}", nodesEvents.Count());

  foreach (var nodeEvent in nodesEvents)
  {
      Console.Write("Node event happened at {0}, Node name: {1} ", nodeEvent.TimeStamp, nodeEvent.NodeName);
      if (nodeEvent is NodeDownEvent)
      {
          var nodeDownEvent = nodeEvent as NodeDownEvent;
          Console.WriteLine("(Node is down, and it was last up at {0})", nodeDownEvent.LastNodeUpAt);
      }
      else if (nodeEvent is NodeUpEvent)
      {
          var nodeUpEvent = nodeEvent as NodeUpEvent;
          Console.WriteLine("(Node is up, and it was last down at {0})", nodeUpEvent.LastNodeDownAt);
      }
  }
```

## <a name="sample-scenarios-and-queries"></a>Ejemplo de escenarios y consultas

Presentamos algunos ejemplos sobre cómo se puede llamar a las API de REST del servicio EventStore para conocer el estado del clúster.

*Actualizaciones del clúster:*

Para ver la última vez el clúster correctamente o ha intentado para actualizarse a la semana pasada, puede consultar las API para las actualizaciones completadas recientemente en el clúster, mediante la consulta de los eventos de "ClusterUpgradeCompleted" en EventStore:
`https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeCompleted`

*Problemas de actualización de clúster:*

Del mismo modo, si hubo problemas con la actualización reciente de un clúster, podría consultar todos los eventos de la entidad del clúster. Podrá ver varios eventos, incluidos el inicio de las actualizaciones y cada UD para el que la actualización se realizó correctamente. También podrá ver los eventos para el punto en el que comenzó la reversión y los eventos de estado correspondientes. Esta es la consulta que utilizaría para esto:
`https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Cambios de estado del nodo:*

Para ver que el estado del nodo cambia durante los últimos días - cuando los nodos subieron o bajaron, o se activa o desactiva (mediante la plataforma, el servicio chaos o por el usuario): use la siguiente consulta:
`https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Eventos de aplicación:*

También puede realizar un seguimiento de las implementaciones y actualizaciones recientes de las aplicaciones. Use la siguiente consulta para ver todos los eventos de aplicación en el clúster:
`https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Estado histórico para una aplicación:*

Además de ver solo los eventos de ciclo de vida de las aplicaciones, también puede ver los datos históricos sobre el estado de una aplicación específica. Para hacerlo, especifique el nombre de la aplicación para la que desea recopilar los datos. Utilice esta consulta para obtener todos los eventos de estado de la aplicación: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.4&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ApplicationNewHealthReport`. Si desea incluir eventos de estado que puedan haber expirado (han superado el período de vida (TTL)), agregue `,ApplicationHealthReportExpired` al final de la consulta para filtrar según dos tipos de evento.

*Estado histórico para todos los servicios de "myApp":*

Actualmente, los eventos de informe de estado para los servicios se muestran como eventos `DeployedServicePackageNewHealthReport` debajo de la entidad de aplicación correspondiente. Para ver cómo han hecho los servicios para "App1", use la siguiente consulta:
`https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServicePackageNewHealthReport`

*Reconfiguración de particiones:*

Para ver todos los movimientos de partición que se produjeron en el clúster, consulte el evento `PartitionReconfigured`. Esto puede ayudarle a averiguar qué cargas de trabajo se ejecutaron en qué nodo a horas específicas cuando diagnostique problemas en el clúster. Esta es una consulta de ejemplo que lo hace:
`https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.4&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigured`

*Servicio CHAOS:*

Hay un evento para cuando se inicia o detiene el servicio Chaos, que se expone en el nivel de clúster. Para ver el uso reciente del servicio Chaos, utilice la siguiente consulta:
`https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

