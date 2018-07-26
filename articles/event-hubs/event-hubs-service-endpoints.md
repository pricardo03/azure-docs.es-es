---
title: Reglas y puntos de conexión del servicio de Virtual Network para Azure Event Hubs | Microsoft Docs
description: Agregue un punto de conexión de servicio Microsoft.EventHub a una red virtual.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: 3746c4b7d1b53d7522f317fd2e349d31ba77f406
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136345"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Usar puntos de conexión de servicio de Virtual Network con Azure Event Hubs

La integración de Event Hubs con los [puntos de conexión de servicio de Virtual Network (VNet)][vnet-sep] permite el acceso seguro a las funcionalidades de mensajería de cargas de trabajo tales como las máquinas virtuales que están enlazadas a redes virtuales, con una ruta de acceso del tráfico de red que está protegida en ambos extremos. 

Una vez realizada la configuración para enlazarse con al menos un punto de conexión de servicio de subred de red virtual, el espacio de nombres respectivo de Event Hubs ya solo aceptará el tráfico procedente de redes virtuales autorizadas. Desde la perspectiva de la red virtual, el enlace de un espacio de nombres de Event Hubs a un punto de conexión de servicio configura un túnel de redes aislado desde la subred de la red virtual al servicio de mensajería.

El resultado es una relación privada y aislada entre las cargas de trabajo enlazadas a la subred y el espacio de nombres respectivo de Event Hubs, a pesar de que la dirección de red que se puede observar en el punto de conexión de servicio de mensajería esté en un intervalo IP público.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Escenarios de seguridad avanzados que habilita la integración de VNet 

En el caso de soluciones que requieren una estricta seguridad compartimentalizada y en las que las subredes de redes virtuales proporcionan la segmentación entre los servicios compartimentalizados, estas siguen necesitando, por lo general, rutas de comunicación entre los servicios que residen en esos compartimientos.

Cualquier ruta IP inmediata entre compartimientos, incluidos aquellos que transportan HTTPS a través de TCP/IP, tienen el riesgo de sufrir ataques por puntos vulnerables en la capa de red o en capas superiores. Los servicios de mensajería proporcionan rutas de comunicación completamente aisladas, donde los mensajes incluso se escriben en discos cuando se trasladan entre entidades. Las cargas de trabajo de dos redes virtuales distintas enlazadas a la misma instancia de Event Hubs pueden comunicarse de forma eficaz y fiable mediante mensajes, al tiempo que se preserva la integridad de los respectivos límites de aislamiento de red.
 
Esto significa que sus soluciones confidenciales en la nube no solo obtienen acceso a las funcionalidades de mensajería asíncrona de Azure (líderes del sector en fiabilidad y escalabilidad), sino que también pueden usar la mensajería para crear rutas de comunicación entre compartimentos seguros de la solución, que son intrínsecamente más seguros de lo que se puede lograr con cualquier otro modo de comunicación entre iguales, incluyendo en protocolo HTTPS y los protocolos de socket protegidos por TLS.

## <a name="bind-event-hubs-to-virtual-networks"></a>Enlazar Event Hubs a redes virtuales

Las *reglas de red virtual* son una característica de firewall que controla si el servidor de Azure Event Hubs acepta las conexiones de una subred determinada de una red virtual.

Enlazar un espacio de nombres de Event Hubs a una red virtual es un proceso de dos pasos. Primero debe crear un **punto de conexión de servicio de Virtual Network** en una subred de Virtual Network y habilitarlo para "Microsoft.EventHub", tal como se explicó en la [introducción a los puntos de conexión de servicio][vnet-sep]. Una vez que haya agregado el punto de conexión de servicio, enlácelo con el espacio de nombres de Event Hubs con una *regla de red virtual*.

La regla de red virtual es una asociación con nombre del espacio de nombres de Event Hubs con una subred de red virtual. Mientras exista la regla, se les concederá acceso a todas las cargas de trabajo que estén enlazadas a la subred, al espacio de nombres de Event Hubs. Event Hubs no establece nunca por sí mismo conexiones de salida, no necesita obtener acceso y, por tanto, nunca se le concede acceso a la subred habilitando esta regla.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Crear una regla de red virtual con plantillas de Azure Resource Manager

La siguiente plantilla de Resource Manager permite agregar una regla de red virtual a un espacio de nombres de Event Hubs.

Parámetros de plantilla:

* **namespaceName**: espacio de nombres de Event Hubs.
* **vnetRuleName**: nombre de la regla de red virtual que se va a crear.
* **virtualNetworkingSubnetId**: ruta completa de Resource Manager para la subred de la red virtual, por ejemplo, `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` para la subred predeterminada de una red virtual.

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
            "type":"Microsoft.EventHub/namespaces/VirtualNetworkRules",         
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
- [Filtrado de IP de Azure Event Hubs][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md