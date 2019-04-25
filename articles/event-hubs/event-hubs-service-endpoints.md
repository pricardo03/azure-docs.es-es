---
title: Puntos de conexión de servicio de Virtual Network - Azure Event Hubs | Microsoft Docs
description: En este artículo se proporciona información sobre cómo agregar un punto de conexión de servicio de Microsoft.EventHub a una red virtual.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 03/12/2019
ms.author: shvija
ms.openlocfilehash: 7b5a62f81238d1ae2b627c395613066350b36efe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60343450"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Usar puntos de conexión de servicio de Virtual Network con Azure Event Hubs

La integración de Event Hubs con los [puntos de conexión de servicio de Virtual Network (VNet)][vnet-sep] permite el acceso seguro a las funcionalidades de mensajería de cargas de trabajo tales como las máquinas virtuales que están enlazadas a redes virtuales, con una ruta de acceso del tráfico de red que está protegida en ambos extremos.

Una vez configurado que va a enlazar al menos un extremo de servicio de subred de red virtual, lo respectivo Event Hubs espacio de nombres ya no acepta el tráfico desde cualquier lugar pero autorizado de las subredes de redes virtuales. Desde la perspectiva de la red virtual, el enlace de un espacio de nombres de Event Hubs a un punto de conexión de servicio configura un túnel de redes aislado desde la subred de la red virtual al servicio de mensajería. 

El resultado es una relación privada y aislada entre las cargas de trabajo enlazadas a la subred y el espacio de nombres respectivo de Event Hubs, a pesar de que la dirección de red que se puede observar en el punto de conexión de servicio de mensajería esté en un intervalo IP público. Hay una excepción a este comportamiento. Habilitación de un extremo de servicio, de forma predeterminada, habilita la regla de denyall en el firewall IP asociada con la red virtual. Puede agregar direcciones IP específicas en el firewall para permitir el acceso al punto de conexión público de centro de eventos. 


>[!WARNING]
> La implementación de la integración de instancias de Virtual Network puede evitar que otros servicios de Azure interactúen con Event Hubs.
>
> Los servicios de confianza de Microsoft no se admiten cuando se implementan instancias de Virtual Network.
>
> Estos son los escenarios comunes de Azure que no funcionan con instancias de Virtual Network (tenga en cuenta que la lista **NO** está completa).
> - Azure Monitor
> - Azure Stream Analytics
> - Integración con Azure Event Grid
> - Enrutamientos de Azure IoT Hub
> - Azure IoT Device Explorer
> - Explorador de datos de Azure
>
> Los siguientes servicios de Microsoft deben estar en una red virtual
> - Azure Web Apps
> - Azure Functions

> [!IMPORTANT]
> Las redes virtuales se admiten en los niveles **estándar** y **dedicado** de Event Hubs. No se admiten en el nivel básico.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Escenarios de seguridad avanzados que habilita la integración de VNet 

Las soluciones que requieren seguridad estricta y compartimentada y donde subredes de redes virtuales proporcionan la segmentación entre los servicios compartimentadas, seguirá necesitan rutas de comunicación entre los servicios que residen en los compartimientos.

Cualquier ruta IP inmediata entre compartimientos, incluidos aquellos que transportan HTTPS a través de TCP/IP, tienen el riesgo de sufrir ataques por puntos vulnerables en la capa de red o en capas superiores. Los servicios de mensajería proporcionan rutas de comunicación completamente aisladas, donde los mensajes incluso se escriben en discos cuando se trasladan entre entidades. Las cargas de trabajo de dos redes virtuales distintas enlazadas a la misma instancia de Event Hubs pueden comunicarse de forma eficaz y fiable mediante mensajes, al tiempo que se preserva la integridad de los respectivos límites de aislamiento de red.
 
Esto significa que sus soluciones confidenciales en la nube no solo obtienen acceso a las funcionalidades de mensajería asíncrona de Azure (líderes del sector en fiabilidad y escalabilidad), sino que también pueden usar la mensajería para crear rutas de comunicación entre compartimentos seguros de la solución, que son intrínsecamente más seguros de lo que se puede lograr con cualquier otro modo de comunicación entre iguales, incluyendo en protocolo HTTPS y los protocolos de socket protegidos por TLS.

## <a name="bind-event-hubs-to-virtual-networks"></a>Enlazar Event Hubs a redes virtuales

Las *reglas de red virtual* son una característica de firewall que controla si el espacio de nombres de Azure Event Hubs acepta las conexiones de una subred determinada de una red virtual.

Enlazar un espacio de nombres de Event Hubs a una red virtual es un proceso de dos pasos. Primero debe crear un **punto de conexión de servicio de Virtual Network** en una subred de Virtual Network y habilitarlo para "Microsoft.EventHub", tal como se explicó en la [introducción a los puntos de conexión de servicio][vnet-sep]. Una vez que haya agregado el punto de conexión de servicio, enlácelo con el espacio de nombres de Event Hubs con una *regla de red virtual*.

La regla de red virtual es una asociación del espacio de nombres de Event Hubs con una subred de red virtual. Mientras exista la regla, se les concederá acceso a todas las cargas de trabajo que estén enlazadas a la subred, al espacio de nombres de Event Hubs. Event Hubs no establece nunca por sí mismo conexiones de salida, no necesita obtener acceso y, por tanto, nunca se le concede acceso a la subred habilitando esta regla.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Crear una regla de red virtual con plantillas de Azure Resource Manager

La siguiente plantilla de Resource Manager permite agregar una regla de red virtual a un espacio de nombres de Event Hubs.

Parámetros de plantilla:

* **namespaceName**: espacio de nombres de Event Hubs.
* **vnetRuleName**: nombre de la regla de Virtual Network que se va a crear.
* **virtualNetworkingSubnetId**: ruta de acceso completa de Resource Manager para la subred de la red virtual; por ejemplo, `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` para la subred predeterminada de una red virtual.

> [!NOTE]
> Si bien no hay reglas de denegación posibles, la plantilla de Azure Resource Manager tiene la acción predeterminada establecida en **"Permitir"**, lo que no restringe las conexiones.
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
      "virtualNetworkName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Rule"
        }
      },
      "subnetName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Sub Net"
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
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
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
        "apiVersion": "2017-09-01",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "type": "Microsoft.Network/virtualNetworks",
        "properties": {
          "addressSpace": {
            "addressPrefixes": [
              "10.0.0.0/23"
            ]
          },
          "subnets": [
            {
              "name": "[parameters('subnetName')]",
              "properties": {
                "addressPrefix": "10.0.0.0/23",
                "serviceEndpoints": [
                  {
                    "service": "Microsoft.EventHub"
                  }
                ]
              }
            }
          ]
        }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": 
          [
            {
              "subnet": {
                "id": "[variables('subNetId')]"
              },
              "ignoreMissingVnetServiceEndpoint": false
            }
          ],
          "ipRules":[<YOUR EXISTING IP RULES>],
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Para implementar la plantilla, siga las instrucciones relativas a [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de las redes virtuales, consulte los siguientes vínculos:

- [Puntos de conexión del servicio Azure Virtual Network][vnet-sep]
- [Filtrado de IP de Azure Event Hubs][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md
