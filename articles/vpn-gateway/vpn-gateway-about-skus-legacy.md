---
title: SKU de puerta de enlace de red virtual de Azure heredada | Microsoft Docs
description: 'Cómo trabajar con las SKU antiguas de puerta de enlace de red virtual: Básica, Estándar y HighPerformance.'
services: vpn-gateway
documentationcenter: na
author: WenJason
manager: digimobile
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 03/20/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.openlocfilehash: 5a9e3f63a484069bf8cd39f8a545d7c37f05c63c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159260"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Trabajo con SKU de puerta de enlace de red virtual (SKU antiguas)

Este artículo contiene información sobre las SKU de puerta de enlace de red virtual heredadas (antiguas). Las SKU heredadas siguen funcionando en ambos modelos de implementación para las puertas de enlace de VPN ya creadas. Las puertas de enlace de VPN clásicas siguen usando las SKU heredadas para puertas de enlace existentes y para nuevas puertas de enlace. Al crear nuevas puertas de enlace de VPN de Resource Manager, use las nuevas SKU de puerta de enlace. Para más información sobre las nuevas SKU, vea [Acerca de VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>SKU de puerta de enlace

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Rendimiento agregado estimado por SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Configuraciones admitidas por el tipo de VPN y SKU

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Cambio del tamaño de una puerta de enlace

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Puede cambiar el tamaño de la puerta de enlace a una SKU de puerta de enlace dentro de la misma familia de la SKU. Por ejemplo, si tiene una SKU Estándar, puede cambiar a una SKU HighPerformance. Sin embargo, no se puede cambiar el tamaño de las puertas de enlace de VPN entre las familias de SKU antiguas y las nuevas. Por ejemplo, no se puede pasar de una SKU Estándar a una SKU VpnGw2, o de una SKU Básica a VpnGw1.

Para cambiar el tamaño de una puerta de enlace para el modelo de implementación clásica, use el siguiente comando:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Para cambiar el tamaño de una puerta de enlace para el modelo de implementación de Resource Manager mediante PowerShell, use el siguiente comando:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
También puede cambiar el tamaño de una puerta de enlace en Azure Portal.

## <a name="change"></a>Cambio a las nuevas SKU de puerta de enlace

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las nuevas SKU de puerta de enlace, consulte [SKU de puerta de enlace](vpn-gateway-about-vpngateways.md#gwsku).

Para más información sobre los valores de configuración, vea [Acerca de la configuración de VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).

<!--Update_Description: wording update-->