---
title: ¿Qué es la dirección IP 168.63.129.16? | Microsoft Docs
description: Obtenga información sobre la dirección IP 168.63.129.16 y cómo funciona con los recursos.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: v-jesits
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2019
ms.author: genli
ms.openlocfilehash: 7f0539e7c2f7e5ae8847b35b47d3708c6c5d6a09
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108000"
---
# <a name="what-is-ip-address-1686312916"></a>¿Qué es la dirección IP 168.63.129.16?

La dirección IP 168.63.129.16 es una dirección IP pública virtual que se usa para facilitar un canal de comunicación a los recursos de la plataforma Azure. Los clientes pueden definir cualquier espacio de direcciones de su red virtual privada en Azure. Por lo tanto, los recursos de la plataforma Azure se deben presentar como una única dirección IP pública. Esta dirección IP pública virtual facilita lo siguiente:

- Permite al agente de VM comunicarse con la plataforma Azure para indicar que se encuentra en estado "Listo".
- Permite la comunicación con el servidor DNS virtual para proporcionar resolución de nombres filtrada a los recursos (como una máquina virtual) que no tienen un servidor DNS personalizado. Este filtrado garantiza que los clientes puedan resolver solo los nombres de host de sus recursos.
- Permite que los sondeos de estado del equilibrador de carga determinen el estado de mantenimiento de las máquinas virtuales de un conjunto con equilibrio de carga.
- Habilita los mensajes de latido del agente invitado para el rol PaaS.

## <a name="scope-of-ip-address-1686312916"></a>Ámbito de la dirección IP 168.63.129.16

La dirección IP pública virtual 168.63.129.16 se utiliza en todas las regiones y en todas las nubes nacionales. Esta dirección IP pública especial no cambiará. La regla del grupo de seguridad de red predeterminado la admite. Le recomendamos que permita esta dirección IP en cualquiera de las directivas de firewall local. La comunicación entre esta dirección IP especial y los recursos es segura porque solo la plataforma interna de Azure puede originar un mensaje desde esta dirección IP. Si esta dirección se bloquea, puede producirse un comportamiento inesperado en una variedad de escenarios.

Además, puede esperar que el tráfico fluya desde la dirección IP pública virtual 168.63.129.16 al punto de conexión que esté configurado para un [sondeo de estado de Load Balancer](../load-balancer/load-balancer-custom-probe-overview.md). En un escenario de red que no es virtual, el sondeo de estado se origina desde una dirección IP privada. 

## <a name="next-steps"></a>Pasos siguientes

- [Grupos de seguridad](security-overview.md)
- [Creación, modificación o eliminación de un grupo de seguridad de red](manage-network-security-group.md)
