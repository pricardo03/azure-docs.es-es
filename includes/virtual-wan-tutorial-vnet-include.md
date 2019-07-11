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
ms.openlocfilehash: 40ae634897361219c39e60d2161d3576cc44a400
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077518"
---
Para crear fácilmente una red virtual, haga clic en "Pruébelo" en este artículo para abrir una consola de PowerShell en Azure Cloud Shell. Ajuste los valores, y luego copie y pegue los comandos en la ventana de consola. 

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
