---
title: Configuración de Global Reach de Azure ExpressRoute | Microsoft Docs
description: Este artículo le ayudará a vincular los circuitos de ExpressRoute para realizar una red privada entre las redes locales y habilitar Global Reach.
documentationcenter: na
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mialdrid
ms.openlocfilehash: 67fbf9dc430d615efe3ef894add1a26bbce792bc
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50237985"
---
# <a name="configure-expressroute-global-reach-preview"></a>Configuración de Global Reach de Azure ExpressRoute (versión preliminar)
Este artículo le ayuda a configurar Global Reach de ExpressRoute con PowerShell. Para más información, consulte [Global Reach de ExpressRoute](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Antes de empezar
> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Antes de iniciar la configuración, deberá comprobar los requisitos siguientes.

* Instale la versión más reciente de Azure PowerShell. Consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Comprenda los [flujos de trabajo](expressroute-workflows.md) del aprovisionamiento del circuito ExpressRoute.
* Asegúrese de que los circuitos ExpressRoute están en estado aprovisionado.
* Asegúrese de que el emparejamiento privado de Azure está configurado en los circuitos ExpressRoute.  

### <a name="log-into-your-azure-account"></a>Inicie sesión en la cuenta de Azure.
Antes de iniciar la configuración, debe iniciar sesión en la cuenta de Azure. 

Abra la consola de PowerShell con privilegios elevados y conéctese a su cuenta. El comando pedirá las credenciales de inicio de sesión para la cuenta de Azure.  

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
Puede permitir Global Reach de ExpressRoute entre cualquiera de los dos circuitos ExpressRoute siempre que se encuentren en los países admitidos y se creen en distintas ubicaciones de emparejamiento. Si ambos circuitos pertenecen a la suscripción, puede elegir cualquiera de ellos para ejecutar la configuración en las secciones siguientes. Si los dos circuitos están en diferentes suscripciones de Azure, se necesita autorización de una suscripción de Azure y pasar la clave de autorización al ejecutar el comando de configuración en la otra suscripción de Azure.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Habilitación de la conectividad entre las redes locales

Use los siguientes comandos para obtener el circuito 1 y el circuito 2. Los dos circuitos están en la misma suscripción.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

Ejecute el siguiente comando en el circuito 1 y pase el identificador de emparejamiento privado del circuito 2.

> [!NOTE]
> El identificador de emparejamiento privado es similar a */subscriptions/{identificador_de_suscripción}/resourceGroups/{grupo_de_recursos}/providers/Microsoft.Network/expressRouteCircuits/{nombre_del_circuito}/peerings/AzurePrivatePeering*
> 
>

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

> [!IMPORTANT]
> *-AddressPrefix* debe ser una subred IPv4 /29, por ejemplo "10.0.0.0/29". Usaremos las direcciones IP de esta subred para establecer conectividad entre los dos circuitos ExpressRoute. No debe utilizar direcciones de esta subred en las redes virtuales de Azure ni en las redes locales.
> 



Guarde la configuración en el circuito 1
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Una vez concluida la operación anterior, debería tener conectividad entre las redes locales en ambos lados a través de los dos circuitos de ExpressRoute.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Circuitos ExpressRoute en distintas suscripciones de Azure

Si los dos circuitos no están en la misma suscripción de Azure, necesitará autorización. En la configuración siguiente, se genera autorización en la suscripción del circuito 2 y la clave de autorización se pasa al circuito 1.

Genere una clave de autorización. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Tome nota del identificador de emparejamiento privado del circuito 2, así como de la clave de autorización.

Ejecute el siguiente comando en el circuito 1. Pase el identificador de emparejamiento privado del circuito 2 y la clave de autorización 
```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

Guarde la configuración en el circuito 1
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Una vez concluida la operación anterior, debería tener conectividad entre las redes locales en ambos lados a través de los dos circuitos de ExpressRoute.

## <a name="get-and-verify-the-configuration"></a>Obtención y comprobación de la configuración

Use el siguiente comando para comprobar la configuración en el circuito donde se ha realizado, por ejemplo, el circuito 1 del ejemplo anterior.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Si ejecuta simplemente *$ckt1* en PowerShell, verá el valor de *CircuitConnectionStatus* en la salida. Le indicará si se establece la conectividad: "Conectado" o, en caso contrario, "Desconectado". 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Deshabilitación de la conectividad entre las redes locales

Para deshabilitar la conectividad, ejecute los comandos en el circuito en el que se ha realizado la configuración, por ejemplo, el circuito 1 del ejemplo anterior.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Puede ejecutar la operación Get para comprobar el estado. 

Tras finalizar la operación anterior, ya no tendrá conectividad entre las redes locales a través de los circuitos ExpressRoute. 


## <a name="next-steps"></a>Pasos siguientes
1. [Más información acerca de Global Reach de ExpressRoute](expressroute-global-reach.md)
2. [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
3. [Vinculación de un circuito ExpressRoute a una red virtual de Azure](expressroute-howto-linkvnet-arm.md)


