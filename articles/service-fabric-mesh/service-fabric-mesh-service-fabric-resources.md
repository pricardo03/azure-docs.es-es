---
title: Introducción al modelo de recursos de Azure Service Fabric | Microsoft Docs
description: Obtenga información sobre el modelo de recursos de Service Fabric, un enfoque simplificado para definir aplicaciones de Service Fabric mesh.
services: service-fabric-mesh
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 6a96995a46cd2ad0a1afe4f4afe49d62409f2b68
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39075551"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Introducción al modelo de recursos de Service Fabric

El modelo de recursos de Service Fabric describe un enfoque simple para definir los recursos que componen una aplicación de Service Fabric mesh. Actualmente, se admiten los siguientes tipos de recursos en este modelo:

- APLICACIONES
- Services
- Volúmenes
- Redes

Cada recurso se describe mediante declaración en un archivo de recursos, que es un documento YAML o JSON simple que describe la aplicación mesh y que aprovisiona la plataforma Service Fabric.

## <a name="resource-overview"></a>Información general sobre recursos

### <a name="applications-and-services"></a>Aplicaciones y servicios

Un recurso de aplicación es la unidad de implementación, control de versiones y duración de una aplicación mesh. Se compone de uno o varios recursos de servicio que representan un microservicio. Cada recurso de servicio, a su vez, se compone de uno o más paquetes de código que describen todo lo necesario para ejecutar la imagen de contenedor asociada con el paquete de código, incluidos los elementos siguientes:

- Nombre, versión y registro del contenedor
- Recursos de CPU y memoria necesarios para cada contenedor
- Puntos de conexión de red
- Volúmenes de montaje en el contenedor, que hacen referencia a un recurso de volumen independiente.

Todos los paquetes de código definidos como parte de un recurso de servicio se implementan y activan juntos como grupo. El recurso de servicio también describe cuántas instancias de servicio se deben ejecutar y también hace referencia a otros recursos (por ejemplo, el recurso de red) de los que depende.

Si una aplicación mesh se compone de más de un servicio, no se garantiza que se ejecuten conjuntamente en el mismo nodo. Además, durante una actualización de la aplicación, un error al actualizar un único servicio provoca que se reviertan todos los servicios a su versión anterior.



```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

Como se ha mencionado antes, el ciclo de vida de cada instancia de aplicación se puede administrar de forma independiente. Por ejemplo, una instancia de aplicación se puede actualizar independientemente de las otras instancias de la aplicación. Normalmente, se mantiene un número reducido de servicios en una aplicación, ya que, cuantos más servicios se agregan a una aplicación, más difícil resulta administrar cada servicio de manera independiente.

### <a name="networks"></a>Redes

Un recurso de red es un recurso que se puede implementar individualmente y es independiente de un recurso de aplicación o servicio al que pueda hacer referencia como dependencia. Se usa para crear una red privada para las aplicaciones. Varios servicios de distintas aplicaciones pueden formar parte de la misma red.

> [!NOTE]
> La versión preliminar actual solo admite una asignación de uno a uno entre aplicaciones y redes.

### <a name="volumes"></a>Volúmenes

Los volúmenes son directorios que se montan dentro de las instancias de contenedor que puede usar para conservar el estado. El recurso de volumen es una manera declarativa de describir cómo se monta un directorio y su almacenamiento de respaldo.

## <a name="programming-models"></a>Modelos de programación
Un recurso de servicio solo requiere una imagen de contenedor para ejecutarse, a la que se hace referencia en los paquetes de código asociados con el recurso. Puede ejecutar cualquier código, escrito en cualquier lenguaje, con cualquier marco, dentro del contenedor sin necesidad de conocer ni usar API específicas de Service Fabric mesh. 

El código de aplicación sigue siendo portátil, incluso fuera de Service Fabric mesh, y las implementaciones de aplicaciones siguen siendo coherentes, independientemente del lenguaje o marco que se use para implementar los servicios. Independientemente de si la aplicación es ASP.NET Core, Go, o, simplemente, un conjunto de procesos y scripts, el modelo de implementación de recursos de Service Fabric mesh es el mismo. 

## <a name="deployment"></a>Implementación

Al implementar en Service Fabric mesh, los recursos se implementan como plantillas de Azure Resource Manager en Azure a través de HTTP o la CLI de Azure. 


## <a name="next-steps"></a>Pasos siguientes 
Para obtener más información sobre Service Fabric mesh, lea la introducción:
- [Introducción a Service Fabric mesh](service-fabric-mesh-overview.md)
