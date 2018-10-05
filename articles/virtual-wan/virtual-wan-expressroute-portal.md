---
title: Uso de Azure Virtual WAN para crear conexiones de ExpressRoute para Azure y entornos locales | Microsoft Docs
description: En este tutorial, aprenderá a usar Azure Virtual WAN para crear conexiones de ExpressRoute con Azure y con entornos locales.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 09/26/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my corporoate on-premises network(s) to my VNets using Virtual WAN and ExpressRoute.
ms.openlocfilehash: 0b8de4d04d9cca47423634164e458e8699154f30
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405315"
---
# <a name="tutorial-create-an-expressroute-association-using-azure-virtual-wan-preview"></a>Tutorial: Creación de una asociación de ExpressRoute mediante Azure Virtual WAN (versión preliminar)

En este tutorial se muestra cómo usar Virtual WAN para conectarse a los recursos de Azure mediante una asociación y un circuito de ExpressRoute. Para más información sobre Virtual WAN, consulte la [Introducción a Virtual WAN](virtual-wan-about.md)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una WAN virtual
> * Crear un concentrador
> * Buscar y asociar un circuito con el centro
> * Asociar el circuito a un centro, o a varios
> * Conectar una red virtual a un concentrador
> * Visualizar la instancia de Virtual WAN
> * Visualizar el estado de los recursos
> * Supervisar una conexión

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Registro de esta característica

Haga clic en **Probar** para registrar esta característica fácilmente mediante Azure Cloud Shell.

>[!NOTE]
>Si no la registra, no podrá usarla ni verla en el portal.
>
>

Después de hacer clic en **Probar** para abrir Azure Cloud Shell, copie y pegue los siguientes comandos:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowCortexExpressRouteGateway
```
 
```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowCortexExpressRouteGateway
```

Una vez que la característica se muestre como registrada, vuelva a registrar la suscripción en el espacio de nombres Microsoft.Network.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Creación de una red virtual

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Creación de una instancia de Virtual WAN

En un explorador, vaya a [Azure Portal (versión preliminar)](http://aka.ms/azurevirtualwanpreviewfeatures) e inicie sesión con su cuenta de Azure.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

### <a name="getting-started-page"></a>Página de introducción

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-gettingstarted-include.md)]

## <a name="hub"></a>3. Crear un concentrador

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="hub"></a>4. Buscar y asociar un circuito con el centro

1. Seleccione su WAN virtual y en **Virtual WAN Architecture** (Arquitectura de WAN virtual), seleccione **ExpressRoute Circuits** (Circuitos de ExpressRoute)
2. Si el circuito de ExpressRoute se encuentra en la misma suscripción que su WAN virtual, haga clic en **Select ExpressRoute circuit** (Seleccionar circuito de ExpressRoute) de sus suscripciones 
3. Mediante la lista desplegable, seleccione la instancia de ExpressRoute que desea asociar al centro.
4. Si el circuito de ExpressRoute no está en la misma suscripción o se le ha proporcionado [una clave de autorización y un identificador de par ID](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md), seleccione **Find a circuit redeeming an authorization key** (Buscar un circuito que canjee una clave de autorización)
5. Escriba la siguiente información:
* **Clave de autorización** : generada por el propietario del circuito como se describió anteriormente
* **URI de circuito del mismo nivel**: identificador URI del circuito que proporciona el propietario del circuito y es el identificador único del circuito
* **Ponderación del enrutamiento** - [Routing Weight](../expressroute/expressroute-optimize-routing.md) permite elegir ciertas rutas de acceso cuando varios circuitos de diferentes ubicaciones de emparejamiento están conectados al mismo centro
6. Haga clic en **Find circuit** (Buscar circuito) y seleccione el circuito, si lo encuentra
7. Seleccione uno o varios centros en la lista desplegable y haga clic en **Guardar**

## <a name="vnet"></a>5. Conexión de la red virtual a un concentrador

En este paso, creará la conexión de emparejamiento entre una red virtual y el concentrador. Repita estos pasos para cada red virtual que desee conectar.

1. En la página de la red virtual WAN, haga clic en **Conexión de red virtual**.
2. En la página de conexión de red virtual, haga clic en **+ Agregar conexión**.
3. En la página **Agregar conexión**, rellene los campos siguientes:

    * **Nombre de la conexión**: asigne un nombre a la conexión.
    * **Centros**: seleccione el concentrador que desea asociar a esta conexión.
    * **Suscripción**: compruebe la suscripción.
    * **Red virtual**: seleccione la red virtual que quiere conectar con este concentrador. La red virtual no puede tener una puerta de enlace de red virtual ya existente.


## <a name="viewwan"></a>6. Visualizar la instancia de Virtual WAN

1. Vaya a la instancia de Virtual WAN.
2. En la página Información general, cada punto del mapa representa un concentrador. Mantenga el mouse sobre cualquier punto para ver el resumen de estado del concentrador.
3. En la sección de concentradores y conexiones, puede ver estado del concentrador, sitio, región, estado de la conexión VPN y bytes de entrada y salida.

## <a name="viewhealth"></a>7. Visualización del estado de los recursos

1. Vaya a su red WAN.
2. En la página de la WAN, en la sección de **Soporte técnico y solución de problemas**, haga clic en **Mantenimiento** y visualice los recursos.

## <a name="connectmon"></a>8. Supervisar una conexión

Cree una conexión para supervisar la comunicación entre una máquina virtual de Azure y un sitio remoto. Para información acerca de cómo configurar una supervisión de conexión, consulte [Supervisar la comunicación de red](~/articles/network-watcher/connection-monitor.md). El campo de origen es la IP de la máquina virtual en Azure y la dirección IP de destino es la dirección IP del sitio.

## <a name="cleanup"></a>9. Limpieza de recursos

Cuando ya no necesite estos recursos, puede usar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene. Reemplace "myResourceGroup" con el nombre del grupo de recursos y ejecute el siguiente comando de PowerShell:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear una WAN virtual
> * Crear un concentrador
> * Buscar y asociar un circuito con el centro
> * Asociar el circuito a un centro, o a varios
> * Conectar una red virtual a un concentrador
> * Visualizar la instancia de Virtual WAN
> * Visualizar el estado de los recursos
> * Supervisar una conexión

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).