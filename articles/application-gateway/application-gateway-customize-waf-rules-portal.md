---
title: Personalizar reglas de firewall de aplicaciones web en Azure Application Gateway mediante Azure portal
description: En este artículo se proporciona información acerca de cómo personalizar reglas de firewall de aplicaciones web en Application Gateway con Azure Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: victorh
ms.openlocfilehash: b18c9666e58925746a3b61740db6fb5118c2010b
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56733723"
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

La hoja **Web application firewall settings** (Configuración del firewall de aplicaciones web) ofrece la posibilidad de filtrar las reglas a través de una búsqueda de texto. El resultado muestra solo las reglas y los grupos de reglas que contienen el texto que se ha buscado.

![Buscar reglas][2]

## <a name="disable-rule-groups-and-rules"></a>Deshabilitación de reglas y grupos de reglas

Cuando se están deshabilitando las reglas, puede deshabilitar un grupo de reglas completo o reglas específicas de uno o varios grupos de reglas. 

**Para deshabilitar grupos de reglas o reglas concretas**

   1. Busque las reglas o los grupos de reglas que desea deshabilitar.
   2. Desactive las casillas de las reglas que desea deshabilitar. 
   2. Seleccione **Guardar**. 

![Guardar cambios][3]

## <a name="mandatory-rules"></a>Normas obligatorias

En la lista siguiente contiene las condiciones que provocan el WAF bloquear la solicitud en el modo de prevención (en modo de detección, se registran como excepciones). Estos no se pueden configurar o deshabilitados:

* Error al analizar el cuerpo de solicitud tiene como resultado la solicitud está bloqueada, a menos que la inspección de cuerpo está desactivado (XML, JSON, datos de formulario)
* Longitud de datos de solicitud de cuerpo (con ningún archivo) es mayor que el límite configurado
* La solicitud es mayor que el límite de cuerpo (incluidos los archivos)
* Se produjo un error interno en el motor de WAF

CRS 3.x específica:

* Entrada de umbral superado de puntuación de anomalías

## <a name="next-steps"></a>Pasos siguientes

Después de configurar las reglas deshabilitadas, puede aprender a ver los registros de WAFS. Para más información, consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
