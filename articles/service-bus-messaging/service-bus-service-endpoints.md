---
title: Reglas y puntos de conexión del servicio de Virtual Network y reglas para Azure Service Bus | Microsoft Docs
description: Agregue un punto de conexión de servicio Microsoft.ServiceBus a una red virtual.
services: service-bus
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: aschhab
ms.openlocfilehash: 8a5832433d1d8c81c22c828ec4659c10951f680a
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103639"
---
# <a name="use-virtual-network-service-endpoints-with-azure-service-bus"></a>Uso de puntos de conexión de servicio de Virtual Network con Azure Service Bus

La integración de Service Bus con los [puntos de conexión de servicio de Virtual Network (VNet)][vnet-sep] permite el acceso seguro a las funcionalidades de mensajería de cargas de trabajo como las de máquinas virtuales que están enlazadas a redes virtuales, con una ruta de acceso del tráfico de red que está protegida en ambos extremos.

Una vez realizada la configuración para enlazarse con al menos un punto de conexión de servicio de subred de red virtual, el espacio de nombres respectivo de Service Bus solo aceptará ya el tráfico procedente de redes virtuales autorizadas. Desde la perspectiva de la red virtual, el enlace de un espacio de nombres de Service Bus a un punto de conexión de servicio configura un túnel de redes aislado desde la subred de la red virtual al servicio de mensajería.

El resultado es una relación privada y aislada entre las cargas de trabajo enlazadas a la subred y el espacio de nombres respectivo de Service Bus, a pesar de que la dirección de red que se puede observar en el punto de conexión de servicio de mensajería esté en un intervalo IP público.

>[!WARNING]
> La implementación de la integración de instancias de Virtual Network puede evitar que otros servicios de Azure interactúen con Service Bus.
>
> Los servicios de confianza de Microsoft no se admiten cuando se implementan las instancias de Virtual Network, pero estarán disponibles muy pronto.
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
> - Azure App Service
> - Azure Functions

> [!IMPORTANT]
> Solo se admiten redes virtuales en espacios de nombres de Service Bus del [nivel Premium](service-bus-premium-messaging.md).

## <a name="enable-service-endpoints-with-service-bus"></a>Habilitar puntos de conexión de servicio con Service Bus

Una consideración importante al utilizar los puntos de conexión de servicio de la red virtual con Service Bus es que no debería habilitar estos puntos de conexión en aplicaciones que combinan espacios de nombres de Service Bus de los niveles Estándar y Premium. Dado que el nivel Estándar no admite redes virtuales, el punto de conexión se restringiría solo a los espacios de nombres del nivel Premium. La red virtual bloqueará el tráfico hacia el espacio de nombres estándar.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Escenarios de seguridad avanzados que habilita la integración de VNet 

En el caso de soluciones que requieren una estricta seguridad compartimentalizada y en las que las subredes de redes virtuales proporcionan la segmentación entre los servicios compartimentalizados, estas siguen necesitando, por lo general, rutas de comunicación entre los servicios que residen en esos compartimientos.

Cualquier ruta IP inmediata entre compartimientos, incluidos aquellos que transportan HTTPS a través de TCP/IP, tienen el riesgo de sufrir ataques por puntos vulnerables en la capa de red o en capas superiores. Los servicios de mensajería proporcionan rutas de comunicación completamente aisladas, donde los mensajes incluso se escriben en discos cuando se trasladan entre entidades. Las cargas de trabajo de dos redes virtuales distintas enlazadas a la misma instancia de Service Bus pueden comunicarse de forma eficaz y confiable mediante mensajes, al tiempo que se preserva la integridad de los respectivos límites de aislamiento de red.
 
Esto significa que sus soluciones confidenciales en la nube no solo obtienen acceso a las funcionalidades de mensajería asíncrona líderes del sector en fiabilidad y escalabilidad, sino que también pueden usar ahora la mensajería para crear rutas de comunicación entre compartimientos seguros de la solución que son intrínsecamente más seguros de lo que se puede lograr con cualquier otro modo de comunicación entre iguales, incluido HTTPS y los protocolos de socket protegidos por TLS.

## <a name="binding-service-bus-to-virtual-networks"></a>Enlace de Service Bus a Virtual Networks

Las *reglas de red virtual* son una característica de firewall que controla si el servidor de Azure Service Bus acepta las conexiones de una subred determinada de una red virtual.

Enlazar un espacio de nombres de Service Bus a una red virtual es un proceso de dos pasos. Primero debe crear un **punto de conexión de servicio de Virtual Network** en una subred de Virtual Network y habilitarlo para "Microsoft.ServiceBus" como se explicó en la [introducción a los puntos de conexión de servicio][vnet-sep]. Una vez que ha agregado el punto de conexión de servicio, enlácelo con el espacio de nombres de Service Bus con una *regla de red virtual*.

La regla de red virtual es una asociación del espacio de nombres de Service Bus con una subred de red virtual. Mientras exista la regla, se les concederá acceso a todas las cargas de trabajo enlazadas a la subred al espacio de nombres de Service Bus. Service Bus no establece nunca por sí mismo conexiones de salida, no necesita obtener acceso y, por tanto, nunca se le concede acceso a la subred habilitando esta regla.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Creación de una regla de red virtual con plantillas de Azure Resource Manager

La siguiente plantilla de Resource Manager permite agregar una regla de red virtual a un espacio de nombres de Service Bus.

Parámetros de plantilla:

* **namespaceName**: Espacio de nombres de Service Bus.
* **virtualNetworkingSubnetId**: ruta de acceso completa de Resource Manager para la subred de la red virtual; por ejemplo, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` para la subred predeterminada de una red virtual.

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

Plantilla:

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
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
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
                    "service": "Microsoft.ServiceBus"
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
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
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
- [Filtrado de IP de Azure Service Bus][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: service-bus-ip-filtering.md
