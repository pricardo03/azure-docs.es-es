---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/15/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f6fd4039614dbd7c1a2b2c6ba8403502a6420fe3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57908559"
---
Para las SKU actuales (VpnGw1, VpnGw2 y VPNGW3), si quiere cambiar el tamaño de la SKU de puerta de enlace para actualizar a una más eficaz, puede usar el cmdlet de PowerShell `Resize-AzVirtualNetworkGateway`. También puede cambiar a una versión anterior del tamaño de la SKU de puerta de enlace con este cmdlet. Si usa la SKU de puerta de enlace Basic, [siga estas instrucciones](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) para cambiar el tamaño de la puerta de enlace.

En el siguiente ejemplo de PowerShell se muestra una SKU de puerta de enlace cuyo tamaño se ha cambiado a VpnGw2.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

También se puede cambiar el tamaño de una puerta de enlace en Azure Portal desde la página **Configuración** para la puerta de enlace de red virtual. Para hacerlo, seleccione otra SKU en la lista desplegable.