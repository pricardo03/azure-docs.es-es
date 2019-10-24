---
title: Configuración de reglas personalizadas del firewall de aplicaciones web v2 con Azure PowerShell
description: Aprenda a configurar reglas personalizadas del firewall de aplicaciones web v2 con Azure PowerShell.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 9e50b47e22f5760c213cd0cafad82ecca592dec8
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72263750"
---
# <a name="configure-web-application-firewall-v2-custom-rules-by-using-azure-powershell"></a>Configuración de reglas personalizadas del firewall de aplicaciones web v2 con Azure PowerShell

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

Con las reglas personalizadas, puede crear reglas propias que se evalúan en cada solicitud que pasa por el firewall de aplicaciones web (WAF). Estas reglas tienen una prioridad mayor que el resto de las reglas de los conjuntos de reglas administrados. Para permitir una personalización completa, las reglas personalizadas incluyen una acción (para permitir o bloquear), una condición de coincidencia y un operador.

En este artículo se crea un WAF v2 de Azure Application Gateway que usa una regla personalizada. La regla personalizada bloqueará el tráfico si el encabezado de la solicitud contiene User-Agent *evilbot*.

Para ver más ejemplos de reglas personalizadas, consulte [Creación y uso de reglas personalizadas del firewall de aplicaciones web](create-custom-waf-rules.md).

Para ejecutar el código de Azure PowerShell de este artículo en un script continuo que puede copiar, pegar y ejecutar, consulte [Ejemplos de Azure PowerShell con Azure Application Gateway](powershell-samples.md).

## <a name="prerequisites"></a>Requisitos previos
* [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

* Necesita un módulo de Azure PowerShell. Si decide instalar y usar Azure PowerShell localmente, este script requiere la versión 2.1.0 del módulo de Azure PowerShell o cualquier versión posterior. Haga lo siguiente:

  1. Para encontrar la versión, ejecute `Get-Module -ListAvailable Az`. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps).
  2. Ejecute `Connect-AzAccount` para crear una conexión con Azure.

## <a name="example-script"></a>Script de ejemplo

### <a name="set-up-variables"></a>Configurar las variables

Ejecute el código siguiente:

```azurepowershell
$rgname = "CustomRulesTest"

$location = "East US"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Ejecute el código siguiente:

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-virtual-network"></a>Creación de una red virtual

Ejecute el código siguiente:

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>Creación de una IP pública estática

Ejecute el código siguiente:

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-a-pool-and-front-end-port"></a>Creación de un grupo y puertos de front-end

Ejecute el código siguiente:

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>Creación de un agente de escucha, una configuración de HTTP, una regla y escalabilidad automática

Ejecute el código siguiente:

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

### <a name="create-the-custom-rule-and-apply-it-to-waf-policy"></a>Creación de la regla personalizada y aplicación a la directiva del WAF

Ejecute el código siguiente:

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

Ejecute el código siguiente:

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

[Más información sobre el firewall de aplicaciones web](waf-overview.md)
