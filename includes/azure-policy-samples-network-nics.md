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
ms.openlocfilehash: f189843e865863d9b4088363e691ebc42ad9f2a9
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186461"
---
### <a name="network-interfaces"></a>Interfaces de red

|  |  |
|---------|---------|
| [NSG X en cada NIC](../articles/governance/policy/samples/nsg-on-nic.md) | Requiere que se use un grupo de seguridad de red específico con cada interfaz de red virtual. Se especifica el identificador del grupo de seguridad de red que se va a usar. |
| [Usar subred aprobada para las interfaces de red de máquinas virtuales](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | Requiere que las interfaces de red usen una subred aprobada. Se especifica el identificador de la subred aprobada. |
| [Usar red virtual aprobada para las interfaces de red de máquinas virtuales](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | Requiere que las interfaces de red usen una red virtual aprobada. Se especifica el identificador de la red virtual aprobada. |