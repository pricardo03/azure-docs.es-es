---
title: Filtrado basado en inteligencia sobre amenazas de Azure Firewall
description: Obtenga información acerca del filtrado basado en inteligencia sobre amenazas de Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: 4ef9089c94d9e806cc519c4f8243cdcb7e73953a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60194060"
---
# <a name="azure-firewall-threat-intelligence-based-filtering---public-preview"></a>Filtrado basado en inteligencia sobre amenazas de Azure Firewall: versión preliminar pública

El filtrado basado en inteligencia sobre amenazas puede habilitarse para que el firewall alerte y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) impulsa la inteligencia sobre amenazas de Microsoft y lo utilizan numerosos servicios, incluido Azure Security Center.

![Inteligencia sobre amenazas de Firewall](media/threat-intel/firewall-threat.png)

> [!IMPORTANT]
> El filtrado basado en inteligencia sobre amenazas se encuentra actualmente en versión preliminar pública y se proporciona con un acuerdo de nivel de servicio de versión preliminar. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.  Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si el filtrado basado en inteligencia sobre amenazas está habilitado, las reglas asociadas se procesan antes que cualquiera de las reglas NAT, reglas de red o reglas de aplicación. Durante la versión preliminar, solo se incluyen los registros de máxima confianza.

Puede optar por registrar solo una alerta cuando se desencadena una regla o puede elegir el modo de alerta y denegación.

De forma predeterminada, el filtrado basado en inteligencia sobre amenazas está habilitado en el modo de alerta. No se puede desactivar esta característica o cambiar el modo hasta que la interfaz del portal esté disponible en su región.

![Interfaz del portal del filtrado basado en inteligencia sobre amenazas](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>Registros

El siguiente extracto del registro muestra una regla desencadenada:

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Prueba

- **Pruebas de salida**: las alertas de tráfico de salida deben ser un caso poco habitual, ya que significa que se ha puesto en peligro su entorno. Para ayudar a probar que las alertas de salida funcionan, se ha creado un FQDN de prueba que desencadena una alerta. Use **testmaliciousdomain.eastus.cloudapp.azure.com** para las pruebas de salida.

- **Pruebas de entrada**: puede esperar ver las alertas en el tráfico de entrada si se configuran reglas DNAT en el firewall. Esto es cierto incluso si solo se permiten orígenes específicos en la regla DNAT y se deniega el tráfico de otra manera. Azure Firewall no alerta sobre todos los escáneres de puerto conocidos, solo sobre escáneres que se sabe que también participan en actividades malintencionadas.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Ejemplos de Log Analytics de Azure Firewall](log-analytics-samples.md)
- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md)
- Revise el [informe de inteligencia de seguridad de Microsoft](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)