---
title: 'Tutorial: Creación de una puerta de enlace de aplicaciones con redundancia de zona, escalabilidad automática y una dirección IP reservada: Azure PowerShell'
description: En este tutorial, obtendrá información sobre cómo crear una puerta de enlace de aplicaciones con redundancia de zona, escalabilidad automática y una dirección IP reservada con Azure PowerShell.
services: application-gateway
author: amitsriva
ms.service: application-gateway
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 99fa5d6f0ba74b56a53f2d1af1b99c7e5c2896a7
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323207"
---
# <a name="tutorial-create-an-application-gateway-that-improves-web-application-access"></a>Tutorial: Creación de una puerta de enlace de aplicaciones que mejora el acceso a aplicaciones web

Si es un administrador de TI interesado en mejorar el acceso a las aplicaciones web, puede optimizar el escalado de Application Gateway en función de la demanda de los clientes y abarcar varias zonas de disponibilidad. Para ello, este tutorial le ayudará a configurar las características de Azure Application Gateway: escalabilidad automática, redundancia de zona y direcciones IP virtuales (IP estática). Use los cmdlets de Azure PowerShell y el modelo de implementación de Azure Resource Manager para solucionar el problema.

> [!IMPORTANT] 
> La SKU de escalabilidad automática y de puerta de enlace de aplicaciones con redundancia de zona actualmente está en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una red virtual de escalabilidad automática
> * Creación de una dirección IP pública reservada
> * Configurar una infraestructura para Application Gateway
> * Especificación de la escalabilidad automática
> * Creación de la puerta de enlace de aplicaciones
> * Prueba de la puerta de enlace de aplicaciones

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial es necesario ejecutar Azure PowerShell en local. Debe tener instalado el módulo Azure PowerShell, versión 6.9.0 o posterior. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Después de verificar la versión de PowerShell, ejecute `Login-AzureRmAccount` para crear una conexión con Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -Subscription "<sub name>"
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Cree un grupo de recursos en una de las ubicaciones disponibles.

```azurepowershell
$location = "East US 2"
$rg = "<rg name>"

#Create a new Resource Group
New-AzureRmResourceGroup -Name $rg -Location $location
```

## <a name="create-a-virtual-network"></a>Creación de una red virtual

Cree una red virtual con una subred específica para una instancia de Application Gateway con escalabilidad automática. Actualmente solo se puede implementar una puerta de enlace de aplicaciones con escalabilidad automática en cada subred específica.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzureRmVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Creación de una dirección IP pública reservada

Especifique el método de asignación de PublicIPAddress como **Estático**. La dirección IP virtual de la puerta de enlace de aplicaciones con escalabilidad automática solo puede ser estática. No se admiten direcciones IP dinámicas. Se admite solo la SKU estándar PublicIpAddress.

```azurepowershell
#Create static public IP
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Recuperación de los detalles

Recupere los detalles del grupo de recursos, la subred y la dirección IP en un objeto local para crear los detalles de configuración de IP de Application Gateway.

```azurepowershell
$resourceGroup = Get-AzureRmResourceGroup -Name $rg
$publicip = Get-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```

## <a name="configure-the-infrastructure"></a>Configuración de la infraestructura

Configure la dirección IP, la dirección IP de front-end, el grupo de servidores back-end, la configuración HTTP, el certificado, el puerto, el agente de escucha y la regla con un formato idéntico al de la instancia estándar de Application Gateway. La nueva SKU sigue el mismo modelo de objeto que la SKU estándar.

```azurepowershell
$ipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzureRmApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "SSLPort" -Port 443
$fp02 = New-AzureRmApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$securepfxpwd = ConvertTo-SecureString -String "scrap" -AsPlainText -Force
$sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -Password $securepfxpwd `
            -CertificateFile "D:\Networking\ApplicationGateway\scrap.pfx"
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "SSLListener" `
             -Protocol Https -FrontendIPConfiguration $fip -FrontendPort $fp01 -SslCertificate $sslCert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp02

$setting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule2" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener02 -BackendAddressPool $pool
```

## <a name="specify-autoscale"></a>Especificación de la escalabilidad automática

Ahora puede especificar la configuración de la escalabilidad automática para Application Gateway. Admite dos tipos de configuración:

* **Modo de capacidad fija**. En este modo, la puerta de enlace de aplicaciones no escala automáticamente y funciona con una unidad de escalado fija.

   ```azurepowershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2
   ```

* **Modo de escalabilidad automática**. En este modo, la puerta de enlace de aplicaciones escala automáticamente en función del patrón de tráfico de las aplicaciones.

   ```azurepowershell
   $autoscaleConfig = New-AzureRmApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>Creación de la puerta de enlace de aplicaciones

Cree la instancia de Application Gateway e incluya las zonas de redundancia y la configuración de escalabilidad automática.

```azurepowershell
$appgw = New-AzureRmApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 `
  -ResourceGroupName $rg -Location $location -BackendAddressPools $pool `
  -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig `
  -FrontendIpConfigurations $fip -FrontendPorts $fp01, $fp02 `
  -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 `
  -Sku $sku -sslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

Use Get-AzureRmPublicIPAddress para obtener la dirección IP pública de Application Gateway. Copie la dirección IP pública o el nombre DNS y péguelo en la barra de direcciones del explorador.

`Get-AzureRmPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP`

## <a name="clean-up-resources"></a>Limpieza de recursos

En primer lugar, explore los recursos que se crearon con Application Gateway. Después, cuando ya no se necesiten, puede usar el comando `Remove-AzureRmResourceGroup` para quitar el grupo de recursos, la instancia de Application Gateway y todos los recursos relacionados.

`Remove-AzureRmResourceGroup -Name $rg`

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear una puerta de enlace de aplicaciones con reglas de enrutamiento basadas en rutas de direcciones URL](./tutorial-url-route-powershell.md)
