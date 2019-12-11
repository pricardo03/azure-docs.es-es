---
title: Creación de un grupo de seguridad de red (clásico) mediante PowerShell | Microsoft Docs
description: Obtenga información sobre cómo crear e implementar grupos de seguridad de red mediante PowerShell.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: ''
tags: azure-service-management
ms.assetid: 86810b0d-0240-46a2-8548-fca22daa56f3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 2/02/2019
ms.author: genli
ms.openlocfilehash: 0f957c5d59dec06057841a95ec48a54462778a69
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74672478"
---
# <a name="create-a-network-security-group-classic-using-powershell"></a>Creación de un grupo de seguridad de red (clásico) mediante PowerShell
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Este artículo trata sobre el modelo de implementación clásico. También puede [crear grupos de seguridad de red con el modelo de implementación del Administrador de recursos](tutorial-filter-network-traffic.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

En los siguientes comandos de PowerShell de ejemplo se presupone que ya se ha creado un entorno simple según el escenario anterior. Si desea ejecutar los comandos tal y como aparecen en este documento, compile primero el entorno de prueba mediante la [creación de una red virtual](virtual-networks-create-vnet-classic-netcfg-ps.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Creación de un grupo de seguridad de red para la subred front-end

1. Si no tiene instalado Azure PowerShell, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview).

2. Cree un grupo de seguridad de red denominado *NSG-FrontEnd*:

    ```powershell   
    New-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" -Location uswest `
      -Label "Front end subnet NSG"
   ```

3. Cree una regla de seguridad que permita el acceso desde Internet al puerto 3389:

    ```powershell   
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name rdp-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
      -SourceAddressPrefix Internet  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '3389'
   ```

4. Cree una regla de seguridad que permita el acceso desde Internet al puerto 80:

    ```powershell   
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name web-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 200 `
      -SourceAddressPrefix Internet  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '80'
    ```
5. Asocie el grupo de seguridad de red con una subred:

    ```powershell   
    Get-AzureNetworkSecurityGroup -Name "NSG-Frontend" `
    | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName "TestVNet" `
    -Subnet "FrontEnd"
    ```
## <a name="create-an-nsg-for-the-back-end-subnet"></a>Creación de un grupo de seguridad de red para la subred de back-end

1. Cree un grupo de seguridad de red denominado *NSG-BackEnd*:
   
    ```powershell
    New-AzureNetworkSecurityGroup -Name "NSG-BackEnd" -Location uswest `
      -Label "Back end subnet NSG"
    ```

2. Crear una regla de seguridad que permita el acceso desde la subred front-end al puerto 1433 (puerto predeterminado utilizado por SQL Server):
   
    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name rdp-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
      -SourceAddressPrefix 192.168.1.0/24  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '1433'
    ```

3. Cree una regla de seguridad que bloquee el acceso a Internet desde la subred:
   
    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-BackEnd" `
      | Set-AzureNetworkSecurityRule -Name block-internet `
      -Action Deny -Protocol '*' -Type Outbound -Priority 200 `
      -SourceAddressPrefix '*'  -SourcePortRange '*' `
      -DestinationAddressPrefix Internet -DestinationPortRange '*'
   ```
4. Asocie el grupo de seguridad de red con una subred:
    ```powershell   
    Get-AzureNetworkSecurityGroup -Name "NSG-Backend" `
    | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName "TestVNet" `
    -Subnet "BackEnd"
    ```