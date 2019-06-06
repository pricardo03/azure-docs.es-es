---
title: Reglas personalizadas de Azure con Firewall de aplicaciones Web (WAF)
description: En este artículo proporciona información general de firewall de aplicaciones web reglas personalizadas de (WAF) de Azure Application Gateway.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 62259749e04d66d78206a0bba77ce88f2c08c82f
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66689007"
---
# <a name="custom-rules-for-web-application-firewall"></a>Reglas personalizadas para Firewall de aplicaciones Web

> [!IMPORTANT]
> Reglas personalizadas de Azure Application Gateway WAF está actualmente en versión preliminar pública. **Reglas personalizadas solo están disponibles para la SKU WAF_v2**.
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El firewall de aplicaciones web de Azure Application Gateway (WAF) viene con un conjunto de reglas configurada previamente, administrada a la plataforma que ofrece protección frente a muchos tipos diferentes de los ataques. Estos ataques se incluyen entre scripts de sitios, inyección de código SQL y otros. Si es un administrador de WAF, puede escribir sus propias reglas para aumentar la regla de core establecen reglas de (CRS). Las reglas pueden bloquear o permitir el tráfico solicitado en función de criterios de coincidencia.

Reglas personalizadas permiten crear sus propias reglas que se evalúan en cada solicitud que atraviesa el WAF. Estas reglas mantienen una prioridad mayor que el resto de las reglas en los conjuntos de reglas administrado. Las reglas personalizadas contienen un nombre de regla, la prioridad de regla y una matriz de condiciones de coincidencia. Si se cumplen estas condiciones, se realiza una acción (para permitir o bloquear).

Por ejemplo, puede bloquear todas las solicitudes de una dirección IP en el intervalo de 192.168.5.4/24. En esta regla, el operador es *IPMatch*, el matchValues es el intervalo de direcciones IP (192.168.5.4/24) y la acción es bloquear el tráfico. También establece nombre y la prioridad de la regla.

Reglas personalizadas se admiten el uso de lógica de interés para realizar esa dirección que sus requisitos de seguridad de reglas más avanzadas. Por ejemplo, (condición 1 **y** condición 2) **o** condición 3).  En este ejemplo significa que si condición 1 **y** se cumplen la condición 2, **o** si se cumple la condición 3, el firewall debe realizar la acción especificada en la regla personalizada.

Diferentes condiciones de búsqueda de coincidencias dentro de la misma regla siempre se agravan mediante **y**. Por ejemplo, bloquear el tráfico de una dirección IP específica, y solo si usa un explorador determinado.

Si desea **o** dos condiciones diferentes, las dos condiciones deben estar en distintas reglas. Por ejemplo, bloquear el tráfico desde un tráfico de bloque o dirección IP específico si está usando un explorador específico.

