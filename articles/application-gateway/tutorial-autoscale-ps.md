---
title: 'Creación de una puerta de enlace de aplicaciones con redundancia de zona, escalabilidad automática y una dirección IP reservada: Azure PowerShell'
description: Aprenda a crear una puerta de enlace de aplicaciones con redundancia de zona, escalabilidad automática y una dirección IP reservada con Azure PowerShell.
services: application-gateway
author: amitsriva
ms.service: application-gateway
ms.topic: tutorial
ms.date: 9/26/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: d86ce2e1bac2fb58df8df748381a00eac21e65cb
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48016941"
---
# <a name="tutorial-create-an-autoscaling-zone-redundant-application-gateway-with-a-reserved-virtual-ip-address-using-azure-powershell"></a>Tutorial: creación de una puerta de enlace de aplicaciones con redundancia de zona, escalabilidad automática y una dirección IP virtual reservada con Azure PowerShell

En este tutorial se describe la creación de una puerta de enlace de aplicaciones de Azure con cmdlets de Azure PowerShell y el modelo de implementación de Azure Resource Manager. Este tutorial se centra en las diferencias entre la nueva SKU con escalabilidad automática y la SKU estándar existente. En concreto, en las características para admitir la escalabilidad automática, la redundancia de zona y las direcciones IP virtuales reservadas (dirección IP estática).

Para más información sobre la escalabilidad automática y la redundancia de zona de la puerta de enlace de aplicaciones, consulte [Autoscaling and Zone-redundant Application Gateway (Public Preview)](application-gateway-autoscaling-zone-redundant.md) [Puerta de enlace de aplicaciones con redundancia de zona y escalabilidad automática (versión preliminar pública)].

> [!IMPORTANT]
> La SKU de escalabilidad automática y de puerta de enlace de aplicaciones con redundancia de zona actualmente está en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar el parámetro de escalabilidad automática
> * Usar el parámetro de zona
> * Usar una dirección IP virtual estática
> * Creación de la puerta de enlace de aplicaciones


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

En este tutorial es necesario ejecutar Azure PowerShell en local. Debe tener instalado el módulo Azure PowerShell, versión 6.9.0 o posterior. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Después de verificar la versión de PowerShell, ejecute `Login-AzureRmAccount` para crear una conexión con Azure.

## <a name="sign-in-to-your-azure-account"></a>Inicio de sesión en la cuenta de Azure.

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

## <a name="create-a-vnet"></a>Creación de una red virtual
Cree una red virtual con una subred específica para una puerta de enlace de aplicaciones con escalabilidad automática. Actualmente solo se puede implementar una puerta de enlace de aplicaciones con escalabilidad automática en cada subred específica.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzureRmVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Creación de una dirección IP pública reservada

Especifique el método de asignación de PublicIPAddress como **Static**. La dirección IP virtual de la puerta de enlace de aplicaciones con escalabilidad automática solo puede ser estática. No se admiten direcciones IP dinámicas. Se admite solo la SKU estándar PublicIpAddress.

```azurepowershell
#Create static public IP
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Recuperación de los detalles

Recupere los detalles del grupo de recursos, la subred y la dirección IP en un objeto local para crear los detalles de configuración de IP de la puerta de enlace de aplicaciones.

```azurepowershell
$resourceGroup = Get-AzureRmResourceGroup -Name $rg
$publicip = Get-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```
## <a name="configure-application-gateway-infrastructure"></a>Configuración de la infraestructura de la puerta de enlace de aplicaciones
Configure la dirección IP, la dirección IP de front-end, el grupo de servidores back-end, la configuración http, el certificado, el puerto, el agente de escucha y la regla en el mismo formato que los de la puerta de enlace de aplicaciones estándar existente. La nueva SKU sigue el mismo modelo de objeto que la SKU estándar.

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

Ahora puede especificar la configuración de la escalabilidad automática para la puerta de enlace de aplicaciones. Admite dos tipos de configuración:

- **Modo de capacidad fija**. En este modo, la puerta de enlace de aplicaciones no escala automáticamente y funciona con una unidad de escalado fija.

   ```azurepowershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2
   ```
- **Modo de escalabilidad automática**. En este modo, la puerta de enlace de aplicaciones escala automáticamente en función del patrón de tráfico de las aplicaciones.

   ```azurepowershell
   $autoscaleConfig = New-AzureRmApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>Creación de la puerta de enlace de aplicaciones

Cree la puerta de enlace de aplicaciones e incluya zonas de redundancia. 

La configuración de zona solo se admite en las regiones donde estén disponibles las zonas de Azure. En las regiones donde no sea el caso no debe usarse el parámetro de zona. También se puede implementar una puerta de enlace de aplicaciones en una única zona, en dos o en las tres. El elemento PublicIPAddress para una puerta de enlace de aplicaciones de una sola zona debe estar enlazada a la misma zona. En el caso de puertas de enlace de aplicaciones con redundancia en dos o tres zonas, PublicIPAddress debe tener también redundancia de zona, por lo que no tendrá una zona específica.

```azurepowershell
$appgw = New-AzureRmApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 `
  -ResourceGroupName $rg -Location $location -BackendAddressPools $pool `
  -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig `
  -FrontendIpConfigurations $fip -FrontendPorts $fp01, $fp02 `
  -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 `
  -Sku $sku -sslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

Use [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) para obtener la dirección IP pública de la puerta de enlace de aplicaciones. Copie la dirección IP pública o el nombre DNS y péguelo en la barra de direcciones del explorador.

`Get-AzureRmPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP`

## <a name="clean-up-resources"></a>Limpieza de recursos
Explore los recursos que se han creado con la puerta de enlace de aplicaciones y, cuando ya no se necesiten, puede usar el comando `Remove-AzureRmResourceGroup` para quitar el grupo de recursos, la puerta de enlace de aplicaciones y todos los recursos relacionados.

`Remove-AzureRmResourceGroup -Name $rg`

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Usar una dirección IP virtual estática
> * Configurar el parámetro de escalabilidad automática
> * Usar el parámetro de zona
> * Creación de la puerta de enlace de aplicaciones

> [!div class="nextstepaction"]
> [Crear una puerta de enlace de aplicaciones con reglas de enrutamiento basadas en rutas de direcciones URL](./tutorial-url-route-powershell.md)
