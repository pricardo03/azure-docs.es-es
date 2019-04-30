---
title: Reglas de firewall de Azure Service Bus | Microsoft Docs
description: Uso de las reglas de firewall para permitir las conexiones desde direcciones IP específicas a Azure Service Bus.
services: service-bus
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 04/23/2019
ms.author: aschhab
ms.openlocfilehash: 540435e3e018ae77477030ae8b9f727d71782121
ms.sourcegitcommit: a95dcd3363d451bfbfea7ec1de6813cad86a36bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62736818"
---
# <a name="use-firewall-rules"></a>Uso de las reglas de firewall

En los escenarios en los que solo se puede acceder a Azure Service Bus desde sitios conocidos, las reglas de firewall le permiten configurar reglas para aceptar el tráfico que se origina desde direcciones IPv4 específicas. Por ejemplo, estas direcciones pueden ser las de una puerta de enlace NAT corporativa.

## <a name="when-to-use"></a>Cuándo se deben usar

Si desea configurar Service Bus de forma que reciba tráfico solo de un rango especificado de direcciones IP y rechace todo lo demás, puede aprovechar un *Firewall* para bloquear los puntos de conexión de Service Bus desde otras direcciones IP. Por ejemplo, usará Service Bus con [Azure ExpressRoute][express-route] para crear conexiones privadas a la infraestructura local. 

## <a name="how-filter-rules-are-applied"></a>Cómo se aplican las reglas de filtro

Las reglas de filtro IP se aplican en el nivel de espacio de nombres de Service Bus. Por lo tanto, las reglas se aplican a todas las conexiones de clientes que usan cualquier protocolo admitido.

Cualquier intento de conexión desde una dirección IP que no coincida con una regla IP admitida en el espacio de nombres de Service Bus se rechaza como no autorizado. La respuesta no menciona la regla IP.

## <a name="default-setting"></a>Configuración predeterminada

De forma predeterminada, la cuadrícula de **filtro IP** del portal para Service Bus está vacía. Esta configuración predeterminada significa que el espacio de nombres acepta la conexión de cualquier dirección IP. Esta configuración predeterminada es equivalente a una regla que acepta el intervalo de direcciones IP 0.0.0.0/0.

## <a name="ip-filter-rule-evaluation"></a>Evaluación de las reglas de filtro IP

Las reglas de filtro IP se aplican en orden y la primera regla que coincida con la dirección IP determina la acción de aceptar o rechazar.

>[!WARNING]
> La implementación de reglas de firewall puede evitar que otros servicios de Azure interactúen con Service Bus.
>
> Los servicios de confianza de Microsoft no se admiten cuando se implementa el filtro de IP (reglas de firewall), pero estarán disponibles muy pronto.
>
> Estos son los escenarios comunes de Azure que no funcionan con el filtro de IP (tenga en cuenta que la lista **NO** está completa).
> - Azure Monitor
> - Azure Stream Analytics
> - Integración con Azure Event Grid
> - Enrutamientos de Azure IoT Hub
> - Azure IoT Device Explorer
> - Explorador de datos de Azure
>
> Los siguientes servicios de Microsoft deben estar en una red virtual
> - Azure App Service
> - Azure Functions

### <a name="creating-a-virtual-network-and-firewall-rule-with-azure-resource-manager-templates"></a>Creación de una regla de red virtual y firewall con plantillas de Azure Resource Manager

> [!IMPORTANT]
> Firewalls y redes virtuales solo se admiten en el **premium** nivel de Service Bus.

La siguiente plantilla de Resource Manager permite agregar una regla de red virtual a un espacio de nombres de Service Bus.

Parámetros de plantilla:

- **ipMask** es una única dirección IPv4 o un bloque de direcciones IP en la notación CIDR. Por ejemplo, en notación CIDR 70.37.104.0/24 representa las 256 direcciones IPv4 de 70.37.104.0 a 70.37.104.255, donde 24 indica el número de bits de prefijo significativos para el intervalo.

> [!NOTE]
> Si bien no hay reglas de denegación posibles, la plantilla de Azure Resource Manager tiene la acción predeterminada establecida en **"Permitir"**, que no restringe las conexiones.
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
      "servicebusNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Service Bus namespace"
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
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('servicebusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Premium",
          "tier": "Premium"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
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

Para implementar la plantilla, siga las instrucciones relativas a [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Pasos siguientes

Para restringir el acceso a Service Bus a redes virtuales de Azure, visite el siguiente vínculo:

- [Puntos de conexión de servicio de red virtual para Service Bus][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
