---
title: Creación de una instancia de Load Balancer con front-end de zona mediante Azure PowerShell
titleSuffix: Azure Load Balancer
description: Aprenda a crear una instancia de Standard Load Balancer con front-end de zona mediante Azure PowerShell.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2019
ms.author: allensu
ms.openlocfilehash: 8fa73479c5bbe73f0cb6c828a0552bb8dca80d3f
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75894499"
---
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-powershell"></a>Creación de un equilibrador Standard Load Balancer con el front-end de zona mediante Azure PowerShell

Este artículo le ayudará a crear una instancia de [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) pública con un front-end de zona mediante una dirección IP pública estándar. Para saber cómo funcionan las zonas de disponibilidad con Load Balancer Estándar, consulte [Load Balancer Estándar y zonas de disponibilidad](load-balancer-standard-availability-zones.md). 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!NOTE]
> El soporte técnico para las zonas de disponibilidad está disponible para recursos y regiones de Azure, y familias de tamaños de máquina virtual seleccionados. Para más información sobre cómo empezar a trabajar y qué recursos, regiones y familias de tamaños de máquina virtual de Azure puede probar con las zonas de disponibilidad, consulte la [introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview). Para soporte técnico, eche un vistazo a [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) o [Creación de una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción de Azure con el comando `Connect-AzAccount` y siga las instrucciones de la pantalla.

```azurepowershell-interactive
Connect-AzAccount
```

## <a name="create-resource-group"></a>Creación de un grupo de recursos

Cree un grupo de recursos con el siguiente comando:

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupZLB -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Crear una dirección IP pública estándar 
Cree una dirección IP pública estándar con el siguiente comando:

```azurepowershell-interactive
$publicIp = New-AzPublicIpAddress -ResourceGroupName myResourceGroupZLB -Name 'myPublicIPZonal' `
  -Location westeurope -AllocationMethod Static -Sku Standard -zone 1
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Creación de una configuración IP front-end para el sitio web

Cree una configuración IP de front-end con el siguiente comando:

```azurepowershell-interactive
$feip = New-AzLoadBalancerFrontendIpConfig -Name 'myFrontEnd' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>Creación del grupo de direcciones de back-end

Cree un grupo de direcciones de back-end con el siguiente comando:

```azurepowershell-interactive
$bepool = New-AzLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>Creación de un sondeo de equilibrio de carga en el puerto 80

Cree un sondeo de estado en el puerto 80 para el equilibrador de carga con el siguiente comando:

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga
 Cree una regla del equilibrador de carga con el comando siguiente:

```azurepowershell-interactive
   $rule = New-AzLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>Creación de un equilibrador de carga
Cree una instancia de Standard Load Balancer con el siguiente comando:

```azurepowershell-interactive
$lb = New-AzLoadBalancer -ResourceGroupName myResourceGroupZLB -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Load Balancer Estándar y zonas de disponibilidad](load-balancer-standard-availability-zones.md).