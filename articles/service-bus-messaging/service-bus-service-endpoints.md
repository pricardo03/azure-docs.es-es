---
title: Reglas y puntos de conexión del servicio de Virtual Network y reglas para Azure Service Bus | Microsoft Docs
description: Agregue un punto de conexión de servicio Microsoft.ServiceBus a una red virtual.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: clemensv
ms.openlocfilehash: 05930dfce64378d792213ccaefa3d15057bd5dfd
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405016"
---
# <a name="use-virtual-network-service-endpoints-with-azure-service-bus"></a>Uso de puntos de conexión de servicio de Virtual Network con Azure Service Bus

La integración de Service Bus con los [puntos de conexión de servicio de Virtual Network (VNet)][vnet-sep] permite el acceso seguro a las funcionalidades de mensajería de cargas de trabajo como las de máquinas virtuales que están enlazadas a redes virtuales, con una ruta de acceso del tráfico de red que está protegida en ambos extremos. 

Una vez realizada la configuración para enlazarse con al menos un punto de conexión de servicio de subred de red virtual, el espacio de nombres respectivo de Service Bus solo aceptará ya el tráfico procedente de redes virtuales autorizadas. Desde la perspectiva de la red virtual, el enlace de un espacio de nombres de Service Bus a un punto de conexión de servicio configura un túnel de redes aislado desde la subred de la red virtual al servicio de mensajería.

El resultado es una relación privada y aislada entre las cargas de trabajo enlazadas a la subred y el espacio de nombres respectivo de Service Bus, a pesar de que la dirección de red que se puede observar en el punto de conexión de servicio de mensajería esté en un intervalo IP público.

## <a name="enable-service-endpoints-with-service-bus"></a>Habilitar puntos de conexión de servicio con Service Bus

Solo se admiten redes virtuales en espacios de nombres de Service Bus del [nivel Premium](service-bus-premium-messaging.md). 

Una consideración importante al utilizar los puntos de conexión de servicio de la red virtual con Service Bus es que no debería habilitar estos puntos de conexión en aplicaciones que combinan espacios de nombres de Service Bus de los niveles Estándar y Premium. Dado que el nivel Estándar no admite redes virtuales, el punto de conexión se restringiría solo a los espacios de nombres del nivel Premium. La red virtual bloqueará el tráfico hacia el espacio de nombres estándar. 

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Escenarios de seguridad avanzados que habilita la integración de VNet 

En el caso de soluciones que requieren una estricta seguridad compartimentalizada y en las que las subredes de redes virtuales proporcionan la segmentación entre los servicios compartimentalizados, estas siguen necesitando, por lo general, rutas de comunicación entre los servicios que residen en esos compartimientos.

Cualquier ruta IP inmediata entre compartimientos, incluidos aquellos que transportan HTTPS a través de TCP/IP, tienen el riesgo de sufrir ataques por puntos vulnerables en la capa de red o en capas superiores. Los servicios de mensajería proporcionan rutas de comunicación completamente aisladas, donde los mensajes incluso se escriben en discos cuando se trasladan entre entidades. Las cargas de trabajo de dos redes virtuales distintas enlazadas a la misma instancia de Service Bus pueden comunicarse de forma eficaz y confiable mediante mensajes, al tiempo que se preserva la integridad de los respectivos límites de aislamiento de red.
 
Esto significa que sus soluciones confidenciales en la nube no solo obtienen acceso a las funcionalidades de mensajería asíncrona líderes del sector en fiabilidad y escalabilidad, sino que también pueden usar ahora la mensajería para crear rutas de comunicación entre compartimientos seguros de la solución que son intrínsecamente más seguros de lo que se puede lograr con cualquier otro modo de comunicación entre iguales, incluido HTTPS y los protocolos de socket protegidos por TLS.

## <a name="binding-service-bus-to-virtual-networks"></a>Enlace de Service Bus a Virtual Networks

Las *reglas de red virtual* son una característica de firewall que controla si el servidor de Azure Service Bus acepta las conexiones de una subred determinada de una red virtual.

Enlazar un espacio de nombres de Service Bus a una red virtual es un proceso de dos pasos. Primero debe crear un **punto de conexión de servicio de Virtual Network** en una subred de Virtual Network y habilitarlo para "Microsoft.ServiceBus" como se explicó en la [introducción a los puntos de conexión de servicio][vnet-sep]. Una vez que ha agregado el punto de conexión de servicio, enlácelo con el espacio de nombres de Service Bus con una *regla de red virtual*.

La regla de red virtual es una asociación con nombre del espacio de nombres de Service Bus con una subred de red virtual. Mientras exista la regla, se les concederá acceso a todas las cargas de trabajo enlazadas a la subred al espacio de nombres de Service Bus. Service Bus no establece nunca por sí mismo conexiones de salida, no necesita obtener acceso y, por tanto, nunca se le concede acceso a la subred habilitando esta regla.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Creación de una regla de red virtual con plantillas de Azure Resource Manager

La siguiente plantilla de Resource Manager permite agregar una regla de red virtual a un espacio de nombres de Service Bus.

Parámetros de plantilla:

* **namespaceName**: espacio de nombres de Service Bus.
* **vnetRuleName**: nombre de la regla de red virtual que se va a crear.
* **virtualNetworkingSubnetId**: ruta completa de Resource Manager para la subred de la red virtual, por ejemplo, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` para la subred predeterminada de una red virtual.

Plantilla:

```json
{  
   "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{     
          "namespaceName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the namespace"
             }
          },
          "vnetRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "virtualNetworkSubnetId":{  
             "type":"string",
             "metadata":{  
                "description":"subnet Azure Resource Manager ID"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('vnetRuleName'))]",
            "type":"Microsoft.ServiceBus/namespaces/VirtualNetworkRules",           
            "properties": {             
                "virtualNetworkSubnetId": "[parameters('virtualNetworkSubnetId')]"  
            }
        } 
    ]
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