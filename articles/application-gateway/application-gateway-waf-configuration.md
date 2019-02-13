---
title: Listas de exclusión y límites de tamaño de solicitud del firewall de aplicaciones web en Azure Application Gateway (Azure Portal)
description: En este artículo se proporciona información acerca de la configuración de listas de exclusión y límites de tamaño de solicitud del firewall de aplicaciones web en Application Gateway con Azure Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.workload: infrastructure-services
ms.date: 1/29/2019
ms.author: victorh
ms.openlocfilehash: 24cb5d7ddb1bfbb64796d871aa79109dfdfa94be
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492936"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Listas de exclusión y límites de tamaño de solicitud del firewall de aplicaciones web

El firewall de aplicaciones web (WAF) de Azure Application Gateway proporciona protección a las aplicaciones web. En este artículo se describe la configuración de listas de exclusión y límites de tamaño de solicitud del WAF.

## <a name="waf-request-size-limits"></a>Límites de tamaño de solicitud del WAF

![Límites de tamaño de solicitud](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

El firewall de aplicaciones web le permite configurar límites de tamaño de solicitud dentro de los límites inferior y superior. Hay disponibles dos configuraciones de límites de tamaño:

- El campo de tamaño máximo del cuerpo de la solicitud se especifica en KB y controla el límite de tamaño de la solicitud general, excluyendo cualquier carga de archivo. Este campo puede oscilar entre 1 KB como mínimo y 128 KB como máximo. El valor predeterminado para el tamaño del cuerpo de la solicitud es de 128 KB.
- El campo de límite de carga de archivo se especifica en MB y controla el tamaño máximo de carga de archivo permitido. Este campo puede tener un valor mínimo de 1 MB y un máximo de 500 MB para las instancias de SKU de grandes, mientras que las SKU medianas tiene un máximo de 100 MB. El valor predeterminado del límite de carga de archivo es 100 MB.

El WAF también ofrece un botón configurable para activar o desactivar la inspección del cuerpo de la solicitud. De forma predeterminada, la inspección del cuerpo de la solicitud está habilitada. Si se desactiva la inspección del cuerpo de la solicitud, el WAF no evalúa el contenido del cuerpo del mensaje HTTP. En tal caso, el WAF continúa aplicando reglas de WAF en URI, cookies y encabezados. Si se desactiva la inspección del cuerpo de la solicitud, el campo de tamaño máximo del cuerpo de la solicitud no es aplicable y no se puede definir. Al desactivar la inspección del cuerpo de la solicitud, se podrán enviar mensajes de más de 128 KB al WAF, pero no se inspecciona el cuerpo de mensaje en busca de vulnerabilidades.

## <a name="waf-exclusion-lists"></a>Listas de exclusión del WAF

![waf-exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

Las listas de exclusión del WAF le permiten omitir determinados atributos de solicitud de una evaluación del WAF. Un ejemplo común son los tokens insertados de Active Directory que se usan para campos de contraseña o autenticación. Estos atributos suelen contener caracteres especiales que podrían desencadenar un falso positivo de las reglas de WAF. Una vez que se agrega un atributo a la lista de exclusión del WAF, ninguna regla de WAF configurada y activa lo toma en consideración. Las listas de exclusión tienen un ámbito global.

Los siguientes atributos se pueden agregar a las listas de exclusión:

* Encabezados de solicitud
* Cookies de solicitud
* Cuerpo de la solicitud

   * Datos de varias partes del formulario
   * XML
   * JSON

Puede especificar una coincidencia exacta con un atributo de cadena de consulta, una cookie, el cuerpo o el encabezado de una solicitud.  Si lo desea, también puede especificar coincidencias parciales. La exclusión siempre se aplica al campo del encabezado; nunca a su valor. Las reglas de exclusión tienen un ámbito global y se aplican a todas las páginas y todas las reglas.

Estos son los operadores de criterios de coincidencia admitidos:

- **Equals** (es igual a):  este operador se usa para una coincidencia exacta. Por ejemplo, para seleccionar el encabezado denominado **bearerToken**, utilice el operador de igualdad con el selector definido como **bearerToken**.
- **Starts with** (empieza por): este operador coincide con todos los campos que comienzan por el valor del selector especificado.
- **Ends with** (termina en):  este operador coincide con todos los campos de solicitud que terminan con el valor del selector especificado.
- **Contains** (contiene): este operador coincide con todos los campos de solicitud que contienen el valor del selector especificado.

En todos los casos, la coincidencia distingue mayúsculas de minúsculas y no se permiten expresiones regulares como selectores.

### <a name="examples"></a>Ejemplos

El siguiente fragmento de código de Azure PowerShell muestra el uso de exclusiones:

```azurepowershell
// exclusion 1: exclude request head start with xyz
// exclusion 2: exclude request args equals a

$exclusion1 = New-AzureRmApplicationGatewayFirewallExclusionConfig -MatchVariable "RequestHeaderNames" -SelectorMatchOperator "StartsWith" -Selector "xyz"

$exclusion2 = New-AzureRmApplicationGatewayFirewallExclusionConfig -MatchVariable "RequestArgNames" -SelectorMatchOperator "Equals" -Selector "a"

// add exclusion lists to the firewall config

$firewallConfig = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention -RuleSetType "OWASP" -RuleSetVersion "2.2.9" -DisabledRuleGroups $disabledRuleGroup1,$disabledRuleGroup2 -RequestBodyCheck $true -MaxRequestBodySizeInKb 80 -FileUploadLimitInMb 70 -Exclusions $exclusion1,$exclusion2
```

El siguiente fragmento de código JSON muestra el uso de exclusiones:

```json
"webApplicationFirewallConfiguration": {
          "enabled": "[parameters('wafEnabled')]",
          "firewallMode": "[parameters('wafMode')]",
          "ruleSetType": "[parameters('wafRuleSetType')]",
          "ruleSetVersion": "[parameters('wafRuleSetVersion')]",
          "disabledRuleGroups": [],
          "exclusions": [
            {
                "matchVariable": "RequestArgNames",
                "selectorMatchOperator": "StartsWith",
                "selector": "a^bc"
            }
```

## <a name="next-steps"></a>Pasos siguientes

Después de configurar el WAF, puede aprender a ver los registros del WAF. Para más información, consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).
