---
title: Escenario de aplicación de Azure ExpressRoute Global Reach | Microsoft Docs
description: En esta página se proporciona un escenario de aplicación de Global Reach.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,global reach
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: rambala
ms.openlocfilehash: 5174baae40d7f14720affe90f702c024541a5745
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54333085"
---
# <a name="expressroute-global-reach-application-scenario"></a>Escenario de aplicación de ExpressRoute Global Reach

Para más información sobre ExpressRoute Global Reach, consulte [ExpressRoute Global Reach][Global Reach]. En este artículo vamos a recorrer un escenario de aplicación, comparar la solución ExpressRoute Global Reach con otras, configurar Global Reach para el escenario de ejemplo y verificar las conexiones. 

##<a name="application-scenario"></a>Escenario de aplicación

Fabrikam Inc. tiene una presencia física importante e implementación de Azure en el Este de EE. UU. Fabrikam tiene conectividad de back-end entre sus servidores locales y las implementaciones de Azure a través de ExpressRoute. Contoso Ltd. tiene presencia e implementación de Azure en el Oeste de EE. UU. Contoso tiene conectividad de back-end entre sus servidores locales y las implementaciones de Azure a través de ExpressRoute.  

Fabrikam Inc. adquiere Contoso Ltd. Después de la fusión, Fabrikam desea interconectar las redes. En la siguiente ilustración se muestra este escenario:

 [![1]][1]

Las flechas discontinuas en medio de la ilustración anterior indican las interconexiones de red necesarias. En la siguiente tabla se muestra la tabla de rutas del emparejamiento privado principal con ExpressRoute de Contoso Ltd. antes de la fusión.

[![2]][2]

En la siguiente tabla se muestra la tabla de rutas del emparejamiento privado principal con ExpressRoute de Fabrikam Ltd. antes de la fusión.

[![3]][3]

## <a name="traditional-solutions"></a>Soluciones tradicionales

### <a name="option-1-interconnect-on-premises-networks"></a>Opción 1: Interconexión de las redes locales

En la siguiente ilustración se muestra esta opción. Como se muestra, puede interconectar las dos redes locales mediante VPN de sitio a sitio o de otras opciones de conexión de banda ancha y administrar todo el enrutamiento entre las dos entidades. 

[![4]][4]

Esta opción tiene las siguientes desventajas:

- Se fuerza la comunicación interregional de Azure para la implementación en una ruta subóptima.
- Se deniega la ventaja de la alta disponibilidad de la red troncal de Microsoft para la comunicación interregional.
- La responsabilidad del enrutamiento para la comunicación interregional es suya totalmente.

### <a name="option-2-expressroute-cross-connectivity-and-interconnect-on-premises-networks"></a>Opción 2: Conectividad cruzada de ExpressRoute e interconexión entre las redes locales

En la siguiente ilustración se muestra esta opción.

[![5]][5]

Como se muestra en la ilustración anterior, además puede establecer conectividad entre los circuitos de ExpressRoute y las redes virtuales regionales cruzadas. La conectividad regional cruzada entre las redes virtuales y los circuitos de ExpressRoute habilitaría las siguientes comunicaciones:

- Que las redes virtuales del Oeste de EE. UU. y el Este de EE. UU. se comuniquen con las redes locales de Fabrikam/Contoso respectivamente.
- Que la red virtual del Oeste de EE. UU. se comunique con la red virtual del Este de EE. UU.

La interconexión entre las dos redes locales sigue siendo necesaria para que estas se comuniquen entre sí.

Esta opción permite que la comunicación interregional de Azure para la implementación privada se conduzca a través de la red troncal de Microsoft y que la comunicación en la red local que se realice a través de la red externa. Sin embargo, el principal inconveniente de la solución es que deberá establecer varias conexiones regionales cruzadas, lo que complica el mantenimiento y la solución de problemas. Además, la opción no permite aprovechar la alta disponibilidad de la red troncal global de Microsoft para la comunicación entre las dos redes locales.

### <a name="option-3-vnet-peering-and-interconnect-on-premises-networks"></a>Opción 3: Emparejamiento de redes virtuales e interconexión de las redes locales

