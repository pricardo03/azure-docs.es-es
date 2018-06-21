---
title: 'CLI de Azure Service Fabric: sfctl | Microsoft Docs'
description: Se describen los comandos de sfctl de la CLI de Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: 456257e54da83ac629039b714cc74f9dafda2174
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763279"
---
# <a name="sfctl"></a>sfctl
Comandos para administrar clústeres y entidades de Service Fabric. Esta versión es compatible con el entorno de ejecución de Service Fabric 6.2.

Los comandos siguientes siguen el patrón nombre-verbo. Para obtener más información, vea los subgrupos.

## <a name="subgroups"></a>Subgrupos
|Subgrupo|DESCRIPCIÓN|
| --- | --- |
| [application](service-fabric-sfctl-application.md) | Cree, elimine y administre aplicaciones y tipos de aplicaciones. |
| [chaos](service-fabric-sfctl-chaos.md) | Inicie, detenga e informe sobre el servicio de prueba de Chaos. |
| [cluster](service-fabric-sfctl-cluster.md) | Seleccione, administre y opere clústeres de Service Fabric. |
| [compose](service-fabric-sfctl-compose.md) | Cree, elimine y administre aplicaciones de Docker Compose. |
| [container](service-fabric-sfctl-container.md) | Ejecute los comandos relacionados con el contenedor en un nodo de clúster. |
| [is](service-fabric-sfctl-is.md) | Consulte y envíe comandos al servicio de infraestructura. |
| [node](service-fabric-sfctl-node.md) | Administre los nodos que forman un clúster. |
| [partition](service-fabric-sfctl-partition.md) | Consulte y administre las particiones para cualquier servicio. |
| [property](service-fabric-sfctl-property.md) | Almacene y consulte las propiedades con nombres de Service Fabric. |
| [replica](service-fabric-sfctl-replica.md) | Administre las réplicas que pertenecen a las particiones del servicio. |
| [rpm](service-fabric-sfctl-rpm.md) | Consulte y envíe comandos al servicio del administrador de reparaciones. |
| [sa-cluster](service-fabric-sfctl-sa-cluster.md) | Administre clústeres de Service Fabric independientes. |
| [service](service-fabric-sfctl-service.md) | Cree, elimine y administre servicios, tipos de servicio y paquetes de servicio. |
| [store](service-fabric-sfctl-store.md) | Realice operaciones básicas a nivel de archivo en el almacén de imágenes del clúster. |

## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).