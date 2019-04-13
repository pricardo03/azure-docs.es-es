---
title: Reescribir encabezados HTTP en Azure Application Gateway
description: En este artículo proporciona información sobre cómo volver a escribir los encabezados HTTP en Azure Application Gateway con Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: bfafc74cbcb97f28cc085196a2cbaf4e9bf2e871
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548326"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>Vuelva a escribir los encabezados de solicitud y respuesta HTTP con Azure Application Gateway mediante PowerShell de Azure

En este artículo se muestra cómo usar Azure PowerShell para configurar un [SKU de puerta de enlace de aplicaciones v2](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) volver a escribir los encabezados HTTP en las solicitudes y respuestas.

> [!IMPORTANT]
> La SKU de escalabilidad automática y de puerta de enlace de aplicaciones con redundancia de zona actualmente está en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

- En este tutorial es necesario ejecutar Azure PowerShell en local. Necesita tener instalado el módulo de Azure versión 1.0.0 o posterior. Ejecute `Import-Module Az` y, después, `Get-Module Az` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps). Después de verificar la versión de PowerShell, ejecute `Login-AzAccount` para crear una conexión con Azure.
- Debe tener un v2 Application Gateway SKU, puesto que el encabezado de la capacidad de reescritura no se admite para la SKU v1. Si no tiene la SKU de v2, cree un [SKU de puerta de enlace de aplicaciones v2](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps>) antes de comenzar.

## <a name="what-is-required-to-rewrite-a-header"></a>¿Qué es necesario volver a escribir un encabezado

Para configurar la reescritura del encabezado HTTP, deberá:

1. Cree los objetos necesarios para reescribir los encabezados HTTP:

   - **RequestHeaderConfiguration**: este objeto se usa para especificar los campos del encabezado de solicitud que va a reescribir y el nuevo valor con el que se reescribirán los encabezados originales.

   - **ResponseHeaderConfiguration**: este objeto se usa para especificar los campos del encabezado de respuesta que quiera reescribir y el nuevo valor con el que se reescribirán los encabezados originales.

   - **ActionSet**: este objeto contiene las configuraciones de los encabezados de solicitud y respuesta especificados anteriormente.

   - **Condición**: Es una configuración opcional. Si se agrega una condición de reescritura, se evaluará el contenido de las respuestas y solicitudes HTTP (S). Si la solicitud HTTP (S) o la respuesta coincide con la condición de reescritura, se basará la decisión de ejecutar la acción de reescritura asociada con la condición de reescritura. 

     Si más de una de las condiciones asociadas con una acción y, a continuación, la acción se ejecutará sólo cuando se cumplen todas las condiciones, es decir, se realizará una operación AND lógica.

   - **RewriteRule**: contiene varios acción reescritura - combinaciones de condición de reescritura.

   - **RuleSequence**: Se trata de una configuración opcional. Le ayuda a determinar el orden en el que se ejecutan las reglas de reescritura diferente. Esto resulta útil cuando hay varias reglas de reescritura en un conjunto de reescritura. La regla de reescritura con el menor valor de secuencia de la regla se ejecuta en primer lugar. Si proporciona la misma secuencia de regla para dos reglas de reescritura, a continuación, el orden de ejecución será no determinista.

     Si no se especifica explícitamente el RuleSequence, se establecerá un valor predeterminado de 100.

   - **RewriteRuleSet**: este objeto contiene varias reglas de reescritura que se van a asociadas a una regla de enrutamiento de solicitud.

2. Se le pedirá para adjuntar el rewriteRuleSet con una regla de enrutamiento. Esto es porque la configuración de reescritura se ha asociado al agente de escucha de origen a través de la regla de enrutamiento. Al usar una regla de enrutamiento básica, la configuración de reescritura de encabezados se asocia a un agente de escucha de origen y es una reescritura de encabezados global. Al usar una regla de enrutamiento basada en rutas, la configuración de reescritura de encabezado se define en la asignación de la ruta de URL. Por lo tanto, solo se aplica al área específica de la ruta de acceso de un sitio.

Puede crear varios conjuntos de reescritura de encabezado de http y cada conjunto de reescritura se puede aplicar a varios agentes de escucha. Sin embargo, puede aplicar sólo una reescritura establecido en un agente de escucha concreto.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Especificación de la configuración de la regla para volver a escribir el encabezado**

En este ejemplo, se modificará la dirección URL de redirección al volver a escribir el encabezado de ubicación en la respuesta http siempre que el encabezado de ubicación contenga una referencia a "azurewebsites.net". Para ello, agregaremos una condición para evaluar si el encabezado de ubicación en la respuesta contiene azurewebsites.net con el patrón `(https?):\/\/.*azurewebsites\.net(.*)$`. Usaremos `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` como el valor del encabezado. Esta acción reemplazará *azurewebsites.net* con *contoso.com* en el encabezado location.

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Recuperación de configuración de la puerta de enlace de aplicaciones existente

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Recuperación de configuración de la regla de enrutamiento de solicitud existente

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Actualización de la puerta de enlace de aplicaciones con la configuración para volver a escribir los encabezados HTTP

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

Para obtener más información acerca de la configuración necesaria para realizar algunas comunes casos de uso, vea [encabezado común escribir escenarios](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).