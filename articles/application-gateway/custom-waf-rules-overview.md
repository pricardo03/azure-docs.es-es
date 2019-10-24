---
title: Reglas personalizadas de Firewall de aplicaciones web (WAF) v2 de Azure
description: En este artículo se proporciona una introducción a las reglas personalizadas del firewall de aplicaciones web (WAF) v2 de Azure Application Gateway.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 8ae5c9b6b52ea13e3d0981664e8c920cc5b47a01
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72263549"
---
# <a name="overview-custom-rules-for-web-application-firewall-v2"></a>Información general: Reglas personalizadas para el firewall de aplicaciones web v2

El firewall de aplicaciones web (WAF) v2 de Azure Application Gateway viene con un conjunto de reglas preconfiguradas y administradas por la plataforma que ofrece protección frente a muchos tipos de ataques diferentes. Estos ataques incluyen scripts entre sitios, inyección de código SQL y otros. Si es un administrador de WAF, puede escribir reglas propias para aumentar las del conjunto de reglas básico. Las reglas pueden bloquear o permitir el tráfico solicitado en función de criterios de coincidencia.

Con las reglas personalizadas, puede crear reglas propias que se evalúan en cada solicitud que pasa por el WAF. Estas reglas tienen una prioridad mayor que el resto de las reglas de los conjuntos de reglas administrados. Las reglas personalizadas contienen un nombre de regla, una prioridad de la regla y una matriz de condiciones de coincidencia. Si estas condiciones se cumplen, se realiza una acción para permitir o bloquear.

Por ejemplo, puede crear una regla para bloquear todas las solicitudes de una dirección IP en el intervalo 192.168.5.4/24. En esta regla, el operador es *IPMatch*, *matchValues* es el intervalo de direcciones IP (192.168.5.4/24) y la *acción* es bloquear el tráfico. También se establecen el nombre y la prioridad de la regla.

Las reglas personalizadas admiten el uso de lógica de composición para crear reglas más avanzadas que solucionen los requisitos de seguridad. Por ejemplo, "(Condición 1 *y* Condición 2) *o* Condición 3)" significa que si se cumplen Condición 1 *y* Condición 2, *o* si se cumple Condición 3, WAF debe realizar la acción especificada en la regla personalizada.

En una misma regla, las condiciones de coincidencia diferentes siempre se combinan mediante *y*. Por ejemplo, una regla que usa *y* puede especificar que se bloquee el tráfico de una dirección IP determinada y solo si se usa un explorador concreto.

Si quiere usar *o* para dos condiciones diferentes, las dos deben estar en reglas distintas. Por ejemplo, la regla que usa *o* puede especificar que se bloquee el tráfico de una dirección IP determinada o bloquearlo si se usa un explorador concreto.