En la siguiente ilustración se muestra esta opción. Para esta opción se usa el emparejamiento de redes virtuales para la comunicación interregional entre redes virtuales. Como se muestra, esta opción es incompleta, ya que no abarca la comunicación entre las redes virtuales de las regiones cruzadas y las locales (lo cual se indica con las dos líneas de puntos y terminadas en flecha del centro). Para este tipo de comunicación, use la conexión de red virtual local a red virtual local (como en la opción 1) o la conectividad cruzada de ExpressRoute (como en la opción 2).

[![6]][6]

Esta opción sí que proporciona un enrutamiento óptimo para la comunicación interregional entre redes virtuales. Sin embargo, se queda corto si Fabrikam o Contoso tienen una implementación más compleja de Azure, como un modelo de red virtual tipo hub-and-spoke. Además, como las dos opciones anteriores, esta no permite aprovechar la alta disponibilidad de la red troncal global de Microsoft para la comunicación entre las dos redes locales.

## <a name="global-reach"></a>Global Reach

ExpressRoute Global Reach vincula el emparejamiento privado de los circuitos ExpressRoute, como se muestra en la siguiente ilustración:

[![7]][7]

La configuración de Global Reach entre los dos circuitos de ExpressRoute permite la comunicación privada entre las dos redes locales y la implementación de Azure en las dos regiones. Por lo tanto, Global Reach logra la comunicación deseada (lo cual se indica con la línea de puntos de la primera ilustración de este artículo) en la red troncal de Microsoft.

### <a name="configure-global-reach"></a>Configuración de Global Reach

Para aprender a configurar ExpressRoute Global Reach, consulte [Configuración de Global Reach][Configure Global Reach]. 

Dado que Contoso Ltd. y Fabrikam Inc. tenían Azure incorporado cuando eran compañías independientes, sus circuitos de ExpressRoute se encuentran en suscripciones de Azure distintas. Para crear la instancia de Global Reach en las suscripciones debe crear una autorización en el circuito de ExpressRoute de Contoso Ltd. y pasarla a Fabrikam Inc. Circuito de ExpressRoute.


Para crear una autorización para el circuito de ExpressRoute de Contoso, primero debe iniciar sesión en la cuenta de Azure de Contoso y, después, seleccionar la suscripción adecuada (si hay varias). Los comandos de PowerShell para estos pasos son los siguientes:

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```
Los pasos para crear una autorización de circuito de ExpressRoute se enumeran a continuación:

```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```

En la salida de "Set-AzureRmExpressRouteCircuit" se mostrará la lista de ExpressRouteCircuit. Anote el identificador de emparejamiento privado y la clave de autorización que aparecen al final de la lista. A continuación, un fragmento de salida de PowerShell de ejemplo:

```powershell
ServiceKey                       : <serviceKey>
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 65020,
                                       "PrimaryPeerAddressPrefix": "192.168.11.16/30",
                                       "SecondaryPeerAddressPrefix": "192.168.11.20/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 110,
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "4",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : [
                                     {
                                       "Name": "Auth4-ASH-Cust11",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/authorizations/Auth4-ASH-Cust11",
                                       "AuthorizationKey": "<authorizationKey>",
                                       "AuthorizationUseStatus": "Used",
                                       "ProvisioningState": "Succeeded"
                                     }
                                   ]
