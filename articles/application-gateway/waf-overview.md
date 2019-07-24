---
title: Introducción al firewall de aplicaciones web de Azure Application Gateway
description: Este artículo proporciona una introducción al firewall de aplicaciones web (WAF) de Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 5/22/2019
ms.author: amsriva
ms.topic: conceptual
ms.openlocfilehash: 9c2759222198f5df682d9e7a5363c0d9679e0fad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65991398"
---
# <a name="web-application-firewall-for-azure-application-gateway"></a>Firewall de aplicaciones web de Azure Application Gateway

Azure Application Gateway ofrece un firewall de aplicaciones web (WAF) que brinda a las aplicaciones web una protección centralizada contra vulnerabilidades de seguridad comunes. Las aplicaciones web son cada vez más el objetivo de ataques malintencionados que aprovechan vulnerabilidades habitualmente conocidas. Los scripts entre sitios y las inyecciones de código SQL están dentro de los ataques más comunes.

Evitar dichos ataques en el código de la aplicación es todo un desafío. Puede requerir un mantenimiento riguroso, revisión y supervisión en varias capas de la topología de la aplicación. Un firewall de aplicaciones web centralizado ayuda a simplificar muchísimo la administración de la seguridad. Un WAF también proporciona a los administradores de la aplicación a un mejor control de la protección contra amenazas e intrusiones.

Las soluciones de WAF pueden reaccionar más rápido ante una amenaza de la seguridad mediante la aplicación centralizada de revisiones que aborden una vulnerabilidad conocida, en lugar de proteger de manera individual cada aplicación web. Las puertas de enlace de aplicación existentes se pueden convertir fácilmente en puertas de enlace de aplicación habilitadas para firewall.

