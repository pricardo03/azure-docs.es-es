---
title: Reglas personalizadas de Firewall de aplicaciones web (WAF) de Azure
description: Este artículo proporciona una introducción a las reglas personalizadas del firewall de aplicaciones web (WAF) de Azure Application Gateway.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: d35e1dc96f65e51ab14c4962d1824334cbdb1616
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66752034"
---
# <a name="custom-rules-for-web-application-firewall"></a>Reglas personalizadas para el firewall de aplicaciones web

El firewall de aplicaciones web (WAF) de Azure Application Gateway viene con un conjunto de reglas configuradas previamente y administradas por la plataforma que ofrece protección frente a muchos tipos diferentes de ataques. Estos ataques incluyen scripts entre sitios, inyección de código SQL y otros. Si es un administrador de WAF, puede escribir reglas propias para aumentar las del conjunto de reglas básico (CRS). Las reglas pueden bloquear o permitir el tráfico solicitado en función de criterios de coincidencia.

Las reglas personalizadas permiten crear reglas propias que se evalúan en cada solicitud que pasa por el WAF. Estas reglas tienen una prioridad mayor que el resto de las reglas de los conjuntos de reglas administrados. Las reglas personalizadas contienen un nombre de regla, la prioridad de la regla y una matriz de condiciones de coincidencia. Si estas condiciones se cumplen, se realiza una acción (para permitir o bloquear).

Por ejemplo, puede bloquear todas las solicitudes de una dirección IP en el intervalo 192.168.5.4/24. En esta regla, el operador es *IPMatch*, matchValues es el intervalo de direcciones IP (192.168.5.4/24) y la acción es bloquear el tráfico. También se establecen el nombre y la prioridad de la regla.

Las reglas personalizadas admiten el uso de lógica de composición para crear reglas más avanzadas que solucionen los requisitos de seguridad. Por ejemplo, (Condición 1 **y** Condición 2) **o** Condición 3).  En este ejemplo significa que si se cumplen Condición 1 **y** Condición 2, **o** si se cumple Condición 3, el WAF debe realizar la acción especificada en la regla personalizada.

En una misma regla, las condiciones de coincidencia diferentes siempre se combinan mediante **y**. Por ejemplo, bloquear el tráfico desde una dirección IP específica, y solo si se usa un explorador determinado.

Si quiere aplicar **o** a dos condiciones diferentes, las dos deben estar en reglas distintas. Por ejemplo, bloquear el tráfico desde una dirección IP específica o bloquear el tráfico si se usa un explorador determinado.

> [!NOTE]
> El número máximo de reglas personalizadas de WAF es 100. Para más información sobre los límites de Application Gateway, vea [Suscripción de Azure y límites de servicio, cuotas y restricciones](../azure-subscription-service-limits.md#application-gateway-limits).

En las reglas personalizadas también se admiten expresiones regulares, como en los conjuntos de reglas CRS. Para obtener ejemplos, vea los ejemplos 3 y 5 en [Creación y uso de reglas personalizadas del firewall de aplicaciones web](create-custom-waf-rules.md).

## <a name="allowing-vs-blocking"></a>Diferencias entre permitir y bloquear

Permitir y bloquear el tráfico es sencillo con las reglas personalizadas. Por ejemplo, puede bloquear todo el tráfico procedente de un intervalo de direcciones IP. Puede crear otra regla para permitir el tráfico si la solicitud proviene de un explorador específico.

Para permitir algo, asegúrese de que el parámetro `-Action` esté establecido en **Allow**. Para bloquear algo, asegúrese de que el parámetro `-Action` esté establecido en **Block**.

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

La regla `$BlockRule` anterior se asigna a la siguiente regla personalizada en Azure Resource Manager:

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

Esta regla personalizada contiene un nombre, una prioridad, una acción y la matriz de condiciones de coincidencia que se deben cumplir para que la acción tenga lugar. Para obtener una explicación de estos campos, vea las descripciones siguientes. Para obtener ejemplos de reglas personalizadas, vea [Creación y uso de reglas personalizadas del firewall de aplicaciones web](create-custom-waf-rules.md).

## <a name="fields-for-custom-rules"></a>Campos para reglas personalizadas

### <a name="name-optional"></a>Nombre [opcional]

Es el nombre de la regla. Este nombre aparece en los registros.

### <a name="priority-required"></a>Prioridad [obligatorio]

- Determina el orden de evaluación de las reglas. Cuanto menor sea el valor, antes se evalúa la regla.
-Debe ser único entre todas las reglas personalizadas. Una regla con prioridad de 100 se evalúa antes que una regla con prioridad de 200.

### <a name="rule-type-required"></a>Tipo de regla [obligatorio]

En la actualidad, debe ser **MatchRule**.

### <a name="match-variable-required"></a>Variable de coincidencia [obligatorio]

Debe ser una de estas variables:

- RemoteAddr: dirección IP o nombre de host de la conexión del equipo remoto
- RequestMethod: método de solicitud HTTP (GET, POST, PUT, DELETE y así sucesivamente).
- QueryString: la variable en el URI
- PostArgs: los argumentos enviados en el cuerpo de POST
- RequestUri: URI de la solicitud
- RequestHeaders: los encabezados de la solicitud
- RequestBody: el cuerpo de la solicitud
- RequestCookies: las cookies de la solicitud

### <a name="selector-optional"></a>Selector [opcional]

Describe el campo de la colección matchVariable. Por ejemplo, si matchVariable es RequestHeaders, el selector podría estar en el encabezado *User-Agent*.

### <a name="operator-required"></a>Operador [obligatorio]

Debe ser uno de los operadores siguientes:

- IPMatch: solo se usa cuando la variable de coincidencia es *RemoteAddr*
- Equals: la entrada es la misma que MatchValue
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- Regex

### <a name="negate-condition-optional"></a>Negar condición [opcional]

Niega la condición actual.

### <a name="transform-optional"></a>Transformación [opcional]

Una lista de cadenas con los nombres de las transformaciones que se van a realizar antes de la coincidencia. Pueden ser las siguientes:

- Minúsculas
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Valores de coincidencia [obligatorio]

Lista de valores con los que se va a comparar, que se pueden considerar como combinados mediante *OR*. Por ejemplo, podrían ser direcciones IP u otras cadenas. El formato del valor depende del operador anterior.

### <a name="action-required"></a>Acción [obligatorio]

- Allow: autoriza la transacción y se omiten todas las reglas siguientes. Esto significa que la solicitud especificada se agrega a la lista de permitidos y una vez que coincide, la solicitud detiene la evaluación adicional y se envía al grupo de back-end. En las reglas que están en la lista de permitidos no se evalúan más reglas administradas ni personalizadas.
- Block: bloquea la transacción en función de *SecDefaultAction* (modo de detección o prevención). Como sucede con la acción Allow, una vez que la solicitud se evalúa y se agrega a la lista de bloqueadas, la evaluación se detiene y la solicitud se bloquea. Las solicitudes posteriores que cumplan las mismas condiciones no se evaluarán y simplemente se bloquearán. 
- Log: permite que la regla escriba en el registro, pero que las reglas restantes se ejecuten para la evaluación. Las reglas personalizadas siguientes se evalúan en orden de prioridad, seguidas por las reglas administradas.

## <a name="next-steps"></a>Pasos siguientes

Una vez que obtenga información sobre las reglas personalizadas, [cree reglas personalizadas propias](create-custom-waf-rules.md).
