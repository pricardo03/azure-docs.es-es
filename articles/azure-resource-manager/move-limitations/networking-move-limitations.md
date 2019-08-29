---
title: Traslado de recursos de red de Azure a una suscripción o un grupo de recursos nuevos | Microsoft Docs
description: Use Azure Resource Manager para trasladar redes virtuales y otros recursos de red a un nuevo grupo de recursos o a una nueva suscripción.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: c41ab1c3309243fa3a96d907f620ffeff10376a1
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69626121"
---
# <a name="move-guidance-for-networking-resources"></a>Directrices para el traslado de recursos de red

En este artículo, se explica cómo trasladar las redes virtuales y otros recursos de red en escenarios específicos.

## <a name="dependent-resources"></a>Recursos dependientes

Si mueve una red virtual, también deberá mover sus recursos dependientes. En el caso de las instancias de VPN Gateway, debe mover las direcciones IP, las puertas de enlace de red virtuales y todos los recursos de conexión asociados. Las puertas de enlace de red local pueden estar en otro grupo de recursos.

Para mover una máquina virtual con una tarjeta de interfaz de red, debe mover también todos los recursos dependientes. Mueva la red virtual de la tarjeta de interfaz de red, todas las demás tarjetas de interfaz de la red virtual y las puertas de enlace VPN.

## <a name="state-of-dependent-resources"></a>Estado de los recursos dependientes

Si el grupo de recursos de origen o de destino contiene una red virtual, los estados de todos los recursos dependientes de la red virtual se comprueban durante el traslado. Si alguno de esos recursos se encuentra en un estado de error, el traslado se bloqueará. Por ejemplo, si se produce un error en una máquina virtual que usa la red virtual, el traslado se bloqueará. El traslado se bloqueará incluso cuando la máquina virtual no sea uno de los recursos que se están trasladando y no esté en uno de los grupos de recursos que forma parte del traslado. Para soslayar este problema, traslade los recursos a un grupo de recursos que no tenga una red virtual.

## <a name="peered-virtual-network"></a>Red virtual emparejada

Para mover una red virtual emparejada, primero debe deshabilitar el emparejamiento de red virtual. Una vez deshabilitado, puede mover la red virtual. Después de moverla, vuelva a habilitar el emparejamiento de red virtual.

## <a name="subnet-links"></a>Vínculos de subredes

Si una red virtual contiene una subred con vínculos de navegación de los recursos, no se puede mover a otra suscripción. Por ejemplo, si un recurso de Azure Cache for Redis está implementado en una subred, esta contiene un vínculo de navegación de recursos.

## <a name="next-steps"></a>Pasos siguientes

Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../resource-group-move-resources.md).
