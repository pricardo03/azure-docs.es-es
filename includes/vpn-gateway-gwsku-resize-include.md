---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 081352a23e6a0d8f9e2daa77eca1f8ac85172ff6
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418235"
---
Para las SKU actuales (VpnGw1, VpnGw2 y VPNGW3), si quiere cambiar el tamaño de la SKU de puerta de enlace para actualizar a una más eficaz, puede usar el cmdlet de PowerShell `Resize-AzVirtualNetworkGateway`. También puede cambiar a una versión anterior del tamaño de la SKU de puerta de enlace con este cmdlet. Si usa la SKU de puerta de enlace Basic, [siga estas instrucciones](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) para cambiar el tamaño de la puerta de enlace.

En el siguiente ejemplo de PowerShell se muestra una SKU de puerta de enlace cuyo tamaño se ha cambiado a VpnGw2.

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

También se puede cambiar el tamaño de una puerta de enlace en Azure Portal desde la página **Configuración** para la puerta de enlace de red virtual. Para hacerlo, seleccione otra SKU en la lista desplegable.