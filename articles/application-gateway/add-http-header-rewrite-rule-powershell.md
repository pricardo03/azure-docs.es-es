---
title: Reescribir encabezados HTTP en Azure Application Gateway
description: En este artículo se proporciona información sobre cómo volver a escribir los encabezados HTTP en Azure Application Gateway mediante Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: 47fe6a5247622e3ad3b3720955068580e0329913
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64947204"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>Vuelva a escribir los encabezados de solicitud y respuesta HTTP con Azure Application Gateway mediante PowerShell de Azure

En este artículo se describe cómo usar Azure PowerShell para configurar un [SKU de puerta de enlace de aplicaciones v2](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) instancia volver a escribir los encabezados HTTP en las solicitudes y respuestas.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

- Deberá ejecutar PowerShell de Azure localmente para completar los pasos descritos en este artículo. También debe tener Az versión 1.0.0 del módulo o una versión posterior instalada. Ejecute `Import-Module Az` y, a continuación, `Get-Module Az` para determinar la versión que ha instalado. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps). Después de verificar la versión de PowerShell, ejecute `Login-AzAccount` para crear una conexión con Azure.
- Debe tener una instancia SKU de puerta de enlace de aplicaciones v2. No se admite la reescritura de encabezados en la SKU v1. Si no tiene la SKU de v2, cree un [SKU de puerta de enlace de aplicaciones v2](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) instancia antes de comenzar.

## <a name="create-required-objects"></a>Crear los objetos necesarios

Para configurar la reescritura del encabezado HTTP, es preciso completar estos pasos.

1. Crear los objetos que son necesarios para la reescritura del encabezado HTTP:

   - **RequestHeaderConfiguration**: Se usa para especificar los campos de encabezado de solicitud que va a escribir y el nuevo valor para los encabezados.

   - **ResponseHeaderConfiguration**: Se usa para especificar los campos de encabezado de respuesta que se va a escribir y el nuevo valor para los encabezados.

   - **ActionSet**: Contiene las configuraciones de los encabezados de solicitud y respuesta especificados anteriormente.

   - **Condición**: Una configuración opcional. Condiciones de reescritura evalúan el contenido de las solicitudes HTTP (S) y las respuestas. Si la solicitud HTTP (S) o la respuesta coincide con la condición de reescritura, se producirá la acción de reescritura.

     Si más de una condición se asocia con una acción, se produce la acción solo cuando se cumplen todas las condiciones. En otras palabras, la operación es una operación AND lógica.

   - **RewriteRule**: Contiene varias acciones de reescritura o vuelva a escribir las combinaciones de condición.

   - **RuleSequence**: Ejecutar una configuración opcional que ayuda a determinar el orden en que las reglas de reescritura. Esta configuración es útil cuando haya varias reglas de reescritura en un conjunto de reescritura. Una regla de reescritura que tiene un valor de secuencia más bajo de regla se ejecuta primera. Si asigna el mismo valor de secuencia de la regla a dos reglas de reescritura, el orden de ejecución es no determinista.

     Si no se especifica explícitamente el RuleSequence, se establece un valor predeterminado de 100.

   - **RewriteRuleSet**: Contiene varias reglas de reescritura que se van a asociadas a una regla de enrutamiento de solicitud.

2. Adjunte el RewriteRuleSet a una regla de enrutamiento. La configuración de reescritura se adjunta al agente de escucha de origen a través de la regla de enrutamiento. Cuando se usa una regla de enrutamiento básica, la configuración de reescritura de encabezado está asociada con un agente de escucha de origen y es una reescritura encabezado global. Cuando se usa una regla de enrutamiento basada en ruta de acceso, la configuración de reescritura de encabezado se define en el mapa de ruta de acceso de dirección URL. En ese caso, se aplica solo al área de ruta de acceso específica de un sitio.

Puede crear varios conjuntos de reescritura de encabezado HTTP y aplicar cada reescritura establecido en varios agentes de escucha. Pero puede aplicar sólo una reescritura establecido en un agente de escucha concreto.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-the-http-header-rewrite-rule-configuration"></a>Especificar el encabezado HTTP vuelva a escribir la configuración de la regla

En este ejemplo, se modificará una dirección URL de redirección al volver a escribir el encabezado de ubicación en la respuesta HTTP siempre que el encabezado de ubicación contenga una referencia a azurewebsites.net. Para ello, vamos a agregar una condición para evaluar si el encabezado de ubicación en la respuesta contiene azurewebsites.net. Vamos a usar el patrón `(https?):\/\/.*azurewebsites\.net(.*)$`. Y vamos a usar `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` como el valor del encabezado. Este valor se reemplazará *azurewebsites.net* con *contoso.com* en el encabezado location.

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-the-configuration-of-your-application-gateway"></a>Recuperar la configuración de la puerta de enlace de aplicaciones

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-the-configuration-of-your-request-routing-rule"></a>Recuperar la configuración de la regla de enrutamiento de solicitud

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Actualizar la puerta de enlace de aplicaciones con la configuración de reescritura de encabezados HTTP

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name LocationHeaderRewrite -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Eliminación de una regla de reescritura

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "LocationHeaderRewrite" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de cómo configurar algunos casos de uso comunes, vea [encabezado común escribir escenarios](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).