---
title: Creación de una puerta de enlace de aplicaciones con redireccionamiento externo mediante Azure PowerShell | Microsoft Docs
description: Obtenga información sobre cómo crear una puerta de enlace de aplicaciones que redirija el tráfico web a un sitio externo mediante Azure PowerShell.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: victorh
ms.openlocfilehash: c8a726473a6b0f313d20dfe048cf56864c62920f
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2019
ms.locfileid: "57314184"
---
# <a name="create-an-application-gateway-with-external-redirection-using-azure-powershell"></a>Creación de una puerta de enlace de aplicaciones con redireccionamiento externo mediante Azure PowerShell

Puede usar Azure PowerShell para configurar el [redireccionamiento del tráfico web](multiple-site-overview.md) al crear una [puerta de enlace de aplicaciones](overview.md). En este tutorial, va a configurar un agente de escucha y una regla que redirigirán el tráfico web que llega a la puerta de enlace de aplicaciones de sitio externo.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Configuración de la red
> * Crear un agente de escucha y una regla de redireccionamiento
> * Creación de una puerta de enlace de aplicaciones

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar PowerShell localmente, este tutorial requiere la versión 1.0.0 del módulo de Azure PowerShell o una versión posterior. Para encontrar la versión, ejecute ` Get-Module -ListAvailable Az`. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Login-AzAccount` para crear una conexión con Azure.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Crear un grupo de recursos de Azure mediante [New AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup).  

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Crear recursos de red

Crear la configuración de subred *myAGSubnet* mediante [New AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Cree la red virtual denominada *myVNet* mediante [New AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) con la configuración de subred. Y por último, cree la dirección IP pública mediante [New AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Estos recursos se usan para proporcionar conectividad de red a la puerta de enlace de aplicaciones y sus recursos asociados.

```azurepowershell-interactive
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig
$pip = New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

### <a name="create-the-ip-configurations-and-frontend-port"></a>Creación de las configuraciones IP y el puerto de front-end

Asociar *myAGSubnet* que creó anteriormente a la puerta de enlace de aplicaciones con [New AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration). Asignar la dirección IP pública a la puerta de enlace de aplicaciones con [New AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig). Y, a continuación, puede crear el puerto HTTP mediante [New AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport).

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool-and-settings"></a>Creación de la configuración y el grupo de servidores back-end

Cree el grupo de back-end llamado *defaultPool* para la puerta de enlace de aplicaciones mediante [New AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool). Configure las opciones del grupo mediante [New AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$defaultPool = New-AzApplicationGatewayBackendAddressPool `
  -Name defaultPool 
$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-rule"></a>Creación del agente de escucha y de una regla

Se necesita un agente de escucha para habilitar la puerta de enlace de aplicaciones para enrutar el tráfico correctamente. Crear el agente de escucha mediante [New AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) con la configuración de front-end y puerto de front-end que creó anteriormente. Se necesita una regla para que el agente de escucha sepa dónde enviar el tráfico entrante. Crear una regla básica llamada *redirectRule* mediante [New AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$defaultListener = New-AzApplicationGatewayHttpListener `
  -Name defaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$redirectConfig = New-AzApplicationGatewayRedirectConfiguration `
  -Name myredirect `
  -RedirectType Temporary `
  -TargetUrl "https://bing.com"
$redirectRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name redirectRule `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -RedirectConfiguration $redirectConfig
```

### <a name="create-the-application-gateway"></a>Creación de la puerta de enlace de aplicaciones

Ahora que ha creado los recursos auxiliares necesarios, especifique los parámetros para la puerta de enlace de aplicación denominado *myAppGateway* mediante [New AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku)y, a continuación, crear mediante [Nuevo AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway).

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
$appgw = New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $redirectRule `
  -RedirectConfigurations $redirectConfig `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

Puede usar [Get AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress) para obtener la dirección IP pública de la puerta de enlace de la aplicación. Copie la dirección IP pública y péguela en la barra de direcciones del explorador.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

En el explorador debe aparecer *bing.com*.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Configuración de la red
> * Crear un agente de escucha y una regla de redireccionamiento
> * Creación de una puerta de enlace de aplicaciones
