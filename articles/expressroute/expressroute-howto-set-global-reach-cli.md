---
title: Configuración de Global Reach de Azure ExpressRoute mediante la CLI de Azure | Microsoft Docs
description: Este artículo le ayudará a vincular los circuitos de ExpressRoute para realizar una red privada entre las redes locales y habilitar Global Reach.
documentationcenter: na
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: cherylmc
ms.openlocfilehash: 9d41ab26876d464187466f566bbfafc4861c799d
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333301"
---
# <a name="configure-expressroute-global-reach-using-azure-cli-preview"></a>Configuración de Global Reach de ExpressRoute mediante la CLI de Azure (versión preliminar)
Gracias a este artículo podrá configurar Global Reach de ExpressRoute con la CLI de Azure. Para más información, consulte [Global Reach de ExpressRoute](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Antes de empezar
> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Antes de iniciar la configuración, deberá comprobar los requisitos siguientes.

* Instale la versión más reciente de la CLI de Azure. Consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli) e [Introducción a la CLI de Azure](/cli/azure/get-started-with-azure-cli).
* Comprenda los [flujos de trabajo](expressroute-workflows.md) del aprovisionamiento del circuito ExpressRoute.
* Asegúrese de que los circuitos ExpressRoute están en estado aprovisionado.
* Asegúrese de que el emparejamiento privado de Azure está configurado en los circuitos ExpressRoute.  

### <a name="log-into-your-azure-account"></a>Inicie sesión en la cuenta de Azure.
Antes de iniciar la configuración, debe iniciar sesión en la cuenta de Azure. El comando abrirá el explorador predeterminado y le pedirá las credenciales de inicio de sesión para la cuenta de Azure.  

```azurecli
az login
```

Si tiene varias suscripciones de Azure, compruebe las suscripciones de la cuenta.

```azurecli
az account list
```

Especifique la suscripción que desea usar.

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Identificación de los circuitos ExpressRoute para la configuración
Puede permitir Global Reach de ExpressRoute entre cualquiera de los dos circuitos ExpressRoute siempre que se encuentren en los países admitidos y se creen en distintas ubicaciones de emparejamiento. Si ambos circuitos pertenecen a la suscripción, puede elegir cualquiera de ellos para ejecutar la configuración en las secciones siguientes. Si los dos circuitos están en diferentes suscripciones de Azure, se necesita autorización de una suscripción de Azure y pasar la clave de autorización al ejecutar el comando de configuración en la otra suscripción de Azure.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Habilitación de la conectividad entre las redes locales

Ejecute la siguiente CLI para conectar dos circuitos de ExpressRoute.

> [!NOTE]
> El valor de *peer-circuit* debe ser el id. de recurso completo; por ejemplo:
> ```
> */subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}*
> ```
> 

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

> [!IMPORTANT]
> *-AddressPrefix* debe ser una subred IPv4 /29, por ejemplo "10.0.0.0/29". Usaremos las direcciones IP de esta subred para establecer conectividad entre los dos circuitos ExpressRoute. No debe utilizar direcciones de esta subred en las redes virtuales de Azure ni en las redes locales.
> 

El resultado de la CLI tendrá un aspecto similar al siguiente.

```azurecli
{
  "addressPrefix": "<__.__.__.__/29>",
  "authorizationKey": null,
  "circuitConnectionStatus": "Connected",
  "etag": "W/\"48d682f9-c232-4151-a09f-fab7cb56369a\"",
  "expressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<ResourceGroupName>"
  },
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering/connections/<ConnectionName>",
  "name": "<ConnectionName>",
  "peerExpressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<Circuit2ResourceGroupName>"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "<ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/peerings/connections"
}
```

Una vez concluida la operación anterior, debería tener conectividad entre las redes locales en ambos lados a través de los dos circuitos de ExpressRoute.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Circuitos ExpressRoute en distintas suscripciones de Azure

Si los dos circuitos no están en la misma suscripción de Azure, necesitará autorización. En la configuración siguiente, se genera autorización en la suscripción del circuito 2 y la clave de autorización se pasa al circuito 1.

Genere una clave de autorización. 
```azurecli
az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
```

El resultado de la CLI tendrá un aspecto similar al siguiente.

```azurecli
{
  "authorizationKey": "<authorizationKey>",
  "authorizationUseStatus": "Available",
  "etag": "W/\"cfd15a2f-43a1-4361-9403-6a0be00746ed\"",
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/authorizations/<AuthorizationName>",
  "name": "<AuthorizationName>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<Circuit2ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/authorizations"
}
```

Tome nota del identificador del recurso del circuito 2, así como de la clave de autorización.

Ejecute el siguiente comando en el circuito 1. Escriba el identificador del recurso del circuito 2 y la clave de autorización. 
```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
```

Una vez concluida la operación anterior, debería tener conectividad entre las redes locales en ambos lados a través de los dos circuitos de ExpressRoute.

## <a name="get-and-verify-the-configuration"></a>Obtención y comprobación de la configuración

Use el siguiente comando para comprobar la configuración en el circuito donde se ha realizado, por ejemplo, el circuito 1 del ejemplo anterior.

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

En la salida de la CLI verá *CircuitConnectionStatus*. Le indicará si se estableció la conectividad entre los dos circuitos indicando el estado "Conectado" o, en caso contrario, "Desconectado". 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Deshabilitación de la conectividad entre las redes locales

Para deshabilitar la conectividad, ejecute los comandos en el circuito en el que se ha realizado la configuración, por ejemplo, el circuito 1 del ejemplo anterior.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

Puede ejecutar la CLI Show para comprobar el estado. 

Tras finalizar la operación anterior, ya no tendrá conectividad entre las redes locales a través de los circuitos ExpressRoute. 


## <a name="next-steps"></a>Pasos siguientes
* [Más información acerca de Global Reach de ExpressRoute](expressroute-global-reach.md)
* [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Vinculación de un circuito ExpressRoute a una red virtual de Azure](expressroute-howto-linkvnet-arm.md)