> [!NOTE]
> El número máximo de reglas personalizadas de WAF es 100. Para obtener más información acerca de los límites de Application Gateway, consulte [suscripción de Azure y límites de servicio, cuotas y restricciones](../azure-subscription-service-limits.md#application-gateway-limits).

También se admiten expresiones regulares en las reglas personalizadas, al igual que en los conjuntos de reglas CRS. Para obtener ejemplos de estos, consulte los ejemplos 3 y 5 en [creación y uso de reglas de firewall de aplicación web personalizada](create-custom-waf-rules.md).

## <a name="allowing-vs-blocking"></a>Lo que permite frente al bloqueo

Permitir y bloquear el tráfico es sencilla con las reglas personalizadas. Por ejemplo, puede bloquear todo el tráfico procedente de un intervalo de direcciones IP. Puede hacer que otra regla para permitir el tráfico si la solicitud proviene de un explorador específico.

Para permitir que algo, asegúrese de que el `-Action` parámetro está establecido en **permitir**. Para bloquear algo, asegúrese de que el `-Action` parámetro está establecido en **bloque**.

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

La anterior `$BlockRule` se asigna a la siguiente regla personalizada en Azure Resource Manager:

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

Esta regla personalizada contiene un nombre, prioridad, una acción y la matriz de condiciones que deben cumplirse para que tienen lugar la acción de coincidencia. Para obtener una explicación de estos campos, consulte las siguientes descripciones de campos. Por ejemplo reglas personalizadas, consulte [creación y uso de reglas de firewall de aplicación web personalizada](create-custom-waf-rules.md).

## <a name="fields-for-custom-rules"></a>Campos de reglas personalizadas

### <a name="name-optional"></a>[Opcional] nombre

Este es el nombre de la regla. Este nombre aparece en los registros.

### <a name="priority-required"></a>Prioridad [requerido]

- Determina el orden en que las reglas se evalúan en. Menor será el valor, el anterior la evaluación de la regla.
-Debe ser único entre todas las reglas personalizadas. Una regla con prioridad de 100 se evalúa antes que una regla con prioridad 200.

### <a name="rule-type-required"></a>Tipo de regla [obligatorio]

Actualmente, debe ser **MatchRule**.

### <a name="match-variable-required"></a>Variable de coincidencia [obligatorio]

Debe ser una de las variables:

- DirRemota: dirección/nombre de host de IP de la conexión del equipo remoto
- RequestMethod: método de solicitud HTTP (GET, POST, PUT, DELETE y así sucesivamente.)
- Cadena de consulta: la Variable en el URI
- PostArgs: argumentos envían en el cuerpo de POST
- RequestUri: URI de la solicitud
- RequestHeaders: encabezados de la solicitud
- RequestBody – cuerpo de la solicitud
- RequestCookies: las Cookies de la solicitud

### <a name="selector-optional"></a>Selector de [opcional]

Describe el campo de la colección matchVariable. Por ejemplo, si la matchVariable es RequestHeaders, podría ser el selector en la *User-Agent* encabezado.

### <a name="operator-required"></a>Operador [obligatorio]

Debe ser uno de los siguientes operadores:

- IPMatch - sólo se utiliza cuando es Variable coincidencia *DirRemota*
- Es igual a – de entrada es el mismo que el MatchValue
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- endsWith
- Regex

### <a name="negate-condition-optional"></a>Negar condición [opcional]

Niega la condición actual.

### <a name="transform-optional"></a>Transformación [opcional]

Se intenta realizar una lista de cadenas con los nombres de las transformaciones para hacer antes de la coincidencia. Puede tratarse de las transformaciones siguientes:

- Minúsculas
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Coincidan con los valores [obligatorio]

Lista de valores que debe coincidir con, que se puede considerar como *o*' ed. Por ejemplo, podría ser direcciones IP u otras cadenas. El formato del valor depende del operador anterior.

### <a name="action-required"></a>Acción [obligatorio]

- Permitir: autoriza la transacción, se omitirá todas las reglas. Esto significa que la solicitud especificada se agrega a la lista de permitidos y una vez que coinciden, la evaluación adicional se detiene la solicitud y se envía al grupo de back-end. No se evalúan las reglas que están en la lista de permitidos para reglas administradas ni aún más las reglas personalizadas.
- Bloquear: bloquea la transacción según *SecDefaultAction* (modo de detección o prevención). Al igual que la acción de permiso, una vez que la solicitud se evalúa y se agrega a la lista de bloques, evaluación se detiene y se bloquea la solicitud. Cualquier solicitud después de que cumpla con las mismas condiciones no se evaluarán y simplemente se bloqueará. 
- Registro: permite que la regla de escribir en el registro, pero permite que el resto de las reglas se ejecute para la evaluación. Reglas personalizadas subsiguientes se evalúan en orden de prioridad, seguido de las reglas administradas.

## <a name="next-steps"></a>Pasos siguientes

Una vez que obtenga información sobre las reglas personalizadas, [crear sus propias reglas personalizadas](create-custom-waf-rules.md).
