---
title: ¿Qué es la dirección IP 168.63.129.16? | Microsoft Docs
description: Obtenga información sobre la dirección IP 168.63.129.16 y cómo funciona con los recursos.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: v-jesits
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/15/2019
ms.author: genli
ms.openlocfilehash: e061d503254ba7aa7735a97a060fc63f96b3fb61
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74196666"
---
# <a name="what-is-ip-address-1686312916"></a>¿Qué es la dirección IP 168.63.129.16?

La dirección IP 168.63.129.16 es una dirección IP pública virtual que se usa para facilitar un canal de comunicación a los recursos de la plataforma Azure. Los clientes pueden definir cualquier espacio de direcciones de su red virtual privada en Azure. Por lo tanto, los recursos de la plataforma Azure se deben presentar como una única dirección IP pública. Esta dirección IP pública virtual facilita lo siguiente:

- Permite al agente de VM comunicarse con la plataforma Azure para indicar que se encuentra en estado "Listo".
- Permite la comunicación con el servidor DNS virtual para proporcionar resolución de nombres filtrada a los recursos (como una máquina virtual) que no tienen un servidor DNS personalizado. Este filtrado garantiza que los clientes puedan resolver solo los nombres de host de sus recursos.
- Permite que los [sondeos de estado de Azure Load Balancer](../load-balancer/load-balancer-custom-probe-overview.md) determinen el estado de mantenimiento de las VM.
- Permite que la máquina virtual obtenga una dirección IP dinámica desde el servicio DHCP en Azure.
- Habilita los mensajes de latido del agente invitado para el rol PaaS.

## <a name="scope-of-ip-address-1686312916"></a>Ámbito de la dirección IP 168.63.129.16

La dirección IP pública 168.63.129.16 se utiliza en todas las regiones y en todas las nubes nacionales. Esta dirección IP pública especial es propiedad de Microsoft y no se cambiará. La regla del grupo de seguridad de red predeterminado la admite. Se recomienda que permita esta dirección IP en las directivas de firewall locales tanto en las direcciones de entrada como de salida. La comunicación entre esta dirección IP especial y los recursos es segura porque solo la plataforma interna de Azure puede originar un mensaje desde esta dirección IP. Si esta dirección se bloquea, puede producirse un comportamiento inesperado en una variedad de escenarios.
Deben abrirse como mínimo los puertos siguientes para permitir la comunicación con WireServer: 80, 443 y 32526.

Los [sondeos de estado de Azure Load Balancer](../load-balancer/load-balancer-custom-probe-overview.md) se originan desde esta dirección IP. Si bloquea esta dirección IP, se producirá un error en los sondeos.

En un escenario de red que no es virtual (clásica), el sondeo de estado se origina desde una dirección IP privada y 168.63.129.16 no se usa.

## <a name="next-steps"></a>Pasos siguientes

- [Grupos de seguridad](security-overview.md)
- [Creación, modificación o eliminación de un grupo de seguridad de red](manage-network-security-group.md)
