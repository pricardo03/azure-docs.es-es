---
title: Creación de un host bastión con Azure PowerShell | Microsoft Docs
description: En este artículo aprenderá a crear un host de Azure Bastion.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 5fea9c9a99250c8abe26f953b1c6632b561a6013
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511889"
---
# <a name="create-an-azure-bastion-host-using-azure-powershell"></a>Creación de un host Azure Bastion con Azure PowerShell

En este artículo aprenderá a crear un host de Azure Bastion. Una vez que haya aprovisionado el servicio Azure Bastion en su red virtual, la experiencia fluida mediante RDP/SSH estará disponible para todas las máquinas virtuales de la misma red virtual. Esta implementación se realiza por red virtual, no por suscripción o cuenta, ni por máquina virtual.

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que tiene una suscripción a Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) o registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="createhost"></a>Creación de un host de Bastion

Esta sección le ayuda a crear un nuevo recurso Azure Bastion con Azure PowerShell.

1. Cree una red virtual y una subred Azure Bastion. Debe crear la subred Azure Bastion con el valor de nombre **AzureBastionSubnet**. Este valor permite a Azure saber en qué subred se deben implementar los recursos de Bastion. Esto no es lo mismo que una subred de puerta de enlace. Debe usar una subred de al menos /27 o una subred más grande (/27, /26, etc.). Cree **AzureBastionSubnet** sin ninguna delegación ni tabla de rutas. Cuando use los grupos de seguridad de red en **AzureBastionSubnet**, consulte [Trabajar con Grupos de seguridad de red](bastion-nsg.md).

   ```azurepowershell-interactive
   $subnetName = "AzureBastionSubnet"
   $subnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   $vnet = New-AzVirtualNetwork -Name "myVnet" -ResourceGroupName " myBastionRG " -Location "westeurope" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

2. Cree una dirección IP pública para Azure Bastion. La IP pública es la dirección IP pública del recurso de Bastion en la que se accederá a RDP/SSH (a través del puerto 443). La dirección debe estar en la misma región que el recurso de Bastion que está creando.

   ```azurepowershell-interactive
   $publicip = New-AzPublicIpAddress -ResourceGroupName "myBastionRG" -name "myPublicIP" -location "westeurope" -AllocationMethod Dynamic -Sku Standard
   ```

3. Cree un nuevo recurso Azure Bastion en AzureBastionSubnet de su red virtual. El recurso de Bastion tardará aproximadamente cinco minutos en crearse e implementarse.

   ```azurepowershell-interactive
   $bastion = New-AzBastion -ResourceGroupName " myBastionRG " -Name "myBastion" -PublicIpAddress $publicip -VirtualNetwork $vnet
   ```

## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).