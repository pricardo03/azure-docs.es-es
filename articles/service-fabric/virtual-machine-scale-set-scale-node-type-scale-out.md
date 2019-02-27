---
title: Incorporación de un tipo de nodo en un clúster de Azure Service Fabric | Microsoft Docs
description: Aprenda a escalar horizontalmente un clúster de Service Fabric incorporando un conjunto de escalado de máquinas virtuales.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/13/2019
ms.author: aljo
ms.openlocfilehash: 01d4af8349d3f5a0f58c4c3fa56b489d739c7b42
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301713"
---
# <a name="scale-a-service-fabric-cluster-out-by-adding-a-virtual-machine-scale-set"></a>Escalado horizontal de un clúster de Service Fabric con un conjunto de escalado de máquinas virtuales
En este artículo, se explica cómo escalar un clúster de Azure Service Fabric agregando un nuevo tipo de nodo a un clúster existente. Un clúster de Service Fabric es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios. Un equipo o máquina virtual que forma parte de un clúster se denomina nodo. Los conjuntos de escalado de máquinas virtuales son un recurso de proceso de Azure que se puede usar para implementar y administrar una colección de máquinas virtuales de forma conjunta. Cada tipo de nodo que se define en un clúster de Azure está [configurado como un conjunto de escalado independiente](service-fabric-cluster-nodetypes.md). Cada tipo de nodo, a continuación, se puede administrar por separado. Después de crear un clúster de Service Fabric, puede escalarlo horizontalmente agregando un nuevo tipo de nodo (conjunto de escalado de máquinas virtuales) a un clúster existente.  Puede escalar el clúster en cualquier momento, incluso con cargas de trabajo en ejecución en el clúster.  Según se escala el clúster, las aplicaciones se escalan automáticamente.

## <a name="add-an-additional-scale-set-to-an-existing-cluster"></a>Incorporación de otro conjunto de escalado en un clúster existente
Agregar un nuevo tipo de nodo (que cuente con el respaldo de un conjunto de escalado de máquinas virtuales) a un clúster existente es igual que [actualizar el tipo de nodo principal](service-fabric-scale-up-node-type.md), salvo que no se utilizará la misma propiedad NodeTypeRef, no se deshabilitará ningún conjunto de escalado de máquinas virtuales que se utilice activamente ni se perderá la disponibilidad del clúster si no se actualiza el tipo de nodo principal. 

La propiedad NodeTypeRef se declara en las propiedades de la extensión de Service Fabric del conjunto de escalado de máquinas virtuales:
```json
<snip>
"publisher": "Microsoft.Azure.ServiceFabric",
     "settings": {
     "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
     "nodeTypeRef": "[parameters('vmNodeType2Name')]",
     "dataPath": "D:\\\\SvcFab",
     "durabilityLevel": "Silver",
<snip>
```

Además deberá agregar este nuevo tipo de nodo al recurso del clúster de Service Fabric:

```json
<snip>
"nodeTypes": [
      {
      "name": "[parameters('vmNodeType2Name')]",
      "applicationPorts": {
                "endPort": "[parameters('nt2applicationEndPort')]",
                "startPort": "[parameters('nt2applicationStartPort')]"
      },
      "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
      "durabilityLevel": "Silver",
       "ephemeralPorts": {
                "endPort": "[parameters('nt2ephemeralEndPort')]",
                "startPort": "[parameters('nt2ephemeralStartPort')]"
      },
      "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
      "isPrimary": false,
      "vmInstanceCount": "[parameters('nt2InstanceCount')]"
},
<snip>
```

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información acerca de cómo [escalar verticalmente el tipo de nodo principal](service-fabric-scale-up-node-type.md)
* Obtenga información sobre [escalabilidad de aplicaciones](service-fabric-concepts-scalability.md).
* [Escalado horizontal de un clúster de Azure](service-fabric-tutorial-scale-cluster.md).
* [Escalado de un clúster de Azure mediante programación](service-fabric-cluster-programmatic-scaling.md) con el SDK de proceso de Azure.
* [Escalado o reducción horizontal de un clúster independiente](service-fabric-cluster-windows-server-add-remove-nodes.md)

