---
title: Reglas personalizadas de Firewall de aplicaciones Web de Azure (WAF) v2 en Application Gateway
description: En este artículo se proporciona una introducción a las reglas personalizadas del Firewall de aplicaciones web (WAF) v2 en Azure Application Gateway.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/04/2019
ms.author: victorh
ms.openlocfilehash: 0ac37378797c59d79af5d026200b68154836c5ac
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585395"
---
# <a name="custom-rules-for-web-application-firewall-v2-on-azure-application-gateway"></a>Personalización de reglas del Firewall de aplicaciones web v2 en Azure Application Gateway

El firewall de aplicaciones web (WAF) v2 de Azure Application Gateway viene con un conjunto de reglas preconfiguradas y administradas por la plataforma que ofrece protección frente a muchos tipos de ataques diferentes. Estos ataques incluyen scripts entre sitios, inyección de código SQL y otros. Si es un administrador de WAF, puede escribir reglas propias para aumentar las del conjunto de reglas básico (CRS). Las reglas pueden bloquear o permitir el tráfico solicitado en función de criterios de coincidencia.

Las reglas personalizadas permiten crear reglas propias que se evalúan en cada solicitud que pasa por el WAF. Estas reglas tienen una prioridad mayor que el resto de las reglas de los conjuntos de reglas administrados. Las reglas personalizadas contienen un nombre de regla, la prioridad de la regla y una matriz de condiciones de coincidencia. Si estas condiciones se cumplen, se realiza una acción (para permitir o bloquear).

Por ejemplo, puede bloquear todas las solicitudes de una dirección IP en el intervalo 192.168.5.4/24. En esta regla, el operador es *IPMatch*, matchValues es el intervalo de direcciones IP (192.168.5.4/24) y la acción es bloquear el tráfico. También se establecen el nombre y la prioridad de la regla.

Las reglas personalizadas admiten el uso de lógica de composición para crear reglas más avanzadas que solucionen los requisitos de seguridad. Por ejemplo, (Condición 1 **y** Condición 2) **o** Condición 3).  En este ejemplo significa que si se cumplen Condición 1 **y** Condición 2, **o** si se cumple Condición 3, el WAF debe realizar la acción especificada en la regla personalizada.

En una misma regla, las condiciones de coincidencia diferentes siempre se combinan mediante **y**. Por ejemplo, bloquear el tráfico desde una dirección IP específica, y solo si se usa un explorador determinado.

Si quiere aplicar **o** a dos condiciones diferentes, las dos deben estar en reglas distintas. Por ejemplo, bloquear el tráfico desde una dirección IP específica o bloquear el tráfico si se usa un explorador determinado.

