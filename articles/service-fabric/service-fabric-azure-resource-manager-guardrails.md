---
title: Protecciones en la implementación de Azure Resource Manager de Service Fabric | Microsoft Docs
description: En este artículo se proporciona información general sobre los errores comunes que se producen al implementar un clúster de Service Fabric mediante Azure Resource Manager y cómo evitarlos.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: gamonroy
editor: ''
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/30/2019
ms.author: pepogors
ms.openlocfilehash: 3ea6f850685a695644cfc3073fc939a58901658c
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73828407"
---
# <a name="service-fabric-guardrails"></a>Protecciones de Service Fabric 
Al implementar un clúster de Service Fabric, se colocan protecciones, lo que producirá un error en una implementación de Azure Resource Manager en el caso de que haya una configuración de clúster no válida. En las secciones siguientes se proporciona información general sobre los problemas comunes de configuración de clústeres y los pasos necesarios para mitigarlos. 

## <a name="durability-mismatch"></a>Error de coincidencia de durabilidad
### <a name="overview"></a>Información general
El valor de durabilidad de un tipo de nodo de Service Fabric se define en dos secciones diferentes de una plantilla de Azure Resource Manager: la sección de la extensión del conjunto de escalado de máquinas virtuales del recurso del conjunto de escalado de máquinas virtuales y la sección del tipo de nodo del recurso de clúster de Service Fabric. El valor de durabilidad de estas secciones debe coincidir; de lo contrario, se producirá un error en la implementación del recurso.

En la siguiente sección se incluye un ejemplo de un error de coincidencia de durabilidad entre el valor de durabilidad de la extensión del conjunto de escalado de máquinas virtuales y el valor de durabilidad del tipo de nodo de Service Fabric:  

**Valor de durabilidad del conjunto de escalado de máquinas virtuales**
```json 
{
  "extensions": [
    {
      "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
      "properties": {
        "type": "ServiceFabricNode",
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
          "durabilityLevel": "Silver",
        }
      }
    }
  ]
}
```

**Valor de durabilidad del tipo de nodo de Service Fabric** 
```json
{
  "nodeTypes": [
    {
      "name": "[variables('vmNodeType0Name')]",
      "durabilityLevel": "Bronze",
      "isPrimary": true,
      "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    }
  ]
}
```

### <a name="error-messages"></a>mensajes de error
* El valor de durabilidad del conjunto de escalado de máquinas virtuales no coincide con el nivel de durabilidad del tipo de nodo actual de Service Fabric
* La durabilidad del conjunto de escalado de máquinas virtuales no coincide con el nivel de durabilidad del tipo de nodo de Service Fabric de destino
* La durabilidad del conjunto de escalado de máquinas virtuales coincide con el nivel de durabilidad de Service Fabric actual o el nivel de durabilidad del tipo de nodo de Service Fabric de destino 


### <a name="mitigation"></a>Mitigación
Para solucionar un error de coincidencia de durabilidad, que se indica mediante cualquiera de los mensajes de error anteriores:
1. Actualice el nivel de durabilidad en la extensión del conjunto de escalado de máquinas virtuales o en la sección del tipo de nodo de Service Fabric de la plantilla de Azure Resource Manager para asegurarse de que los valores coinciden.
2. Vuelva a implementar la plantilla de Azure Resource Manager con los valores actualizados.

## <a name="next-steps"></a>Pasos siguientes
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster independiente con Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Crear un clúster Linux](service-fabric-cluster-creation-via-portal.md)
* Solución de problemas de Service Fabric: [Guías de solución de problemas](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)
