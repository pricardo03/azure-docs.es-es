---
title: Introducción a firewall de aplicaciones web para Azure Application Gateway
description: Este artículo proporciona una introducción al firewall de aplicaciones web (WAF) de Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: amsriva
ms.topic: conceptual
ms.openlocfilehash: 830513a03bd65ca14cb0938ae599a676f1bb3bca
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518191"
---
# <a name="web-application-firewall-for-azure-application-gateway"></a>Firewall de aplicaciones Web de Azure Application Gateway

Azure Application Gateway ofrece un firewall de aplicaciones web (WAF) que proporciona una protección centralizada de las aplicaciones web contra vulnerabilidades de seguridad comunes. Las aplicaciones Web se aplica cada vez más ataques malintencionados que aprovechan vulnerabilidades conocidas comúnmente. Inyección de código SQL y el scripting entre sitios se encuentran entre los ataques más comunes.

Impedir tales ataques en el código de aplicación es un desafío. Puede requerir tareas rigurosas de mantenimiento, aplicación de revisiones y supervisión en varias capas de la topología de la aplicación. Un firewall de aplicaciones web centralizado facilita la administración de seguridad mucho más sencillo. Un WAF también proporciona a los administradores de aplicación mayor protección de protección contra amenazas e intrusiones.

Una solución WAF puede reaccionar ante una amenaza de seguridad más rápida aplicando revisiones que aborden una vulnerabilidad conocida, en lugar de proteger cada aplicación web individual de forma centralizada. Puertas de enlace de aplicaciones existentes pueden convertirse fácilmente en puertas de enlace de aplicaciones basadas en la pared de incendios.

