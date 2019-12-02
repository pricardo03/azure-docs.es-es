---
title: Reglas de firewall de Azure Event Hubs | Microsoft Docs
description: Use las reglas de firewall para permitir las conexiones desde direcciones IP específicas a Azure Event Hubs.
services: event-hubs
documentationcenter: ''
author: spelluru
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.custom: seodec18
ms.topic: article
ms.date: 12/06/2018
ms.author: spelluru
ms.openlocfilehash: 2350586501fae84726aa2aa2438ea676b90c1dbb
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279682"
---
# <a name="use-firewall-rules"></a>Usar las reglas de firewall

En los escenarios en los que solo se puede acceder a Azure Event Hubs desde sitios conocidos, las reglas de firewall le permiten configurar reglas para aceptar el tráfico que se origina desde direcciones IPv4 específicas. Por ejemplo, estas direcciones pueden ser las de una puerta de enlace NAT corporativa.

## <a name="when-to-use"></a>Cuándo se deben usar

Si desea configurar el espacio de nombres Event Hubs de forma que solamente reciba tráfico de un determinado intervalo de direcciones IP y rechace todo lo demás, puede utilizar una *regla de firewall* para bloquear los puntos de conexión de Event Hubs de otras direcciones IP. Por ejemplo, si usa Event Hubs con [Azure ExpressRoute][express-route], puede crear una *regla de firewall* para restringir el tráfico procedente de las direcciones IP de la infraestructura local.

## <a name="how-filter-rules-are-applied"></a>Cómo se aplican las reglas de filtro

Las reglas de filtro IP se aplican en el nivel de espacio de nombres de Event Hubs. Por lo tanto, las reglas se aplican a todas las conexiones de clientes que usan cualquier protocolo admitido.

Cualquier intento de conexión desde una dirección IP que no coincida con una regla IP admitida en el espacio de nombres de Event Hubs se rechaza como no autorizado. La respuesta no menciona la regla IP.

## <a name="default-setting"></a>Configuración predeterminada

De forma predeterminada, la cuadrícula de **filtro IP** del portal para Event Hubs está vacía. Esta configuración predeterminada significa que el centro de eventos acepta la conexión de cualquier dirección IP. Esta configuración predeterminada es equivalente a una regla que acepta el intervalo de direcciones IP 0.0.0.0/0.

## <a name="ip-filter-rule-evaluation"></a>Evaluación de las reglas de filtro IP

Las reglas de filtro IP se aplican en orden y la primera regla que coincida con la dirección IP determina la acción de aceptar o rechazar.

>[!WARNING]
> La implementación de firewalls puede evitar que otros servicios de Azure interactúen con Event Hubs.
>
> Los servicios de confianza de Microsoft no se admiten cuando se implementa el filtro de IP (firewalls), pero estarán disponibles muy pronto.
>
> Estos son los escenarios comunes de Azure que no funcionan con el filtro de IP (tenga en cuenta que la lista **NO** está completa).
> - Azure Stream Analytics
> - Integración con Azure Event Grid
> - Enrutamientos de Azure IoT Hub
> - Azure IoT Device Explorer
>
> Los siguientes servicios de Microsoft deben estar en una red virtual
> - Azure Web Apps
> - Azure Functions

### <a name="creating-a-firewall-rule-with-azure-resource-manager-templates"></a>Creación de una regla de firewall con plantillas de Azure Resource Manager

> [!IMPORTANT]
> Las reglas de firewall se admiten en los niveles **estándar** y **dedicado** de Event Hubs. No se admiten en el nivel básico.

La siguiente plantilla de Resource Manager permite agregar una regla de filtro de IP a un espacio de nombres de Event Hubs.

Parámetros de plantilla:

- **ipMask** es una única dirección IPv4 o un bloque de direcciones IP en la notación CIDR. Por ejemplo, en notación CIDR 70.37.104.0/24 representa las 256 direcciones IPv4 de 70.37.104.0 a 70.37.104.255, donde 24 indica el número de bits de prefijo significativos para el intervalo.

> [!NOTE]
> Si bien no hay reglas de denegación posibles, la plantilla de Azure Resource Manager tiene la acción predeterminada establecida en **"Permitir"** , que no restringe las conexiones.
> Cuando se realizan las reglas de Virtual Network o de firewall, es necesario cambiar el valor ***"defaultAction"***
> 
> De
> ```json
> "defaultAction": "Allow"
> ```
> to
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Para implementar la plantilla, siga las instrucciones para [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Pasos siguientes

Para restringir el acceso a Event Hubs a las redes virtuales de Azure, visite el siguiente vínculo:

- [Puntos de conexión de servicio de red virtual para Event Hubs][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md