> [!NOTE]
> El número máximo de reglas personalizadas de WAF es 100. Para más información sobre los límites de Application Gateway, vea [Suscripción de Azure y límites de servicio, cuotas y restricciones](../azure-subscription-service-limits.md#application-gateway-limits).

En las reglas personalizadas también se admiten expresiones regulares, como en los conjuntos de reglas principales. Para obtener ejemplos de estas reglas, vea los ejemplos 3 y 5 en [Creación y uso de reglas personalizadas del firewall de aplicaciones web](create-custom-waf-rules.md).

## <a name="allowing-or-blocking-traffic"></a>Permitir o bloquear el tráfico

Permitir o bloquear el tráfico es sencillo con las reglas personalizadas. Por ejemplo, puede bloquear todo el tráfico procedente de un intervalo de direcciones IP. Puede crear otra regla para permitir el tráfico si la solicitud proviene de un explorador específico.

Para permitir algo, asegúrese de que el parámetro `-Action` esté establecido en **Allow**. Para bloquear algo, asegúrese de que el parámetro `-Action` esté establecido en **Block**, como se muestra en el siguiente código:

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

Esta regla personalizada contiene un nombre, una prioridad, una acción y una matriz de condiciones de coincidencia que se deben cumplir para que la acción tenga lugar. Para ver descripciones de los campos de reglas personalizadas, consulte las secciones siguientes. Para ver ejemplos de reglas personalizadas, consulte [Creación y uso de reglas personalizadas del firewall de aplicaciones web](create-custom-waf-rules.md).

## <a name="custom-rule-fields"></a>Campos de reglas personalizadas

### <a name="name-optional"></a>Nombre (opcional)

Es el nombre de la regla. El nombre aparece en los registros.

### <a name="priority-required"></a>Prioridad (obligatorio)

- La prioridad determina el orden en el que se evalúan las reglas. Cuanto menor sea el valor, antes se evalúa la regla. El intervalo permitido es de 1 a 100.
- La prioridad debe ser única entre todas las reglas personalizadas. Una regla con una prioridad de 40 se evalúa antes que una regla con prioridad de 80.

### <a name="rule-type-required"></a>Tipo de regla (obligatorio)

Actualmente, el tipo de regla debe ser **MatchRule**.

### <a name="match-variable-required"></a>Variable de coincidencia (obligatorio)

La variable de coincidencia debe ser una de las siguientes:

- RemoteAddr: dirección IP o nombre de host de la conexión del equipo remoto
- RequestMethod: método de solicitud HTTP (GET, POST, PUT, DELETE y así sucesivamente).
- QueryString: variable en el URI.
- PostArgs: los argumentos enviados en el cuerpo de POST. Las reglas personalizadas que usan esta variable de coincidencia solo se aplican si el encabezado Content-Type está establecido en "application/x-www-form-urlencoded" y "multipart/form-data".
- RequestUri: URI de la solicitud.
- RequestHeaders: los encabezados de la solicitud.
- RequestBody: variable que contiene el cuerpo de la solicitud en su totalidad. Las reglas personalizadas que usan esta variable de coincidencia solo se aplican si el encabezado Content-Type está establecido en "application/x-www-form-urlencoded". 
- RequestCookies: las cookies de la solicitud.

### <a name="selector-optional"></a>Selector (opcional)

El selector describe el campo de la colección matchVariable. Por ejemplo, si matchVariable es "RequestHeaders", el selector podría estar en el encabezado User-Agent.

### <a name="operator-required"></a>Operador (obligatorio)

El operador debe ser uno de los siguientes:

- IPMatch: este operador se usa solo cuando la variable de coincidencia es *RemoteAddr*.
- Equals: la entrada es la misma que MatchValue.
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- Regex

### <a name="negate-condition-optional"></a>Negar condición (opcional)

Niega la condición actual.

### <a name="transform-optional"></a>Transformación (opcional)

Una lista de cadenas con los nombres de las transformaciones que se van a completar antes de la coincidencia. Entre las transformaciones se incluyen:

- Minúsculas
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Valores de coincidencia (obligatorio)

El campo matchValues es una lista de valores con los que se va a comparar, que se pueden considerar como combinados mediante *or*. Por ejemplo, los valores podrían ser direcciones IP u otras cadenas. El formato del valor depende del operador anterior.

### <a name="action-required"></a>Acción (obligatorio)

El campo de acción ofrece las siguientes opciones: 

- Permitir: autoriza la transacción y omite todas las reglas siguientes. Esto significa que la solicitud especificada se agrega a la lista de permitidos y, una vez que coincide, la solicitud detiene la evaluación adicional y se envía al grupo de back-end. En las reglas que están en la lista de permitidos no se evalúan más reglas administradas ni personalizadas.

- Bloquear: bloquea la transacción en función de *SecDefaultAction* (modo de detección o prevención). Como sucede con la acción Allow, una vez que la solicitud se evalúa y se agrega a la lista de bloqueadas, la evaluación se detiene y la solicitud se bloquea. Las solicitudes subsiguientes que cumplen las mismas condiciones no se evalúan. Solo se bloquean. 

- Log: permite que la regla escriba en el registro y que las reglas restantes se ejecuten para la evaluación. Las reglas personalizadas siguientes se evalúan en orden de prioridad, seguidas por las reglas administradas.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha obtenido información sobre las reglas personalizadas, puede [crear sus propias reglas personalizadas](create-custom-waf-rules.md).
