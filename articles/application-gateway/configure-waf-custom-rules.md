---
title: Configurar reglas de Firewall de aplicaciones Web personalizadas con Azure PowerShell
description: Obtenga información sobre cómo configurar reglas de WAF personalizadas con Azure PowerShell
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/7/2019
ms.author: victorh
ms.openlocfilehash: f7215c4f35d36486b8dda483f34bc487cc16fc69
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743057"
---
# <a name="configure-web-application-firewall-with-a-custom-rule-using-azure-powershell"></a>Configurar Firewall de aplicaciones Web con una regla personalizada mediante Azure PowerShell

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

Reglas personalizadas permiten crear sus propias reglas que se evalúa para cada solicitud que se pasa a través de Firewall de aplicaciones Web (WAF). Estas reglas mantienen una prioridad mayor que el resto de las reglas en los conjuntos de reglas administrado. Las reglas personalizadas tienen una acción (para permitir o bloquear), una condición de coincidencia y un operador para permitir la personalización completa.

En este artículo se crea un WAF de Application Gateway que usa una regla personalizada. Si el encabezado de solicitud contiene un agente de usuario de tráfico de los bloques de regla personalizada *evilbot*.

Para ver más ejemplos de reglas personalizado, consulte [creación y uso de reglas de firewall de aplicación web personalizada](create-custom-waf-rules.md)

Si quiere ejecutar Azure PowerShell en este artículo en una secuencia de comandos continua que puede copiar, pegar y ejecutar, consulte [ejemplos de PowerShell de Azure Application Gateway](powershell-samples.md).

## <a name="prerequisites"></a>Requisitos previos

### <a name="azure-powershell-module"></a>Módulo de Azure PowerShell

Si decide instalar y usar PowerShell de Azure localmente, este script requiere la versión 2.1.0 de módulo Azure PowerShell o una versión posterior.

1. Para encontrar la versión, ejecute `Get-Module -ListAvailable Az`. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps).
2. Ejecute `Connect-AzAccount` para crear una conexión con Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="example-script"></a>Script de ejemplo

### <a name="set-up-variables"></a>Configurar las variables

```azurepowershell
$rgname = "CustomRulesTest"

$location = "East US"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-vnet"></a>Creación de una red virtual

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>Crear a una VIP pública estática

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-pool-and-frontend-port"></a>Crear el puerto de front-end y de grupo

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>Crear un agente de escucha, configuración de http, una regla y escalado automático

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled

$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3

$sku = New-AzApplicationGatewaySku -Name WAF_v2 -Tier WAF_v2
```

### <a name="create-the-custom-rule-and-apply-it-to-waf-policy"></a>Crear la regla personalizada y aplicarlo a la directiva de WAF

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-the-application-gateway"></a>Creación de Application Gateway

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $rgname `
   -Location $location -BackendAddressPools $pool `
   -BackendHttpSettingsCollection  $poolSetting01 `
   -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
   -FrontendPorts $fp01 -HttpListeners $listener01 `
   -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig `
   -WebApplicationFirewallConfig $wafConfig `
   -FirewallPolicy $wafPolicy
```

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre Firewall de aplicaciones Web](waf-overview.md)