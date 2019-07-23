---
title: 'Personalización de las reglas de firewall de aplicaciones web en Azure Application Gateway: Azure Portal'
description: En este artículo se proporciona información acerca de cómo personalizar reglas de firewall de aplicaciones web en Application Gateway con Azure Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: victorh
ms.openlocfilehash: f7ffb8d6adfd4afc75618834a3fe82cf9a3d0c9f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64720380"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Personalización de reglas de firewall de aplicaciones web mediante Azure Portal

El firewall de aplicaciones web (WAF) de Azure Application Gateway proporciona protección a las aplicaciones web. Dicha protección la proporciona Open Web Application Security Project (OWASP) Core Rule Set (CRS). Algunas reglas pueden producir falsos positivos y bloquear el tráfico real. Por este motivo, Application Gateway ofrece la posibilidad de personalizar reglas y grupos de reglas. Para más información acerca de reglas y grupos de reglas específicos, consulte [Lista de reglas y grupos de reglas de CRS de firewall de aplicaciones web que se ofrecen](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Si la puerta de enlace de aplicaciones no usa el nivel WAF, la opción para actualizar la puerta de enlace de aplicaciones al nivel WAF aparece en el panel derecho. 

![Habilitar WAF][fig1]

## <a name="view-rule-groups-and-rules"></a>Visualización de reglas y grupos de reglas

**Para ver reglas y grupos de reglas**
   1. Navegue hasta la puerta de enlace de aplicaciones y seleccione **Firewall de aplicaciones web**.  
   2. Seleccione **Configuración de reglas avanzada**.  
   Esta vista muestra una tabla en la página de todos los grupos de reglas proporcionados con el conjunto de reglas elegido. Todas las casillas de la regla están seleccionadas.

![Configurar reglas deshabilitadas][1]

## <a name="search-for-rules-to-disable"></a>Búsqueda de reglas para deshabilitar

La página **Web application firewall settings** (Configuración del firewall de aplicaciones web) ofrece la posibilidad de filtrar las reglas a través de una búsqueda de texto. El resultado muestra solo las reglas y los grupos de reglas que contienen el texto que se ha buscado.

![Buscar reglas][2]

## <a name="disable-rule-groups-and-rules"></a>Deshabilitación de reglas y grupos de reglas

> [!IMPORTANT]
> Tenga cuidado al deshabilitar los grupos de reglas o reglas. Esta acción puede exponerlo a mayores riesgos de seguridad.

Cuando deshabilita reglas, puede deshabilitar un grupo de reglas completo o reglas específicas de uno o más grupos de reglas. 

**Para deshabilitar grupos de reglas o reglas concretas**

   1. Busque las reglas o los grupos de reglas que desea deshabilitar.
   2. Desactive las casillas de las reglas que desea deshabilitar. 
   2. Seleccione **Guardar**. 

![Guardar cambios][3]

## <a name="mandatory-rules"></a>Reglas obligatorias

La lista siguiente contiene las condiciones que provocan que el firewall de aplicaciones web bloquee la solicitud durante el modo de prevención. En modo de detección, se registran como excepciones.

Estas condiciones no se pueden configurar o deshabilitar:

* La imposibilidad de analizar el cuerpo de la solicitud tiene como resultado que la solicitud se bloquea, a menos que la inspección del cuerpo esté desactivada (XML, JSON, datos de formulario).
* La longitud de los datos del cuerpo de la solicitud (sin archivos) es mayor que el límite configurado.
* El cuerpo de la solicitud (archivos incluidos) es mayor que el límite.
* Se produjo un error interno en el motor de WAF.

Específico de CRS 3.x:

* La puntuación de anomalías de entrada superó el umbral.

## <a name="next-steps"></a>Pasos siguientes

Después de configurar las reglas deshabilitadas, puede aprender a ver los registros de WAFS. Para más información, consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
