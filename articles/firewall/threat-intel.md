---
title: Filtrado basado en inteligencia de amenazas de Firewall de Azure
description: Obtenga información sobre el filtrado de inteligencia de amenazas de Firewall de Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: 4ef9089c94d9e806cc519c4f8243cdcb7e73953a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60194060"
---
# <a name="azure-firewall-threat-intelligence-based-filtering---public-preview"></a>Filtrado de amenazas de Firewall de Azure basado en inteligencia - versión preliminar pública

El filtrado basado en inteligencia sobre amenazas puede habilitarse para que el firewall alerte y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) alimenta la inteligencia de amenazas de Microsoft y se usa en varios servicios, incluidos Azure Security Center.

![Inteligencia de amenazas de Firewall](media/threat-intel/firewall-threat.png)

> [!IMPORTANT]
> Filtrado basado en inteligencia de amenazas está actualmente en versión preliminar pública y se proporcionan con un acuerdo de nivel de servicio de vista previa. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.  Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si está habilitado el filtrado basado en inteligencia de amenazas, las reglas asociadas se procesan antes que cualquiera de las reglas NAT, reglas de red o las reglas de aplicación. Durante la versión preliminar, se incluyen solo los registros de confianza más altos.

Puede elegir registrar solo una alerta cuando se desencadena una regla, o puede elegir la alerta y denegar el modo.

De forma predeterminada, el filtrado basado en inteligencia de amenazas está habilitado en el modo de alerta. No se puede desactivar esta característica o cambiar el modo hasta que la interfaz del portal está disponible en su región.

![Inteligencia de amenazas en función de filtrado interfaz del portal](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>Registros

El siguiente extracto del registro muestra una regla activada:

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

- **Las pruebas saliente** -alertas de tráfico de salida deben ser un caso poco habitual, ya que significa que se ha puesto en peligro su entorno. Para ayudar a las alertas de salida de prueba trabaja, una prueba se ha creado el FQDN que desencadena una alerta. Use **testmaliciousdomain.eastus.cloudapp.azure.com** para las pruebas de salida.

- **Las pruebas de entrada** : puede esperar a ver las alertas en el tráfico entrante, si se configuran reglas DNAT en el firewall. Esto es cierto incluso si se permiten orígenes específicos solo en la regla DNAT y en caso contrario, se deniega el tráfico. Firewall de Azure no de alerta en todos los escáneres de puerto conocido; solo en analizadores que se conocen también participar en actividades malintencionadas.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [ejemplos de Firewall de Azure Log Analytics](log-analytics-samples.md)
- Obtenga información sobre cómo [implementar y configurar un Firewall de Azure](tutorial-firewall-deploy-portal.md)
- Revise el [informe de inteligencia de Microsoft Security](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)