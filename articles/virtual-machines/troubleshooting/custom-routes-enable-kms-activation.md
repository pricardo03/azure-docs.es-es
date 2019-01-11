---
title: Uso de rutas personalizadas de Azure para habilitar la activación de KMS con tunelización forzada | Microsoft Docs
description: Se muestra cómo usar rutas personalizadas de Azure para habilitar la activación de KMS con tunelización forzada.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: f1e2ab6a954361a7807d78dc2baf5d24af52a679
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53797584"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Error de activación de Windows en el escenario de tunelización forzada

En este artículo se describe cómo resolver el problema de activación de KMS que podría experimentar cuando se habilita de la tunelización forzada en escenarios de conexión VPN de sitio a sitio o ExpressRoute.

## <a name="symptom"></a>Síntoma

La [tunelización forzada](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) se habilita en las subredes de red virtual de Azure para dirigir todo el tráfico vinculado a Internet de vuelta a la red local. En este escenario, las máquinas virtuales (VM) de Azure que ejecutan Windows Server 2012 R2 o versiones posteriores pueden activar Windows correctamente. Pero las máquinas virtuales que ejecutan una versión anterior de Windows no pueden activar Windows. 

## <a name="cause"></a>Causa

Las máquinas virtuales Windows de Azure se deben conectar al servidor de KMS de Azure para la activación de Windows. La activación requiere que la solicitud de activación proceda de una dirección IP pública de Azure. En el escenario de tunelización forzada, se producirá un error en la activación porque la solicitud de activación procede de la red local en lugar de una dirección IP pública de Azure. 

## <a name="solution"></a>Solución

Para resolver este problema, use la ruta personalizada de Azure para dirigir el tráfico de activación al servidor de KMS de Azure (23.102.135.246). 

La dirección IP 23.102.135.246 es la del servidor de KMS para la nube global de Azure. El nombre DNS es kms.core.windows.net. Si usa otras plataformas de Azure como Azure Alemania, tendrá que usar la dirección IP del servidor de KMS correspondiente. Para más información, vea la tabla siguiente:

|Plataforma| DNS DE KMS|DIRECCIÓN IP DE KMS|
|------|-------|-------|
|Azure Global|kms.core.windows.net|23.102.135.246|
|Azure Alemania|kms.core.cloudapi.de|51.4.143.248|
|Azure US Government|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure China 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


Para agregar la ruta personalizada, siga estos pasos:

### <a name="for-resource-manager-vms"></a>Para máquinas virtuales de Resource Manager

1. Abra Azure PowerShell y después [inicie sesión en la suscripción de Azure](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Ejecute los comandos siguientes:

    ```powershell
    # First, we will get the virtual network hosts the VMs that has activation problems. In this case, I get virtual network ArmVNet-DM in Resource Group ArmVNet-DM

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, we create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out

    $RouteTable = New-AzureRmRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzureRmRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzureRmRouteTable -RouteTable $RouteTable
    ```
3. Vaya a la máquina virtual que tenga el problema de activación y use [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) para comprobar si puede acceder al servidor de KMS:

        psping kms.core.windows.net:1688

4. Intente activar Windows y compruebe si el problema se ha resuelto.

### <a name="for-classic-vms"></a>Para máquinas virtuales clásicas

1. Abra Azure PowerShell y después [inicie sesión en la suscripción de Azure](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Ejecute los comandos siguientes:

    ```powershell
    # First, we will create a new route table
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the routetable that was created
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply KMS route table to the subnet that host the problem VMs (in this case, I will apply it to the subnet named Subnet-1)
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Vaya a la máquina virtual que tenga el problema de activación y use [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) para comprobar si puede acceder al servidor de KMS:

        psping kms.core.windows.net:1688

4. Intente activar Windows y compruebe si el problema se ha resuelto.

## <a name="next-steps"></a>Pasos siguientes

- [Claves de configuración de cliente KMS](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Review and Select Activation Methods](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
) (Revisión y selección de métodos de activación)