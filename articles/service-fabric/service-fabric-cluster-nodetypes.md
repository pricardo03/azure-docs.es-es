---
title: Tipos de nodos y conjuntos de escalado de máquinas virtuales de Azure Service Fabric | Microsoft Docs
description: Aprenda a relacionar los tipos de nodos de Azure Service Fabric con los conjuntos de escalado de máquinas virtuales y a conectarse de forma remota a una instancia de conjunto de escalado o un nodo de clúster.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: a5f8735df2b230de2b0ddcdcccff09430bada9e3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64684688"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Tipos de nodos y conjuntos de escalado de máquinas virtuales de Azure Service Fabric
Los [conjuntos de escalado de máquinas virtuales](/azure/virtual-machine-scale-sets) son un recurso de Azure Compute. Puede usarlos para implementar y administrar una colección de máquinas virtuales como conjunto. Cada tipo de nodo que defina en un clúster de Azure Service Fabric configura un escalado independiente.  El tiempo de ejecución de Service Fabric instalado en cada máquina virtual en el conjunto de escalado, la extensión de máquina Virtual de Microsoft.Azure.ServiceFabric. Cada tipo de nodo se puede escalar o reducir verticalmente de forma independiente; puede cambiar la SKU del sistema operativo que se ejecuta en cada nodo de clúster, tener diferentes conjuntos de puertos abiertos y usar distintas métricas de capacidad.

En la siguiente ilustración, se muestra un clúster que tiene dos tipos de nodos llamados front-end y back-end. A su vez, cada tipo de nodo tiene cinco nodos.

![Un clúster que tiene dos tipos de nodos][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Asignación de instancias de conjuntos de escalado de máquinas virtuales a los nodos
Como se muestra en la ilustración anterior, las instancias del conjunto de escalado empiezan por la instancia 0 y, a continuación, se van incrementando de uno en uno. La numeración se refleja en los nombres de los nodos. Por ejemplo, el nodo BackEnd_0 es la instancia 0 del conjunto de escalado de BackEnd. En concreto, este conjunto de escalado tiene cinco instancias, denominadas BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 y BackEnd_4.

Al escalar verticalmente un conjunto de escalado, se crea una nueva instancia. El nuevo nombre de la instancia de conjunto de escalado será normalmente el nombre del conjunto de escalado seguido del número de instancia. En nuestro ejemplo, es BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Asignación de los equilibradores de carga del conjunto de escalado a los tipos de nodos y conjuntos de escalado
Si ha implementado el clúster en Azure Portal o ha usado la plantilla de Azure Resource Manager de ejemplo, obtendrá una lista de todos los recursos en un grupo de recursos. Puede ver los equilibradores de carga de cada conjunto de escalado o tipo de nodo. El nombre del equilibrador de carga utiliza el formato siguiente: **LB -&lt;nombre de tipo de nodo&gt;**. Por ejemplo: LB-sfcluster4doc-0, tal y como se muestra en la ilustración siguiente:

![Recursos][Resources]

## <a name="service-fabric-virtual-machine-extension"></a>Extensión de máquina Virtual de Service Fabric
Extensión de máquina Virtual de Service Fabric se utiliza para arrancar a Service Fabric en Azure Virtual Machines y configurar la seguridad de nodo.

Este es un fragmento de código de extensión de máquina Virtual de Service Fabric:

```json
"extensions": [
  {
    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
    "properties": {
      "type": "ServiceFabricLinuxNode",
      "autoUpgradeMinorVersion": true,
      "protectedSettings": {
        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
       },
       "publisher": "Microsoft.Azure.ServiceFabric",
       "settings": {
         "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
         "nodeTypeRef": "[variables('vmNodeType0Name')]",
         "durabilityLevel": "Silver",
         "enableParallelJobs": true,
         "nicPrefixOverride": "[variables('subnet0Prefix')]",
         "certificate": {
           "commonNames": [
             "[parameters('certificateCommonName')]"
           ],
           "x509StoreName": "[parameters('certificateStoreValue')]"
         }
       },
       "typeHandlerVersion": "1.1"
     }
   },
```

Estas son las descripciones de propiedad:

| **Nombre** | **Valores permitidos** | ** --- ** | **Orientación o breve descripción** |
| --- | --- | --- | --- |
| Nombre | string | --- | nombre único para la extensión |
| Tipo | "ServiceFabricLinuxNode" o "ServiceFabricWindowsNode | --- | Identifica es de arranque del sistema operativo Service Fabric para |
| autoUpgradeMinorVersion | true o false | --- | Habilitar la actualización automática de las versiones secundarias de SF en tiempo de ejecución |
| publisher | Microsoft.Azure.ServiceFabric | --- | nombre del publicador de la extensión de Service Fabric |
| clusterEndpont | string | --- | URI:Port al punto de conexión de administración |
| nodeTypeRef | string | --- | nombre del tipo de nodo |
| durabilityLevel | Bronce, plata, oro, platinum | --- | tiempo permitido para pausar la infraestructura inmutable de Azure |
| enableParallelJobs | true o false | --- | Habilitar proceso ParallelJobs como quitar la máquina virtual y reinicie la máquina virtual del mismo conjunto de escalado en paralelo |
| nicPrefixOverride | string | --- | Prefijo de subred, como "10.0.0.0/24" |
| commonNames | string[] | --- | Nombres comunes de los certificados de clúster instalados |
| x509StoreName | string | --- | Nombre de Store donde se encuentra el certificado de clúster instalados |
| typeHandlerVersion | 1.1 | --- | Versión de extensión. se recomienda actualizar a la versión 1.1 1.0 versión clásica de extensión |

## <a name="next-steps"></a>Pasos siguientes
* Consulte [Descripción general de la característica "Deploy anywhere" y comparación con los clústeres administrados de Azure](service-fabric-deploy-anywhere.md).
* Obtenga más información sobre la [seguridad del clúster](service-fabric-cluster-security.md).
* [Conexión remota](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) a la instancia específica del conjunto de escalado
* [Actualización de los valores del intervalo de puertos RDP](./scripts/service-fabric-powershell-change-rdp-port-range.md) en máquinas virtuales del clúster después de la implementación
* [Cambio de la contraseña y el nombre de usuario administrador](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) para máquinas virtuales del clúster

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