El firewall de aplicaciones web de Application Gateway se basa en el [conjunto de reglas básicas (CRS)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 o 2.2.9 de OWASP (Open Web Application Security Project). El WAF se actualiza automáticamente para incluir protección frente a nuevas vulnerabilidades sin necesidad de configuración adicional.

![Diagrama de WAF de Application Gateway](./media/waf-overview/WAF1.png)

Application Gateway funciona como un controlador de entrega de aplicaciones (ADC). Ofrece terminación de la Capa de sockets seguros (SSL), afinidad de sesión basada en cookies, distribución de la carga en operaciones por turnos, enrutamiento basado en contenido, posibilidad de hospedar varios sitios y mejoras de seguridad.

Las mejoras de seguridad de Application Gateway incluyen la administración de directivas SSL y la compatibilidad con SSL de extremo a extremo. La integración de WAF con Application Gateway mejora la seguridad de las aplicaciones. La combinación protege las aplicaciones web ante las vulnerabilidades comunes y proporciona una ubicación central fácil de configurar que se va a administrar.

## <a name="benefits"></a>Ventajas

En esta sección se describen las ventajas principales que Application Gateway y su WAF proporcionan.

### <a name="protection"></a>Protección

* Proteja las aplicaciones web contra las vulnerabilidades y los ataques web si modificación del código de back-end.

* Proteja varias aplicaciones web al mismo tiempo. Una instancia de Application Gateway puede hospedar hasta 100 sitios web protegidos por un firewall de aplicaciones web.

### <a name="monitoring"></a>Supervisión

* Supervise los ataques contra sus aplicaciones web con un registro de WAF en tiempo real. El registro se integra con [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) para hacer un seguimiento de las alertas de WAF y permite supervisar con facilidad las tendencias.

* El WAF de Application Gateway está integrado con Azure Security Center. Security Center proporciona una visión centralizada del estado de la seguridad de todos los recursos de Azure.

### <a name="customization"></a>Personalización

* Puede personalizar las reglas y grupos de reglas del WAF para satisfacer los requisitos de su aplicación y eliminar los falsos positivos.

## <a name="features"></a>Características

- Protección contra inyección de código SQL.
- Protección contra scripts entre sitios.
- Protección contra otros ataques web comunes, como la inyección de comandos, el contrabando de solicitudes HTTP, la división de respuestas HTTP y la inclusión de archivos remotos.
- Protección contra infracciones del protocolo HTTP.
- Protección contra anomalías del protocolo HTTP, como la falta de agentes de usuario de host y encabezados de aceptación.
- Protección contra bots, rastreadores y escáneres.
- Detección de errores de configuración comunes de las aplicaciones (por ejemplo, Apache e IIS).
- Límites de tamaño de solicitud configurables con límites inferior y superior.
- Las listas de exclusión le permiten omitir determinados atributos de una solicitud de una evaluación del WAF. Un ejemplo común son los tokens insertados de Active Directory que se usan para campos de contraseña o autenticación.

### <a name="core-rule-sets"></a>Conjuntos de reglas principales

Application Gateway admite dos conjuntos de reglas: CRS 3.0 y CRS 2.2.9. Estas reglas protegen las aplicaciones web frente a actividades malintencionadas.

El WAF de Application Gateway está preconfigurado con CRS 3.0 de manera predeterminada, pero puede elegir usar CRS 2.2.9 en su lugar. CRS 3.0 ofrece menos falsos positivos en comparación con CRS 2.2.9. También puede [personalizar reglas para satisfacer sus necesidades](application-gateway-customize-waf-rules-portal.md).

El WAF brinda protección contra las vulnerabilidades web siguientes:

- Ataques por inyección de código SQL.
- Ataques por scripts entre sitios.
- Otros ataques comunes, como la inyección de comandos, el contrabando de solicitudes HTTP, la división de respuestas HTTP y la inclusión de archivos remotos.
- Infracciones del protocolo HTTP.
- Anomalías del protocolo HTTP, como la falta de agentes de usuario de host y encabezados de aceptación.
- Bots, rastreadores y escáneres.
- Errores de configuración comunes de las aplicaciones (por ejemplo, Apache e IIS).

#### <a name="owasp-crs-30"></a>OWASP CRS 3.0

CRS 3.0 incluye 13 grupos de reglas, tal como se muestra en la tabla siguiente. Cada grupo contiene varias reglas, las que se pueden deshabilitar.

|Grupo de reglas|DESCRIPCIÓN|
|---|---|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Métodos de bloqueo (PUT, PATCH)|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**|Protección contra los escáneres de puertos y entornos|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Protección contra problemas de protocolo y codificación|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Protección contra ataques por inyección de encabezado, contrabando de solicitudes y división de respuestas|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Protección contra ataques a archivos y rutas de acceso|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Protección contra ataques por inclusión de archivos remotos (RFI)|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Protección contra ataques de ejecución de código remoto|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|Protección contra ataques por inyección de código PHP|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Protección contra ataques por scripts entre sitios|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|Protección contra ataques por inyección de código SQL|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Protección contra ataques por fijación de sesión|

#### <a name="owasp-crs-229"></a>OWASP CRS 2.2.9

CRS 2.2.9 incluye 10 grupos de reglas, tal como se muestra en la tabla siguiente. Cada grupo contiene varias reglas, las que se pueden deshabilitar.

|Grupo de reglas|DESCRIPCIÓN|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Protección contra infracciones de protocolo (como caracteres no válidos o GET con un cuerpo de solicitud)|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Protección contra información de encabezados incorrecta|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Protección contra argumentos o archivos que superan las limitaciones|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Protección contra métodos, encabezados y tipos de archivo restringidos|
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Protección contra escáneres y rastreadores web|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Protección contra ataques genéricos (como fijación de sesión, inclusión de archivos remotos e inyección de código PHP)|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|Protección contra ataques por inyección de código SQL|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Protección contra ataques por scripts entre sitios|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Protección contra ataques punto punto barra|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Protección contra troyanos de puerta trasera|

### <a name="waf-modes"></a>Modos de WAF

El WAF de Application Gateway se puede configurar para ejecutarse en dos modos:

* **Modo de detección**: supervisa y registra todas las alertas de amenazas. Puede activar el registro de diagnósticos de Application Gateway en la sección **Diagnósticos**. También debe asegurarse de que el registro del WAF está seleccionado y activado. El firewall de aplicaciones web no bloquea solicitudes entrantes cuando se ejecuta en modo de detección.
* **Modo de prevención**: bloquea las intrusiones y los ataques que detectan las reglas. El atacante recibe una excepción de "acceso no autorizado 403" y se termina la conexión. El modo de prevención registra dichos ataques en los registros del WAF.

### <a name="anomaly-scoring-mode"></a>Modo de puntuación de anomalías

OWASP tiene dos modos para decidir si bloquear el tráfico: el modo tradicional y el modo de puntuación de anomalías.

En el modo tradicional, se considera el tráfico que coincide con alguna regla, independientemente de si otra regla coincide. Este modo es fácil de entender, pero la falta de información sobre cuántas reglas coinciden con una solicitud específica representa una limitación. Es por eso que se introdujo el modo de puntuación de anomalías. Es el valor predeterminado en OWASP 3.*x*.

En el modo de puntuación de anomalías, el tráfico que coincide con alguna regla no se bloquea de inmediato cuando el firewall está en modo de prevención. Las reglas tienen una gravedad determinada: *Crítica*, *Error*, *Advertencia* o *Aviso*. Dicha gravedad afecta a un valor numérico para la solicitud, denominado como la puntuación de anomalía. Por ejemplo, una coincidencia con la regla *Advertencia* contribuye con un valor de 3 a la puntuación. Una coincidencia con la regla *Crítica*, con 5.

|Gravedad  |Valor  |
|---------|---------|
|Crítico     |5|
|Error        |4|
|Advertencia      |3|
|Aviso       |2|

Hay un umbral de 5 para que la puntuación de anomalías bloquee el tráfico. Por lo tanto, solo hace falta una coincidencia con la regla *Crítica* para que el WAF de Application Gateway bloquee una solicitud, includo en modo de prevención. Pero una coincidencia con la regla *Advertencia* solo aumenta la puntuación de anomalías en 3, lo que no es suficiente para bloquear por sí misma el tráfico.

> [!NOTE]
> El mensaje que se registra cuando una regla de WAF coincide con el tráfico incluye el valor de acción "Blocked" (Bloqueado), pero el tráfico en realidad solo se bloquea con una puntuación de anomalías de 5 o más.  

### <a name="waf-monitoring"></a>Supervisión de WAF

Es importante supervisar el estado de la instancia de Application Gateway. Supervisar el estado del WAF y las aplicaciones que protege es compatible con la integración con Azure Security Center, Azure Monitor y los registros de Azure Monitor.

![Diagrama de los diagnósticos de WAF de Application Gateway](./media/waf-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure Monitor

Los registros de Application Gateway se integran con [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md). Esto permite realizar un seguimiento de la información de diagnósticos incluidos los registros y las alertas de WAF. Puede acceder a esta funcionalidad en la pestaña **Diagnósticos** del recurso de Application Gateway en el portal o directamente en Azure Monitor. Para más información sobre cómo habilitar los registros, consulte el artículo sobre los [diagnósticos de Application Gateway](application-gateway-diagnostics.md).

#### <a name="azure-security-center"></a>Azure Security Center

[Security Center](../security-center/security-center-intro.md) ayuda a evitar y a detectar las amenazas, además de responder a ellas. Brinda una visibilidad mejorada y más control sobre la seguridad de los recursos de Azure. Application Gateway se [integra con Security Center](application-gateway-integration-security-center.md). Security Center examina su entorno para detectar aplicaciones web no protegidas. Puede recomendar el firewall de aplicaciones web (WAF) de Application Gateway para proteger estos recursos vulnerables. Puede crear los firewalls directamente desde Security Center. Estas instancias de WAF se integran con Security Center. Envían alertas e información de estado a Security Center para generar informes.

![Ventana de información general de Security Center](./media/waf-overview/figure1.png)

#### <a name="logging"></a>Registro

WAF de Application Gateway ofrece informes detallados sobre cada amenaza que detecta. El registro se integra con los registros de Azure Diagnostics. Las alertas se registran en el formato .json. Estos registros pueden integrarse con los [registros de Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).

![Ventanas de registros de diagnósticos de Application Gateway](./media/waf-overview/waf2.png)

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupId}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{appGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

## <a name="application-gateway-waf-sku-pricing"></a>Precios de las SKU de WAF de Application Gateway

El WAF de Application Gateway está disponible en una nueva SKU. Esta SKU está disponible solo en el modelo de aprovisionamiento de Azure Resource Manager y no en el modelo de implementación clásica. Además, la SKU de WAF está disponible solo para tamaños de instancias medianas y grandes de Application Gateway. Todos los límites de Application Gateway también se aplican a la SKU de WAF.

El precio se basa en un cargo por instancia de puerta de enlace por hora y un cargo por procesamiento de datos. Los [precios de Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/) para la SKU de WAF difieren de los cargos de SKU estándar. Los cargos por procesamiento de datos siguen iguales. No hay cargos por regla ni por grupo de reglas. Puede proteger varias aplicaciones web detrás del mismo firewall de aplicaciones web. No se le cobrará por admitir varias aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

Consulte los [procedimientos para configurar el firewall de aplicaciones web en Application Gateway](tutorial-restrict-web-traffic-powershell.md).
