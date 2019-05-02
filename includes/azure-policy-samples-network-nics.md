---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 05/17/2018
ms.date: 11/12/2018
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: f189843e865863d9b4088363e691ebc42ad9f2a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60866032"
---
### <a name="network-interfaces"></a>Interfaces de red

|  |  |
|---------|---------|
| [NSG X en cada NIC](../articles/governance/policy/samples/nsg-on-nic.md) | Requiere que se use un grupo de seguridad de red específico con cada interfaz de red virtual. Se especifica el identificador del grupo de seguridad de red que se va a usar. |
| [Usar subred aprobada para las interfaces de red de máquinas virtuales](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | Requiere que las interfaces de red usen una subred aprobada. Se especifica el identificador de la subred aprobada. |
| [Usar red virtual aprobada para las interfaces de red de máquinas virtuales](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | Requiere que las interfaces de red usen una red virtual aprobada. Se especifica el identificador de la red virtual aprobada. |