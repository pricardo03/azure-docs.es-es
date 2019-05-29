---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 04/23/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 40c8cb41ad3bcd46e9973a5f96134ff1bfd02fd2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150801"
---
Para crear fácilmente una red virtual, haga clic en "Pruébelo" en este artículo para abrir una consola de PowerShell. Ajuste los valores, y luego copie y pegue los comandos en la ventana de consola. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](/powershell/azure/new-azureps-module-az) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az, consulte [Instalación de Azure PowerShell](/powershell/azure/install-az-ps).

Asegúrese de que el espacio de direcciones para la red virtual que cree no se superpone a ninguno de los intervalos de direcciones de otras redes virtuales a las que desee conectarse, o a los espacios de direcciones de red local.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Si aún no tiene un grupo de recursos que quiera usar, cree uno. Ajuste los comandos de PowerShell para que reflejen el nombre del grupo de recursos que desea utilizar y, a continuación, ejecute el siguiente cmdlet:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Creación de una red virtual

Ajuste los comandos de PowerShell para crear una red virtual que sea compatible con su entorno.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```