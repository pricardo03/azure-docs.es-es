---
title: Regla personalizada de Web application firewall para el acceso de principal de Azure
description: Obtenga información sobre cómo utilizar reglas aplicaciones web (WAF) de firewall personalizadas proteger las aplicaciones web frente a ataques malintencionados.
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/07/2019
ms.author: kumud;tyao
ms.openlocfilehash: 744c6fb9235c9daa2d5239ef9fd13679db943650
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59783974"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Reglas personalizadas para firewall de aplicaciones web con Azure puerta de entrada
Firewall de aplicaciones web de Azure (WAF) con el servicio de puerta de entrada permite controlar el acceso a sus aplicaciones web en función de las condiciones que defina. Una regla de WAF personalizada consta de un número de prioridad, un tipo de regla, las condiciones de coincidencia y una acción. Hay dos tipos de reglas personalizadas: coincide con las reglas y reglas de limitación de velocidad. Una regla de coincidencia controla el acceso basado en condiciones de coincidencia, mientras que una regla de límite de velocidad controla el acceso basándose en la coincidencia de las condiciones y las tasas de solicitudes entrantes. Puede deshabilitar una regla personalizada para impedir que se está evaluando, pero mantener la configuración. En este artículo se describe las reglas de coincidencia que se basan en los parámetros http.

## <a name="priority-match-conditions-and-action-types"></a>Tipos de acciones, condiciones de coincidencia y prioridad
Puede controlar el acceso con una regla de WAf personalizada que define un número de prioridad, un tipo de regla, las condiciones de coincidencia y una acción. 

- **Prioridad:** es un entero único que describe el orden de evaluación de reglas de WAF. Las reglas con los valores más bajos se evalúan antes que las reglas con los valores más altos

- **Acción:** define cómo enrutar una solicitud si coincide con una regla de WAF. Puede elegir una de las siguientes acciones para que se aplica cuando una solicitud coincide con una regla personalizada.

    - *Permitir* -WAF reenvía la solicitud al back-end, registra una entrada en los registros de WAF y se cierra.
    - *Bloque* -solicitud está bloqueada, WAF envía la respuesta al cliente sin reenviar la solicitud al back-end. WAF registra una entrada en los registros de WAF.
    - *Registro* -registros de WAF una entrada de WAF se registra y se continúa evaluación la regla siguiente.
    - *Redirigir* -WAF redirige la solicitud a un URI especificado, registra una entrada en los registros de WAF y se cierra.

- **Condición de coincidencia:** define una variable de coincidencia, un operador y que coinciden con el valor. Cada regla puede contener varias condiciones de coincidencia. Una condición de coincidencia puede basarse en la siguiente *coinciden con las variables*:
    - DirRemota (IP de cliente)
    - requestMethod
    - QueryString
    - PostArgs
    - RequestUri
    - RequestHeader
    - RequestBody

- **Operador:** lista incluye lo siguiente:
    - Ninguno: a menudo se usa para definir la acción predeterminada si no hay reglas se cumplen. Cualquier es una coincidencia operador all.
    - IPMatch: definir restricciones de IP para la variable DirRemota
    - GeoMatch: definir geográfica filtrado para DirRemota variable
    - Igual
    - Contains
    - LessThan: restricción del tamaño
    - GreaterThan: restricción del tamaño
    - LessThanOrEqual: restricción del tamaño
    - GreaterThanOrEqual: restricción del tamaño
    - BeginsWith
     - endsWith

Puede establecer *negar* condición sea true si debe se niega el resultado de una condición.

*Coincide con el valor* define la lista de valores posibles de coincidencia.
Admite el método de solicitud HTTP, los valores incluyen:
- GET
- POST
- PUT
- HEAD
- DELETE
- BLOQUEO
- DESBLOQUEAR
- PERFIL
- OPCIONES
- PROPFIND
- PROPPATCH
- MKCOL
- COPIA
- MOVER

## <a name="examples"></a>Ejemplos

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>Ejemplo de reglas personalizadas de WAF en función de los parámetros de http

Este es un ejemplo que muestra la configuración de una regla personalizada con dos condiciones de coincidencia. Las solicitudes proceden de un sitio especificado según se define en el origen de referencia y cadena de consulta no contiene "password".

```
# http rules example
{
  "name": "AllowFromTrustedSites",
  "priority": 1,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestHeader",
      "selector": "Referer",
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "www.mytrustedsites.com/referpage.html"
      ]
    },
    {
      "matchVariable": "QueryString",
      "operator": "Contains",
      "matchValue": [
        "password"
      ],
      "negateCondition": true
    }
  ],
  "action": "Allow",
  "transforms": []
}

```
Se muestra un ejemplo de configuración para bloquear el método "PUT" como sigue:

``` 
# http Request Method custom rules
{
  "name": "BlockPUT",
  "priority": 2,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestMethod",
      "selector": null,
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "PUT"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

### <a name="size-constraint"></a>Restricción del tamaño

Puede crear una regla personalizada que especifica la restricción de tamaño en la parte de una solicitud entrante. Por ejemplo, a continuación de la regla bloquea una dirección Url que tiene más de 100 caracteres.

```
# http parameters size constraint
{
  "name": "URLOver100",
  "priority": 5,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestUri",
      "selector": null,
      "operator": "GreaterThanOrEqual",
      "negateCondition": false,
      "matchValue": [
        "100"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre [firewall de aplicaciones web](waf-overview.md)
- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).

