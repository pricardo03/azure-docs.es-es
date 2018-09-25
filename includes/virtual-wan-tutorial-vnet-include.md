---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1fae3c3889242dfbf8f270d3762ea7434ceda6da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004192"
---
Si no dispone de una red virtual, puede crear rápidamente una mediante PowerShell. También puede crear una red virtual mediante Azure Portal.

* Asegúrese de que el espacio de direcciones para la red virtual que cree no se superpone a ninguno de los intervalos de direcciones de otras redes virtuales a las que desee conectarse, o a los espacios de direcciones de red local. 
* Si ya tiene una red virtual, verifique que cumple los criterios necesarios y no tiene una puerta de enlace de red virtual.

Puede crear fácilmente su red virtual haciendo clic en "Pruébelo" en este artículo para abrir una consola de PowerShell. Ajuste los valores, y luego copie y pegue los comandos en la ventana de consola.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Ajuste los comandos de PowerShell y luego cree un grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Creación de una red virtual

Ajuste los comandos de PowerShell para crear la red virtual que sea compatible con su entorno.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```