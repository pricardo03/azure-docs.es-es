---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 660bbf50e1a8ae73bd7bbe1f7c42691ed62d276a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552990"
---
Si no dispone de una red virtual, puede crear rápidamente una mediante PowerShell. También puede crear una red virtual mediante Azure Portal.

* Asegúrese de que el espacio de direcciones para la red virtual que cree no se superpone a ninguno de los intervalos de direcciones de otras redes virtuales a las que desee conectarse, o a los espacios de direcciones de red local. 
* Si ya tiene una red virtual, verifique que cumple los criterios necesarios y no tiene una puerta de enlace de red virtual.

Puede crear fácilmente su red virtual haciendo clic en "Pruébelo" en este artículo para abrir una consola de PowerShell. Ajuste los valores, y luego copie y pegue los comandos en la ventana de consola.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Ajuste los comandos de PowerShell y luego cree un grupo de recursos.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Creación de una red virtual

Ajuste los comandos de PowerShell para crear la red virtual que sea compatible con su entorno.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```