El WAF de Application Gateway se basa en [Core Rule Set (CRS)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 o 2.2.9 de Open Web Application Security Project (OWASP). El WAF se actualiza automáticamente para incluir la protección contra vulnerabilidades de nuevo, no necesitan configuraciones adicionales.

![Diagrama de WAF de puerta de enlace de aplicaciones](./media/waf-overview/WAF1.png)

Instancia de Application Gateway funciona como un controlador de entrega de aplicaciones (ADC). Ofrece terminación de la capa de Sockets seguros (SSL), afinidad de sesión basada en cookies, distribución de carga round robin, enrutamiento basado en contenido, posibilidad de hospedar varios sitios Web y las mejoras de seguridad.

Mejoras de seguridad de puerta de enlace de aplicaciones incluyen la administración de directivas SSL y SSL de extremo a extremo admite. Se refuerza la seguridad de la aplicación mediante la integración de WAF en Application Gateway. La combinación protege las aplicaciones web contra las vulnerabilidades más comunes. Y proporciona una ubicación central para administrar y configurar.

## <a name="benefits"></a>Ventajas

Esta sección describen las ventajas principales que proporcionan Application Gateway y su WAF.

### <a name="protection"></a>Protección

* Proteja sus aplicaciones web frente a vulnerabilidades web y ataques sin modificación del código de back-end.

* Proteger varias aplicaciones web al mismo tiempo. Puede hospedar una instancia de Application Gateway de hasta 20 sitios Web que están protegidos por un firewall de aplicaciones web.

### <a name="monitoring"></a>Supervisión

* Supervisar los ataques contra sus aplicaciones web mediante el uso de un registro de WAF en tiempo real. El registro se integra con [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) para realizar un seguimiento de las alertas de WAF y supervisar con facilidad las tendencias.

* El WAF de Application Gateway se integra con Azure Security Center. Security Center proporciona una visión central del estado de seguridad de todos los recursos de Azure.

### <a name="customization"></a>Personalización

* Puede personalizar las reglas de WAF y grupos de reglas para satisfacer las necesidades de su aplicación y eliminar los falsos positivos.

## <a name="features"></a>Características

- Protección de inyección de código SQL.
- Protección de scripting entre sitios.
- Protección frente a otros ataques web comunes, como inyección de comandos, contrabando de división de respuestas HTTP de solicitudes HTTP e inclusión de archivos remoto.
- Protección contra infracciones del protocolo HTTP.
- Protección contra anomalías del protocolo HTTP, como la falta de host de agente de usuario y los encabezados de aceptación.
- Protección contra bots, rastreadores y escáneres.
- Detección de errores comunes (por ejemplo, Apache e IIS).
- Límites de tamaño configurable de la solicitud con los límites inferior y superior.
- Las listas de exclusión le permiten omitir determinados atributos de la solicitud de una evaluación de WAF. Un ejemplo común es insertada por Active Directory los tokens que se usan para la autenticación o campos de contraseña.

### <a name="core-rule-sets"></a>Conjuntos de reglas principales

Application Gateway admite dos conjuntos de reglas: CRS 3.0 y CRS 2.2.9. Estas reglas protegen sus aplicaciones web frente a actividades malintencionadas.

La puerta de enlace de aplicaciones WAF viene preconfigurado con CRS 3.0 de forma predeterminada. Pero puede elegir usar CRS 2.2.9 en su lugar. CRS 3.0 permite reducir los falsos positivos en comparación con CRS 2.2.9. También puede [Personalizar reglas para satisfacer sus necesidades](application-gateway-customize-waf-rules-portal.md).

El Firewall de aplicaciones protege contra las vulnerabilidades web siguientes:

- Ataques de inyección de SQL
- Ataques de scripts entre sitios
- Otros ataques comunes, como la inyección de comandos, HTTP solicitan contrabando, división de la respuesta HTTP y la inclusión de archivo remoto
- Infracciones del protocolo HTTP
- Anomalías del protocolo HTTP, como la falta de hospedan de agente de usuario y los encabezados de aceptación
- Los bots, rastreadores y escáneres
- Aplicación las configuraciones erróneas comunes (por ejemplo, Apache e IIS)

#### <a name="owasp-crs-30"></a>OWASP CRS 3.0

CRS 3.0 incluye 13 grupos de reglas, tal como se muestra en la tabla siguiente. Cada grupo contiene varias reglas, que se pueden deshabilitar.

|Grupo de reglas|DESCRIPCIÓN|
|---|---|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Bloquear métodos (PUT, PATCH)|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**|Protegerse frente a escáneres de puertos y entornos|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Protéjase de protocolo y problemas de codificación|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Protección contra la inyección de encabezado, contrabando de solicitudes y división de respuestas|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Protección frente a ataques de archivo y ruta de acceso|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Protección frente a ataques de inclusión (RFI) del archivo remoto|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Protegerse contra ataques de ejecución remota de código|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|Protección frente a ataques de inyección en PHP|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Protegerse de ataques de scripting entre sitios|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|Protección frente a ataques de inyección de código SQL|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Protección frente a ataques de fijación de sesión|

#### <a name="owasp-crs-229"></a>OWASP CRS 2.2.9

CRS 2.2.9 incluye 10 grupos de reglas, tal como se muestra en la tabla siguiente. Cada grupo contiene varias reglas, que se pueden deshabilitar.

|Grupo de reglas|DESCRIPCIÓN|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Proteger contra infracciones de protocolo (por ejemplo, los caracteres no válidos o una operación GET con un cuerpo de solicitud)|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Protegerse frente a la información de encabezado incorrecto|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Proteger frente a argumentos o archivos que superan las limitaciones|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Protegerse frente a métodos restringidos, encabezados y tipos de archivo|
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Proteger frente a escáneres y rastreadores web|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Proteger contra ataques genéricos (por ejemplo, la fijación de sesión, inclusión de archivo remoto e inyección en PHP)|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|Protección frente a ataques de inyección de código SQL|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Protegerse de ataques de scripting entre sitios|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Protección frente a ataques de recorrido de la ruta de acceso|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Protegerse frente a troyanos de puerta trasera|

### <a name="waf-modes"></a>Modos de WAF

El WAF de Application Gateway puede configurarse para ejecutarse en los dos modos siguientes:

* **Modo de detección**: Supervisa y registra todas las alertas de amenaza. Activar el registro de diagnósticos para Application Gateway en el **diagnósticos** sección. También hay que asegurarse de que el registro de WAF está seleccionado y activado. Firewall de aplicaciones Web no bloquea las solicitudes entrantes cuando se está ejecutando en modo de detección.
* **Modo de prevención**: Las intrusiones de bloques y los ataques que las reglas de detección. El atacante recibe una excepción "acceso no autorizado 403", y se termina la conexión. Modo de prevención registra estos ataques en los registros de WAF.

### <a name="anomaly-scoring-mode"></a>Modo de puntuación de anomalías
 
OWASP tiene dos modos para decidir si va a bloquear el tráfico: Modo tradicional y modo de puntuación de anomalías.

En el modo tradicional, el tráfico que coincide con ninguna regla se considera independientemente de las otras coincidencias de regla. Este modo es fácil de entender. Pero la falta de información sobre cuántas reglas coinciden con una solicitud específica es una limitación. Por lo tanto, se introdujo el modo de puntuación de anomalías. Es el valor predeterminado de 3 OWASP. *x*.

En el modo de puntuación de anomalías, el tráfico que coincide con cualquier regla no se bloquea inmediatamente cuando el firewall está en modo de prevención. Las reglas tienen una gravedad determinada: *Crítica*, *Error*, *advertencia*, o *aviso*. Que la gravedad afecta a un valor numérico para la solicitud, que se llama a la puntuación de anomalías. Por ejemplo, una *advertencia* regla de coincidencia contribuye 3 a la puntuación. Una *crítico* regla de coincidencia contribuye a 5.

Hay un umbral de 5 para la puntuación de anomalías para bloquear el tráfico. Por lo tanto, una sola *crítico* coincidencia de regla es suficiente para la puerta de enlace de aplicaciones WAF bloquear una solicitud, incluso en modo de prevención. Pero una *advertencia* regla de coincidencia solo aumenta la anomalía puntuación por 3, que no es suficiente por sí solo para bloquear el tráfico.

> [!NOTE]
> El mensaje que se registra cuando una regla de WAF coincide con el tráfico incluye el valor de la acción "Bloqueado". Pero el tráfico es realmente solo bloqueadas de una puntuación de anomalía de 5 o superior.  

### <a name="waf-monitoring"></a>Supervisión de WAF

Es importante supervisar el estado de la instancia de Application Gateway. Supervisión del estado de su WAF y las aplicaciones que protege es compatible con la integración con Azure Security Center, Azure Monitor, y los registros de Azure Monitor.

![Diagrama de diagnóstico de WAF de Application Gateway](./media/waf-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure Monitor

Registros de puerta de enlace de aplicaciones se integran con [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md). Esto le permite realizar un seguimiento de información de diagnóstico, incluidos los registros y alertas de WAF. Puede obtener acceso a esta funcionalidad en el **diagnósticos** ficha en el recurso de Application Gateway en el portal o directamente a través de Azure Monitor. Para más información acerca de cómo habilitar los registros, consulte [diagnósticos de Application Gateway](application-gateway-diagnostics.md).

#### <a name="azure-security-center"></a>Azure Security Center

[Security Center](../security-center/security-center-intro.md) ayuda a evitar, detectar y responder a amenazas. Proporciona mayor visibilidad y control sobre la seguridad de los recursos de Azure. Application Gateway es [integrados con Security Center](application-gateway-integration-security-center.md). Security Center examina su entorno para detectar aplicaciones web no protegidas. Puede recomendar WAF de Application Gateway para proteger estos recursos vulnerables. Cree los servidores de seguridad directamente desde Security Center. Estas instancias de WAF se integran con Security Center. Envían alertas e información de estado al centro de seguridad para los informes.

![Ventana de información general de Security Center](./media/waf-overview/figure1.png)

#### <a name="logging"></a>Registro

WAF de Application Gateway ofrece informes detallados sobre cada amenaza que detecte. El registro se integra con los registros de diagnóstico de Azure. Las alertas se registran en el formato JSON. Estos registros pueden integrarse con los [registros de Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).

![Registros de diagnóstico de puerta de enlace de aplicaciones de windows](./media/waf-overview/waf2.png)

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

El WAF de Application Gateway está disponible en una nueva SKU. Esta SKU está disponible solo en el modelo de aprovisionamiento de Azure Resource Manager, no en el modelo de implementación clásica. Además, la SKU de WAF está disponible sólo en tamaños de instancia de Application Gateway medianas y grandes. Todos los límites de Application Gateway también se aplican a la SKU de WAF.

Los precios se basan en un cargo por hora de instancia de puerta de enlace y una carga de procesamiento de datos. [Precios de Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/) difiere de la SKU de WAF de cargos de la SKU estándares. Los cargos de procesamiento de datos son los mismos. No hay ningún cargo por regla o un grupo de reglas. Puede proteger varias aplicaciones web detrás del mismo firewall de aplicación web. No se cobra por admitir varias aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

Consulte [cómo configurar firewall de aplicaciones web en Application Gateway](tutorial-restrict-web-traffic-powershell.md).
