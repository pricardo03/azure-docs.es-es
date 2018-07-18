---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: b7bbf1fb8ffc1f57a53682d3baf288077b144572
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664694"
---
### <a name="virtual-networks"></a>Virtual Networks

|  |  |
|---------|---------|
| [SKU permitidas de Application Gateway](../articles/azure-policy/scripts/allowed-app-gate-sku.md) | Requiere que Application Gateways use una SKU aprobada. Se especifica una matriz de las SKU aprobadas. |
| [Sin emparejamiento de red con la red de ER](../articles/azure-policy/scripts/no-peering-er-net.md) | Prohíbe que se asocie un emparejamiento de red con una red de un grupo de recursos especificado. Se usa para evitar la conexión con la infraestructura de red administrada central. Se especifica el nombre del grupo de recursos para evitar la asociación. |
| [Sin tabla de rutas definida por el usuario](../articles/azure-policy/scripts/no-user-def-route-table.md)  |Prohíbe que se implementen redes virtuales con una tabla de rutas definida por el usuario. |
| [NSG X en cada subred](../articles/azure-policy/scripts/nsg-on-subnet.md) | Requiere que se use un grupo de seguridad de red específico con cada subred virtual. Se especifica el identificador del grupo de seguridad de red que se va a usar. |
| [Usar subred aprobada para las interfaces de red de máquinas virtuales](../articles/azure-policy/scripts/use-approved-subnet-vm-nics.md) | Requiere que las interfaces de red usen una subred aprobada. Se especifica el identificador de la subred aprobada. |
| [Usar red virtual aprobada para las interfaces de red de máquinas virtuales](../articles/azure-policy/scripts/use-approved-vnet-vm-nics.md) | Requiere que las interfaces de red usen una red virtual aprobada. Se especifica el identificador de la red virtual aprobada. |