> [!NOTE]
> El número máximo de reglas personalizadas de WAF es 100. Para más información sobre los límites de Application Gateway, vea [Suscripción de Azure y límites de servicio, cuotas y restricciones](../../azure-subscription-service-limits.md#application-gateway-limits).

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

- Determina el orden de evaluación de la regla. Cuanto menor sea el valor, antes se evalúa la regla. El intervalo permitido es de 1 a 100. 
- Debe ser única en todas las reglas personalizadas. Una regla con prioridad de 40 es evaluada antes que una regla con prioridad de 80.

### <a name="rule-type-required"></a>Tipo de regla [obligatorio]

En la actualidad, debe ser **MatchRule**.

### <a name="match-variable-required"></a>Variable de coincidencia [obligatorio]

Debe ser una de estas variables:

- RemoteAddr: dirección IP o nombre de host de la conexión del equipo remoto
- RequestMethod: método de solicitud HTTP (GET, POST, PUT, DELETE y así sucesivamente).
- QueryString: la variable en el URI
- PostArgs: los argumentos enviados en el cuerpo de POST. Las reglas personalizadas que usan esta variable de coincidencia solo se aplican si el encabezado "Content-Type" está establecido en "application/x-www-form-urlencoded" y "multipart/form-data".
- RequestUri: URI de la solicitud
- RequestHeaders: los encabezados de la solicitud
- RequestBody: contiene el cuerpo de la solicitud en su totalidad. Las reglas personalizadas que usan esta variable de coincidencia solo se aplican si el encabezado "Content-Type" está establecido en "application/x-www-form-urlencoded". 
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
- Geomatch (vista previa)

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

## <a name="geomatch-custom-rules-preview"></a>Reglas personalizadas de Geomatch (vista previa)

Las reglas personalizadas permiten la creación de reglas adaptadas para satisfacer las necesidades precisas de las aplicaciones y las directivas de seguridad. Ahora puede restringir el acceso a las aplicaciones web por país o región, característica que está disponible en versión preliminar pública. Como con todas las reglas personalizadas, esta lógica se puede componer con otras reglas para satisfacer las necesidades de la aplicación. 

   > [!NOTE]
   > Las reglas personalizadas de geocoincidencia están disponibles en Centro-sur de EE. UU. y Norte de Europa. Para acceder a ellas en el portal, use [este vínculo](https://aka.ms/AppGWWAFGeoMatch) hasta que esté disponible para todos. 

Si usa el operador Geomatch, los selectores pueden ser cualquiera de los siguientes códigos de país de dos dígitos. 

|Código de país | Nombre del país |
| ----- | ----- |
| AD | Andorra |
| AE | Emiratos Árabes Unidos|
| AF | Afganistán|
| AG | Antigua y Barbuda|
| AL | Albania|
| AM | Armenia|
| AO | Angola|
| AR | Argentina|
| AS | Samoa Americana|
| AT | Austria|
| AU | Australia|
| AZ | Azerbaiyán|
| BA | Bosnia y Herzegovina|
| BB | Barbados|
| BD | Bangladés|
| BE | Bélgica|
| BF | Burkina Faso|
| BG | Bulgaria|
| BH | Bahréin|
| BI | Burundi|
| BJ | Benín|
| BL | San Bartolomé|
| BN | Brunéi Darussalam|
| BO | Bolivia|
| BR | Brasil|
| BS | Bahamas|
| BT | Bután|
| BW | Botsuana|
| BY | Belarús|
| BZ | Belice|
| CA | Canadá|
| CD | República Democrática del Congo|
| CF | República Centroafricana|
| CH | Suiza|
| CI | República de Côte d’Ivoire|
| CL | Chile|
| CM | Camerún|
| CN | China|
| CO | Colombia|
| CR | Costa Rica|
| CU | Cuba|
| CV | Cabo Verde|
| CY | Chipre|
| CZ | República Checa|
| DE | Alemania|
| DK | Dinamarca|
| DO | República Dominicana|
| DZ | Argelia|
| EC | Ecuador|
| EE | Estonia|
| EG | Egipto|
| ES | España|
| ET | Etiopía|
| FI | Finlandia|
| FJ | Islas Fiji|
| FM | Micronesia, Estados Federados de|
| VF | Francia|
| GB | Reino Unido|
| GE | Georgia|
| GF | Guayana Francesa|
| GH | Ghana|
| GN | Guinea|
| GP | Guadalupe|
| GR | Grecia|
| GT | Guatemala|
| GY | Guayana|
| HK | RAE de Hong Kong|
| HN | Honduras|
| HR | Croacia|
| HT | Haití|
| HU | Hungría|
| id | Indonesia|
| IE | Irlanda|
| IL | Israel|
| IN | India|
| IQ | Iraq|
| IR | Irán, República Islámica de|
| IS | Islandia|
| IT | Italia|
| JM | Jamaica|
| JO | Jordania|
| JP | Japón|
| KE | Kenia|
| KG | Kirguistán|
| KH | Camboya|
| KI | Kiribati|
| KN | San Cristóbal y Nieves|
| KP | Corea, República Popular Democrática de|
| KR | Corea, República de|
| KW | Kuwait|
| KY | Islas Caimán|
| KZ | Kazajistán|
| LA | República Democrática Popular de Laos|
| LB | Líbano|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberia|
| LS | Lesoto|
| LT | Lituania|
| LU | Luxemburgo|
| LV | Letonia|
| LY | Libia |
| MA | Marruecos|
| MD | Moldavia, República de|
| MG | Madagascar|
| MK | Macedonia del Norte|
| ML | Malí|
| MM | Myanmar|
| MN | Mongolia|
| MO | RAE de Macao|
| MQ | Martinica|
| MR | Mauritania|
| MT | Malta|
| MV | Maldivas|
| MW | Malaui|
| MX | México|
| MY | Malasia|
| MZ | Mozambique|
| N/D | Namibia|
| NE | Níger|
| NG | Nigeria|
| NI | Nicaragua|
| NL | Países Bajos|
| NO | Noruega|
| NP | Nepal|
| NR | Nauru|
| NZ | Nueva Zelanda|
| OM | Omán|
| PA | Panamá|
| PE | Perú|
| PH | Filipinas|
| PK | Pakistán|
| PL | Polonia|
| PR | Puerto Rico|
| PT | Portugal|
| PW | Palaos|
| PY | Paraguay|
| QA | Qatar|
| RE | Reunión|
| RO | Rumanía|
| RS | Serbia|
| RU | Federación Rusa|
| RW | Ruanda|
| SA | Arabia Saudí|
| SD | Sudán|
| SE | Suecia|
| SG | Singapur|
| SI | Eslovenia|
| SK | Eslovaquia|
| SN | Senegal|
| SO | Somalia|
| SR | Surinam|
| SS | Sudán del Sur|
| SV | El Salvador|
| SY | República Árabe Siria|
| SZ | Suazilandia|
| TC | Islas Turcas y Caicos|
| TG | Togo|
| TH | Tailandia|
| TN | Túnez|
| TR | Turquía|
| TT | Trinidad y Tobago|
| TW | Taiwán|
| TZ | Tanzania, República Unida de|
| UA | Ucrania|
| UG | Uganda|
| US | Estados Unidos|
| UY | Uruguay|
| UZ | Uzbekistán|
| VC | San Vicente y las Granadinas|
| VE | Venezuela|
| VG | Islas Vírgenes Británicas|
| VI | Islas Vírgenes de EE. UU.|
| VN | Vietnam|
| ZA | Sudáfrica|
| ZM | Zambia|
| ZW | Zimbabue|

## <a name="next-steps"></a>Pasos siguientes

Una vez que obtenga información sobre las reglas personalizadas, [cree reglas personalizadas propias](create-custom-waf-rules.md).
