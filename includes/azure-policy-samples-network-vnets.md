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
ms.openlocfilehash: f93e22012a4855257f5372c1fc1dbc05ad29a6cd
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155623"
---
### <a name="virtual-networks"></a>Virtual Networks

|  |  |
|---------|---------|
| [SKU permitidas de Application Gateway](../articles/governance/policy/samples/allowed-app-gateway-sku.md) | Requiere que Application Gateways use una SKU aprobada. Se especifica una matriz de las SKU aprobadas. |
| [SKU permitidas de puerta de enlace de red virtual](../articles/governance/policy/samples/allowed-vnet-gateway-sku.md) | Requiere que las puertas de enlace de red virtual usen un tipo de SKU aprobado. Se especifica una matriz de las SKU aprobadas. |
| [SKU permitidas de Load Balancer](../articles/governance/policy/samples/allowed-load-balancer-skus.md) | Requiere que los equilibradores de carga de la red virtual usen un tipo de SKU aprobado. Se especifica una matriz de las SKU aprobadas. |
| [No hay ningún emparejamiento con la red de Express Route](../articles/governance/policy/samples/no-peering-express-route-network.md) | Prohíbe que se asocie un emparejamiento de red con una red de un grupo de recursos especificado. Se usa para evitar la conexión con la infraestructura de red administrada central. Se especifica el nombre del grupo de recursos para evitar la asociación. |
| [Sin tabla de rutas definida por el usuario](../articles/governance/policy/samples/no-user-defined-route-table.md)  | Prohíbe que se implementen redes virtuales con una tabla de rutas definida por el usuario. |
| [NSG X en cada subred](../articles/governance/policy/samples/nsg-on-subnet.md) | Requiere que se use un grupo de seguridad de red específico con cada subred virtual. Se especifica el identificador del grupo de seguridad de red que se va a usar. |
| [Usar subred aprobada para las interfaces de red de máquinas virtuales](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | Requiere que las interfaces de red usen una subred aprobada. Se especifica el identificador de la subred aprobada. |
| [Usar red virtual aprobada para las interfaces de red de máquinas virtuales](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | Requiere que las interfaces de red usen una red virtual aprobada. Se especifica el identificador de la red virtual aprobada. |