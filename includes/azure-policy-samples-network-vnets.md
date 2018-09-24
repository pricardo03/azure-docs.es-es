---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: df9c291136fc6a48effb08cd59eeb66486eb641c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003971"
---
### <a name="virtual-networks"></a>Virtual Networks

|  |  |
|---------|---------|
| [SKU permitidas de Application Gateway](../articles/governance/policy/samples/allowed-app-gate-sku.md) | Requiere que Application Gateways use una SKU aprobada. Se especifica una matriz de las SKU aprobadas. |
| [Sin emparejamiento de red con la red de ER](../articles/governance/policy/samples/no-peering-er-net.md) | Prohíbe que se asocie un emparejamiento de red con una red de un grupo de recursos especificado. Se usa para evitar la conexión con la infraestructura de red administrada central. Se especifica el nombre del grupo de recursos para evitar la asociación. |
| [Sin tabla de rutas definida por el usuario](../articles/governance/policy/samples/no-user-def-route-table.md)  |Prohíbe que se implementen redes virtuales con una tabla de rutas definida por el usuario. |
| [NSG X en cada subred](../articles/governance/policy/samples/nsg-on-subnet.md) | Requiere que se use un grupo de seguridad de red específico con cada subred virtual. Se especifica el identificador del grupo de seguridad de red que se va a usar. |
| [Usar subred aprobada para las interfaces de red de máquinas virtuales](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | Requiere que las interfaces de red usen una subred aprobada. Se especifica el identificador de la subred aprobada. |
| [Usar red virtual aprobada para las interfaces de red de máquinas virtuales](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | Requiere que las interfaces de red usen una red virtual aprobada. Se especifica el identificador de la red virtual aprobada. |