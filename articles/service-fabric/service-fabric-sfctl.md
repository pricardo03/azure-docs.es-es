---
title: 'CLI de Azure Service Fabric: sfctl | Microsoft Docs'
description: Se describen los comandos de sfctl de la CLI de Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: c195d0c4250022102e735cf584370278e354bc41
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60545051"
---
# <a name="sfctl"></a>sfctl
Comandos para administrar clústeres y entidades de Service Fabric. Esta versión es compatible con el entorno de ejecución de Service Fabric 6.4.

Los comandos siguientes siguen el patrón nombre-verbo. Consulte los subgrupos para más información.

## <a name="subgroups"></a>Subgrupos
|Subgrupo|DESCRIPCIÓN|
| --- | --- |
| [application](service-fabric-sfctl-application.md) | Cree, elimine y administre aplicaciones y tipos de aplicaciones. |
| [chaos](service-fabric-sfctl-chaos.md) | Inicie, detenga e informe sobre el servicio de prueba de Chaos. |
| [cluster](service-fabric-sfctl-cluster.md) | Seleccione, administre y opere clústeres de Service Fabric. |
| [compose](service-fabric-sfctl-compose.md) | Cree, elimine y administre aplicaciones de Docker Compose. |
| [container](service-fabric-sfctl-container.md) | Ejecute los comandos relacionados con el contenedor en un nodo de clúster. |
| [is](service-fabric-sfctl-is.md) | Consulte y envíe comandos al servicio de infraestructura. |
| [mesh](service-fabric-sfctl-mesh.md) | Elimina y administra aplicaciones de Service Fabric Mesh. |
| [node](service-fabric-sfctl-node.md) | Administre los nodos que forman un clúster. |
| [partition](service-fabric-sfctl-partition.md) | Consulte y administre las particiones para cualquier servicio. |
| [property](service-fabric-sfctl-property.md) | Almacene y consulte las propiedades con nombres de Service Fabric. |
| [replica](service-fabric-sfctl-replica.md) | Administre las réplicas que pertenecen a las particiones del servicio. |
| [rpm](service-fabric-sfctl-rpm.md) | Consulte y envíe comandos al servicio del administrador de reparaciones. |
| [sa-cluster](service-fabric-sfctl-sa-cluster.md) | Administre clústeres de Service Fabric independientes. |
| [service](service-fabric-sfctl-service.md) | Cree, elimine y administre servicios, tipos de servicio y paquetes de servicio. |
| [settings](service-fabric-sfctl-settings.md) | Configura los valores locales para esta instancia de sfctl. |
| [store](service-fabric-sfctl-store.md) | Realice operaciones básicas a nivel de archivo en el almacén de imágenes del clúster. |

## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).