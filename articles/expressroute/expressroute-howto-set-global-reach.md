---
title: 'Configuración Global Reach - ExpressRoute: Azure | Microsoft Docs'
description: Este artículo le ayudará a vincular los circuitos de ExpressRoute para realizar una red privada entre las redes locales y habilitar Global Reach.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: ab1098ca65ad92cffdbe1dfb24fd43fcc8f10eae
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54431685"
---
# <a name="configure-expressroute-global-reach-preview"></a>Configuración de Global Reach de ExpressRoute (versión preliminar)
Este artículo le ayuda a configurar Global Reach de ExpressRoute con PowerShell. Para más información, consulte [Global Reach de ExpressRoute](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Antes de empezar
> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin contrato de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Antes de comenzar la configuración, confirme lo siguiente:

* Que ha instalado la versión más reciente de Azure PowerShell. Para más información, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).
* Que conoce los [flujos de trabajo](expressroute-workflows.md) del aprovisionamiento de circuitos ExpressRoute.
* Asegúrese de que los circuitos ExpressRoute están en estado aprovisionado.
* Que el emparejamiento privado de Azure está configurado en los circuitos ExpressRoute.  

### <a name="sign-in-to-your-azure-account"></a>Inicio de sesión en la cuenta de Azure.
Para comenzar la configuración, inicie sesión en la cuenta de Azure. 

Abra la consola de PowerShell con privilegios elevados y conéctese a su cuenta. El comando le pide las credenciales de inicio de sesión de la cuenta de Azure.  

```powershell
Connect-AzureRmAccount
```

Si tiene varias suscripciones de Azure, compruebe las suscripciones de la cuenta.

```powershell
Get-AzureRmSubscription
```

Especifique la suscripción que desea usar.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Identificación de los circuitos ExpressRoute para la configuración
Puede habilitar Global Reach de ExpressRoute entre dos circuitos ExpressRoute cualesquiera, siempre que se encuentren en los países admitidos y se hayan creado en distintas ubicaciones de emparejamiento. Si ambos circuitos pertenecen a la suscripción, puede elegir cualquiera de ellos para ejecutar la configuración en las secciones siguientes. 

Si los circuitos están en diferentes suscripciones de Azure, necesita la autorización de una suscripción de Azure. Luego debe usar la clave de autorización al ejecutar el comando de configuración en la otra suscripción de Azure.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Habilitación de la conectividad entre las redes locales

Use los siguientes comandos para obtener el circuito 1 y el circuito 2. Los dos circuitos están en la misma suscripción.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

Ejecute el siguiente comando en el circuito 1 y use el identificador de emparejamiento privado del circuito 2. Al ejecutar el comando, tenga en cuenta lo siguiente:

* El identificador de emparejamiento privado es similar al del siguiente ejemplo: 

  ```
  /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering
  ```
* *-AddressPrefix* debe ser una subred IPv4 /29, por ejemplo, "10.0.0.0/29". Las direcciones IP de esta subred se usan para establecer conectividad entre los dos circuitos ExpressRoute. Las direcciones de esta subred no se deben usar en las redes virtuales de Azure ni en la red local.

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

Guarde la configuración en el circuito 1 como se indica a continuación:
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Una vez concluida la operación anterior, debería tener conectividad entre las redes locales en ambos lados a través de los dos circuitos ExpressRoute.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Circuitos ExpressRoute en distintas suscripciones de Azure

Si los dos circuitos no están en la misma suscripción de Azure, necesita autorización. En la configuración siguiente, la autorización se genera en la suscripción del circuito 2 y la clave de autorización se pasa al circuito 1.

Genere una clave de autorización. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Anote el identificador de emparejamiento privado del circuito 2, así como de la clave de autorización.

Ejecute el siguiente comando en el circuito 1. Pase el identificador de emparejamiento privado del circuito 2 y la clave de autorización.
```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

Guarde la configuración en el circuito 1.
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Una vez concluida la operación anterior, debería tener conectividad entre las redes locales en ambos lados a través de los dos circuitos ExpressRoute.

## <a name="get-and-verify-the-configuration"></a>Obtención y comprobación de la configuración

Use el siguiente comando para comprobar la configuración en el circuito en el que se realizó, por ejemplo, el circuito 1 en el ejemplo anterior.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Si solo ejecuta *$ckt1* en PowerShell, se ve el valor de *CircuitConnectionStatus* en la salida. Le indica si se establece la conectividad, "Conectado" o "Desconectado". 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Deshabilitación de la conectividad entre las redes locales

Para deshabilitar la conectividad, ejecute los comandos en el circuito en el que se ha realizado la configuración, (por ejemplo, el circuito 1 en el ejemplo anterior).

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Puede ejecutar la operación Get para comprobar el estado. 

Tras finalizar la operación anterior, dejará de tener conectividad entre la red local a través de los circuitos ExpressRoute. 


## <a name="next-steps"></a>Pasos siguientes
1. [Más información acerca de Global Reach de ExpressRoute](expressroute-global-reach.md)
2. [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
3. [Vinculación de un circuito ExpressRoute a una red virtual de Azure](expressroute-howto-linkvnet-arm.md)


