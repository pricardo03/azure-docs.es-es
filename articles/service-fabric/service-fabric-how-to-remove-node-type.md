---
title: Eliminación de un tipo de nodo en Azure Service Fabric | Microsoft Docs
description: Aprenda a quitar un tipo de nodo de un clúster de Service Fabric que se ejecute en Azure.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/14/2019
ms.author: aljo
ms.openlocfilehash: 779051135a994574cb2bed7bfc4879270ec1d8fa
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443029"
---
# <a name="remove-a-service-fabric-node-type"></a>Quitar un tipo de nodo de Service Fabric
En este artículo, se explica cómo escalar un clúster de Azure Service Fabric quitando un tipo de nodo existente de un clúster. Un clúster de Service Fabric es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios. Un equipo o máquina virtual que forma parte de un clúster se denomina nodo. Los conjuntos de escalado de máquinas virtuales son un recurso de proceso de Azure que se puede usar para implementar y administrar una colección de máquinas virtuales de forma conjunta. Cada tipo de nodo que se define en un clúster de Azure está [configurado como un conjunto de escalado independiente](service-fabric-cluster-nodetypes.md). Cada tipo de nodo, a continuación, se puede administrar por separado. Después de crear un clúster de Service Fabric, puede escalarlo horizontalmente quitando un tipo de nodo (conjunto de escalado de máquinas virtuales) junto con todos sus nodos.  Puede escalar el clúster en cualquier momento, incluso con cargas de trabajo en ejecución en el clúster.  Según se escala el clúster, las aplicaciones se escalan automáticamente.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Use [Remove-AzServiceFabricNodeType](https://docs.microsoft.com/powershell/module/az.servicefabric/remove-azservicefabricnodetype) para quitar un tipo de nodo de Service Fabric.

Las tres operaciones que se producen cuando se lama a Remove-AzServiceFabricNodeType son las siguientes:
1.  Se elimina el conjunto de escalado de máquinas virtuales que subyace al tipo de nodo.
2.  El tipo de nodo se elimina del clúster.
3.  En el caso de cada uno de los nodos de ese tipo de nodo, se quita del sistema todo el estado de ese nodo. Si hay servicios en ese nodo, primero se moverán a otro nodo. Si el administrador de clústeres no puede encontrar un nodo para la réplica o servicio, la operación se retrasará o bloqueará.

> [!WARNING]
> No es aconsejable usar Remove-AzServiceFabricNodeType para quitar un tipo de nodo de un clúster de producción de forma frecuente. Se trata de un comando muy peligroso, ya que elimina el recurso del conjunto de escalado de máquinas virtuales que hay detrás del tipo de nodo. 

## <a name="durability-characteristics"></a>Características de durabilidad
Al usar Remove-AzServiceFabricNodeType, se da prioridad a la seguridad frente a la velocidad. El tipo de nodo debe tener un [nivel de durabilidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) Silver o Gold, ya que:
- Bronze no ofrece ningún tipo de garantía sobre la información de estado de guardado.
- Las durabilidades Silver y Gold identifican cualquier cambio tiene lugar en el conjunto de escalado.
- La durabilidad Gold también permite controlar las actualizaciones de Azure que tienen lugar bajo el conjunto de escalado.

Service Fabric “organiza” los cambios subyacentes y las actualizaciones para que no se pierdan los datos. Pero, al quitar un nodo con una durabilidad Bronze, puede que se pierda la información de estado. Si quita un tipo de nodo principal y la aplicación no tiene ningún estado, es aceptable el uso de Bronze. Al ejecutar cargas de trabajo con estado en producción, la configuración mínima tiene que ser Silver. De forma similar, para escenarios de producción, el tipo de nodo principal siempre tiene que ser Silver o Gold.

### <a name="more-about-bronze-durability"></a>Más información sobre la durabilidad Bronze

Al quitar un tipo de nodo Bronze, todos los nodos del tipo de nodo dejarán de estar activos de inmediato. Service Fabric no obtiene las actualizaciones del conjunto de escalado de los nodos Bronze y, por lo tanto, todas las máquinas virtuales dejan de estar activas de inmediato. Si tenía algún elemento con estado en esos nodos, los datos se perderán. Ahora, aunque no tengan estado, todos los nodos de Service Fabric participarán en el anillo, lo que podría hacer que se perdiera un grupo de nodos entero y desestabilizar al propio clúster.

## <a name="recommended-node-type-removal-process"></a>Proceso de eliminación de tipo de nodo recomendado

Para quitar el tipo de nodo, ejecute el cmdlet [Remove-AzServiceFabricNodeType](/powershell/module/az.servicefabric/remove-azservicefabricnodetype).  El cmdlet tarda un tiempo en completarse.  Una vez que todas las máquinas virtuales hayan desaparecido (representadas como "Fuera de servicio"), fabric:/System/InfrastructureService/[nombre de tipo de nodo] mostrará un estado de Error.

```powershell
$groupname = "mynodetype"
$nodetype = "nt2vm"
$clustername = "mytestcluster"

Remove-AzServiceFabricNodeType -Name $clustername  -NodeType $nodetype -ResourceGroupName $groupname

Connect-ServiceFabricCluster -ConnectionEndpoint mytestcluster.eastus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <thumbprint> `
          -FindType FindByThumbprint -FindValue <thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

A continuación, puede actualizar el recurso de clúster para quitar el tipo de nodo. Puede usar la implementación de plantilla de Resource Manager, o editar el recurso de clúster a través de [Azure Resource Manager](https://resources.azure.com). Esta acción iniciará una actualización de clúster que quitará al servicio fabric:/System/InfrastructureService/[nombre del tipo de nodo]que se encuentra en estado de error.

Se seguirán viendo los nodos con el estado "Fuera de servicio" en Service Fabric Explorer. Ejecute [Remove-ServiceFabricNodeState](/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) en cada uno de los nodos que quiera quitar.


```powershell
$nodes = Get-ServiceFabricNode | Where-Object {$_.NodeType -eq $nodetype} | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending

Foreach($node in $nodes)
{
    Remove-ServiceFabricNodeState -NodeName $node.NodeName -TimeoutSec 300 -Force 
}
```

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las [características de durabilidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) del clúster.
- Obtenga más información sobre los [tipos de nodo y los conjuntos de escalado de máquinas virtuales](service-fabric-cluster-nodetypes.md).
- Obtenga más información sobre el [escalado de clústeres de Service Fabric](service-fabric-cluster-scaling.md).
