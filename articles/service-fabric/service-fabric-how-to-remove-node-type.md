---
title: Configurar de forma remota un tipo de nodo en Azure Service Fabric | Microsoft Docs
description: Obtenga información sobre cómo quitar un tipo de nodo en Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: v-steg
manager: JeanPaul.Connick
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/26/2018
ms.author: v-steg
ms.openlocfilehash: 3704a356763b16a30285baee1aabffdd3aa3f8aa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53981891"
---
# <a name="remove-a-service-fabric-node-type"></a>Quitar un tipo de nodo de Service Fabric

Use [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) para quitar un tipo de nodo de Service Fabric.

Las dos operaciones que se producen cuando se realiza una llamada a Remove-AzureRmServiceFabricNodeType son las siguientes:
1.  Se elimina el conjunto de escalado de máquinas virtuales (VMSS) detrás del tipo de nodo.
2.  Para todos los nodos dentro de ese tipo de nodo, se quitan del sistema todos los estados de esos nodos. Si hay servicios en ese nodo, primero se moverán a otro nodo. Si el administrador de clústeres no puede encontrar un nodo para la réplica o servicio, la operación se retrasará o bloqueará.

> [!NOTE]
> No le recomendamos usar Remove-AzureRmServiceFabricNodeType para quitar un tipo de nodo de un clúster de producción de forma frecuente. En este caso, se trata de un comando muy peligroso, ya que prácticamente elimina el recurso del conjunto de escalado de máquinas virtuales detrás del tipo de nodo. Al realizar una llamada a Remove-AzureRmServiceFabricNodeType, el sistema no tiene forma de conocer si es importante que la eliminación se realice de forma segura. 

## <a name="durability-characteristics"></a>Características de durabilidad
Al usar Remove-AzureRmServiceFabricNodeType, se da prioridad a la seguridad frente a la velocidad. Se recomiendan las [características de durabilidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) Silver o Gold por estos motivos:
- Bronze no ofrece ningún tipo de garantía sobre la información de estado de guardado.
- Las durabilidades Silver y Gold identifican los cambios que se realicen en VMSS.
- Gold también le permite controlar las actualizaciones de Azure en VMSS.

Service Fabric “organiza” los cambios subyacentes y las actualizaciones para que no se pierdan los datos. Pero, al quitar un nodo con una durabilidad Bronze, puede que se pierda la información de estado. Si quita un tipo de nodo principal y la aplicación no tiene ningún estado, es aceptable el uso de Bronze. Al ejecutar cargas de trabajo con estado en producción, la configuración mínima tiene que ser Silver. De forma similar, para escenarios de producción, el tipo de nodo principal siempre tiene que ser Silver o Gold.

### <a name="more-about-bronze-durability"></a>Más información sobre la durabilidad Bronze

Al quitar un tipo de nodo Bronze, todos los nodos del tipo de nodo dejarán de estar activos de inmediato. Service Fabric no obtiene las actualizaciones de VMSS de los nodos Bronze y, por lo tanto, todas las VM dejan de estar activas de inmediato. Si tenía algún elemento con estado en esos nodos, los datos se perderán. Ahora, incluso si estaba sin estado, todos los nodos de Service Fabric participan en el anillo y, por lo tanto, puede que se pierda un grupo de nodos entero, lo que podría afectar al clúster en sí.

Al contrario que la acción de quitar un único nodo (porque, en teoría, se pueden quitar nodos de uno en uno), espere hasta que las réplicas y servicios terminen de migrarse, espere hasta que se estabilice el sistema antes de quitar otro nodo, etc.  Pero, si quita varios nodos a la vez, puede que el clúster deje de estar activo (ya que Service Fabric no obtiene las actualizaciones de VMSS con durabilidad Bronze).

## <a name="recommended-node-type-removal-process"></a>Proceso de eliminación de tipo de nodo recomendado

Para quitar el tipo de nodo de la forma más rápida y segura:
1.  Si usa la durabilidad Bronze o no quiere que el sistema migre aplicaciones que contengan información de estado que se perdería como parte de la eliminación del tipo de nodo, primero vacíe los datos con estado de los nodos que se verán afectados por la eliminación del tipo de nodo.
2.  Ejecute [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) en cada uno de los nodos que quiera quitar.
3.  Ejecute [Remove-AzureRmVmss](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#remove-vms-from-a-scale-set) para las VM que se verán afectadas por la eliminación del tipo de nodo.
4. Ejecute [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) para quitar el tipo de nodo.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las [características de durabilidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) del clúster.
- Obtenga más información sobre los [tipos de nodo y los conjuntos de escalado de máquinas virtuales](service-fabric-cluster-nodetypes.md).
- Obtenga más información sobre el [escalado de clústeres de Service Fabric](service-fabric-cluster-scaling.md).