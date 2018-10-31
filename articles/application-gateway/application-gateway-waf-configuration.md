---
title: Listas de exclusión y límites de tamaño de solicitud del firewall de aplicaciones web en Azure Application Gateway (Azure Portal)
description: En este artículo se proporciona información acerca de la configuración de listas de exclusión y límites de tamaño de solicitud del firewall de aplicaciones web en Application Gateway con Azure Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: victorh
ms.openlocfilehash: 12115770959c3869184f0af78c4feba2fd6f2be4
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49984900"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists-public-preview"></a>Listas de exclusión y límites de tamaño de solicitud del firewall de aplicaciones web (versión preliminar pública)

El firewall de aplicaciones web (WAF) de Azure Application Gateway proporciona protección a las aplicaciones web. En este artículo se describe la configuración de listas de exclusión y límites de tamaño de solicitud del WAF.

> [!IMPORTANT]
> La configuración de listas de exclusión y límites de tamaño de solicitud del WAF está actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="waf-request-size-limits"></a>Límites de tamaño de solicitud del WAF

![Límites de tamaño de solicitud](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

El firewall de aplicaciones web permite a los usuarios configurar límites de tamaño de solicitud dentro de los límites inferior y superior. Hay disponibles dos configuraciones de límites de tamaño:

- El campo de tamaño máximo del cuerpo de la solicitud se especifica en KB y controla el límite de tamaño de la solicitud general, excluyendo cualquier carga de archivo. Este campo puede oscilar entre 1 KB como mínimo y 128 KB como máximo. El valor predeterminado para el tamaño del cuerpo de la solicitud es de 128 KB.
- El campo de límite de carga de archivo se especifica en MB y controla el tamaño máximo de carga de archivo permitido. Este campo puede tener un valor mínimo de 1 MB y un máximo de 500 MB. El valor predeterminado del límite de carga de archivo es 100 MB.

El WAF también ofrece un botón configurable para activar o desactivar la inspección del cuerpo de la solicitud. De forma predeterminada, la inspección del cuerpo de la solicitud está habilitada. Si se desactiva la inspección del cuerpo de la solicitud, el WAF no evalúa el contenido del cuerpo del mensaje HTTP. En tal caso, el WAF continúa aplicando reglas de WAF en URI, cookies y encabezados. Si se desactiva la inspección del cuerpo de la solicitud, el campo de tamaño máximo del cuerpo de la solicitud no es aplicable y no se puede definir. Al desactivar la inspección del cuerpo de la solicitud, se podrán enviar mensajes de más de 128 KB al WAF. Sin embargo, no se inspeccionará el cuerpo del mensaje para detectar vulnerabilidades.

## <a name="waf-exclusion-lists"></a>Listas de exclusión del WAF

![waf-exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

Las listas de exclusión del WAF permiten a los usuarios omitir determinados atributos de solicitud de una evaluación del WAF. Un ejemplo común son los tokens insertados de Active Directory que se usan para campos de contraseña o autenticación. Estos atributos suelen contener caracteres especiales que podrían desencadenar un falso positivo de las reglas de WAF. Una vez que se agrega un atributo a la lista de exclusión del WAF, ninguna regla de WAF configurada y activa lo toma en consideración. Las listas de exclusión tienen un ámbito global.
Puede agregar encabezados de solicitud, cuerpos de la solicitud, cookies de solicitud o argumentos de cadena de consulta de solicitud a las listas de exclusión del WAF. Si el cuerpo tiene datos de formulario o XML/JSON (pares clave-valor), se puede usar el tipo de exclusión del atributo de solicitud.

Puede especificar una coincidencia exacta de atributo de cadena de consulta, cookie, cuerpo o encabezado de solicitud u, opcionalmente, puede especificar coincidencias parciales.

Estos son los operadores de criterios de coincidencia admitidos:

- **Equals** (es igual a): este operador se usa para una coincidencia exacta. Por ejemplo, para seleccionar el encabezado denominado **bearerToken**, utilice el operador de igualdad con el selector definido como **bearerToken**.
- **Starts with** (empieza por): este operador coincide con todos los campos que comienzan por el valor del selector especificado. 
- **Ends with** (termina en): este operador coincide con todos los campos de solicitud que terminan con el valor del selector especificado. 
- **Contains** (contiene): este operador coincide con todos los campos de solicitud que contienen el valor del selector especificado.

En todos los casos, la coincidencia distingue mayúsculas de minúsculas y no se permiten expresiones regulares como selectores.

## <a name="next-steps"></a>Pasos siguientes

Después de configurar el WAF, puede aprender a ver los registros del WAF. Para más información, consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).