AllowClassicOperations           : False
GatewayManagerEtag               : 4
```

Con el identificador de emparejamiento y la clave de autorización puede crear la instancia de Global Reach en el circuito de ExpressRoute de Fabrikam. Inicie sesión en la cuenta de Azure de Fabrikam. Si hay más de una suscripción, seleccione la apropiada.

Global Reach crea un conjunto redundante de conexiones de punto a punto en los dos pares de MSEE. Para las dos conexiones de punto a punto, debe especificar un prefijo de dirección /29 (para este ejemplo usaremos 192.168.11.64/29). Para crear la conexión de Global Reach, use los siguientes comandos:

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix "__.__.__.__/29" -AuthorizationKey "########-####-####-####-############"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Una vez ejecutados los comandos anteriores, las conexiones redundantes de Global Reach tardarán un par de minutos en crearse.

### <a name="verify-global-reach"></a>Verificación de Global Reach

En la siguiente tabla se muestra la tabla de rutas del emparejamiento privado principal con ExpressRoute de Contoso Ltd. después de la configuración de Global Reach.

[![8]][8]

En la siguiente tabla se muestra la tabla de rutas del emparejamiento privado principal con ExpressRoute de Fabrikam Inc. después de la configuración de Global Reach.

[![9]][9]

En las tablas anteriores se observan todos los prefijos "NETWORK" de destino previstos y se enumeran los valores de "NEXT HOP" apropiados.

Arriba se ve la hoja "Obtener la tabla de rutas", a la que se puede acceder desde Azure Portal, en "Emparejamiento privado" de un circuito de ExpressRoute. También puede especificar una tabla de rutas de ExpressRoute con el siguiente comando de PowerShell:

```powershell
Get-AzExpressRouteCircuitRouteTable -DevicePath 'primary' -ExpressRouteCircuitName "Your_circuit_name" -PeeringType AzurePrivatePeering -ResourceGroupName "Your_resource_group"
```

Para ver la tabla de rutas del MSEE secundario, sustituya "primary" por la palabra clave "secondary" en el comando anterior.

Vamos a también a verificar la conectividad del plano de datos al hacer ping con un destino distinto desde varias redes. Los tres pings siguientes verifican la conectividad del plano de datos de la red local de Contoso, la red virtual de Azure de Contoso y la red virtual de Azure de Fabrikam desde la red local de esta.


    C:\Users\FabrikamUser>ping 10.1.11.10
    
    Pinging 10.1.11.10 with 32 bytes of data:
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    
    Ping statistics for 10.1.11.10:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 69ms, Maximum = 69ms, Average = 69ms
    
    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=82ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=81ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 82ms, Average = 78ms
    
    C:\Users\FabrikamUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=2ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 2ms, Maximum = 3ms, Average = 2ms


Los dos pings siguientes verifican la conectividad del plano de datos de la red virtual de Azure de Contoso y la red virtual de Azure de Fabrikam desde la red local de Contoso.

    C:\Users\ContosoUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=6ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 5ms, Maximum = 6ms, Average = 5ms
    
    C:\Users\ContosoUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=74ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 73ms, Maximum = 74ms, Average = 73ms

El siguiente ping verifica la conectividad del plano de datos en la red virtual de Azure de Contoso desde la red virtual de Azure de Fabrikam.

    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=78ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 78ms, Average = 77ms

## <a name="optimization-for-latency-sensitive-traffic"></a>Optimización para el tráfico confidencial con latencia

Global Reach enruta el tráfico mediante dispositivos de Microsoft Edge. En el escenario específico de este artículo, puede conseguir un enrutamiento mejor entre las dos redes virtuales al habilitar el emparejamiento de redes virtuales entre ellas. Igualmente, puede conseguir mejor enrutamiento entre las redes locales mediante un proveedor de servicios que ofrezca una conexión WAN más directa entre los sitios. En estos escenarios, puede usar el enrutamiento de Global Reach como opción de conmutación por recuperación para esas conexiones. 

## <a name="next-steps"></a>Pasos siguientes

Global Reach se está dando a conocer país a país. Para ver si Global Reach está disponible en los países que desea, consulte [ExpressRoute Global Reach][Global Reach].

<!--Image References-->
[1]: ./media/expressroute-globalreach-applicationscenario/PreMergerScenario.png "Escenario de aplicación"
[2]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Premerger.png "Tabla de rutas de ExpressRoute de Contoso antes de la fusión"
[3]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Premerger.png "Tabla de rutas de ExpressRoute de Fabrikam antes de la fusión"
[4]: ./media/expressroute-globalreach-applicationscenario/S2SVPN-Solution.png "Interconexión de las redes locales"
[5]: ./media/expressroute-globalreach-applicationscenario/ExR-XConnect-Solution.png "Conexión cruzada de ExpressRoute"
[6]: ./media/expressroute-globalreach-applicationscenario/VNet-peering-Solution.png "Emparejamiento de redes virtuales"
[7]: ./media/expressroute-globalreach-applicationscenario/GlobalReach-Solution.png "Global Reach"
[8]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Postmerger.png "Tabla de rutas de ExpressRoute de Contoso con Global Reach"
[9]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Postmerger.png "Tabla de rutas de ExpressRoute de Fabrikam con Global Reach"

<!--Link References-->
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach





