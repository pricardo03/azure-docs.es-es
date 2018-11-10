---
title: archivo de inclusión
description: archivo de inclusión
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 11/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fab00e5281bb91bce10228b3bc2e9cfd503d5d5b
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219824"
---
Antes de efectuar las siguientes tareas, debe crear una red virtual y una subred de puerta de enlace.

> [!NOTE]
> Estos ejemplos no se aplican a configuraciones coexistentes de S2S y ExpressRoute.
> Para más información acerca de cómo trabajar con puertas de enlace en una configuración de coexistencia, consulte [Configuración de conexiones coexistentes](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Adición de una puerta de enlace

Utilice el siguiente comando para crear una puerta de enlace. Asegúrese de sustituir los valores por los suyos propios.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Comprobación de la creación de la puerta de enlace

Utilice el siguiente comando para comprobar si se ha creado la puerta de enlace. Este comando también recupera el identificador de la puerta de enlace, que necesita para realizar otras operaciones.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Cambio del tamaño de una puerta de enlace

Hay varias [SKU de puerta de enlace](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Puede utilizar el siguiente comando para cambiar en cualquier momento la SKU de puerta de enlace.

> [!IMPORTANT]
> Este comando no funciona para la puerta de enlace de UltraPerformance. Para cambiar la puerta de enlace a una puerta de enlace de UltraPerformance, quite primero la puerta de enlace de ExpressRoute existente y, después, cree una nueva puerta de enlace de UltraPerformance. Para degradar la puerta de enlace desde una puerta de enlace de UltraPerformance, quite primero la puerta de enlace de UltraPerformance existente y, después, cree una nueva puerta de enlace. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Eliminación de una puerta de enlace

Utilice el siguiente comando para quitar una puerta de enlace.

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```
