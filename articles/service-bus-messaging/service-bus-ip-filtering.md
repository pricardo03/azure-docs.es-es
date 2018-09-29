---
title: Filtros de conexión de IP de Azure Service Bus | Microsoft Docs
description: Se describe cómo usar el filtrado IP para bloquear las conexiones desde direcciones IP específicas a Azure Service Bus.
services: service-bus
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: clemensv
ms.openlocfilehash: ccb759a9151d734aa99a6f6b9c68e6072874dd84
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394835"
---
# <a name="use-ip-filters"></a>Uso de filtros IP

En los escenarios en los que solo se puede acceder a Azure Service Bus desde sitios conocidos, la característica *filtro IP* le permite configurar reglas para rechazar o aceptar el tráfico que se origina desde direcciones IPv4 específicas. Por ejemplo, estas direcciones pueden ser las de una puerta de enlace NAT corporativa.

## <a name="when-to-use"></a>Cuándo se deben usar

Hay dos casos de uso específicos en los que resulta útil bloquear los puntos de conexión de Service Bus para determinadas direcciones IP:

- Service Bus debe recibir tráfico solo de un intervalo concreto de direcciones IP y rechazar todas las demás. Por ejemplo, usará Service Bus con [Azure ExpressRoute][express-route] para crear conexiones privadas a la infraestructura local.
- Deberá rechazar el tráfico de direcciones IP que el administrador de Service Bus haya identificado como sospechosas.

## <a name="how-filter-rules-are-applied"></a>Cómo se aplican las reglas de filtro

Las reglas de filtro IP se aplican en el nivel de espacio de nombres de Service Bus. Por lo tanto, las reglas se aplican a todas las conexiones de clientes que usan cualquier protocolo admitido.

Cualquier intento de conexión desde una dirección IP que coincida con una regla IP de rechazo en el espacio de nombres de Service Bus se rechaza como no autorizado. La respuesta no menciona la regla IP.

## <a name="default-setting"></a>Configuración predeterminada

De forma predeterminada, la cuadrícula de **filtro IP** del portal para Service Bus está vacía. Esta configuración predeterminada significa que el espacio de nombres acepta la conexión de cualquier dirección IP. Esta configuración predeterminada es equivalente a una regla que acepta el intervalo de direcciones IP 0.0.0.0/0.

## <a name="ip-filter-rule-evaluation"></a>Evaluación de las reglas de filtro IP

Las reglas de filtro IP se aplican en orden y la primera regla que coincida con la dirección IP determina la acción de aceptar o rechazar.

Por ejemplo, si desea aceptar direcciones del intervalo 70.37.104.0/24 y rechazar todas las demás, la primera regla de la cuadrícula debe aceptar el intervalo de direcciones 70.37.104.0/24. La siguiente regla debe rechazar todas las direcciones mediante el intervalo 0.0.0.0/0.

> [!NOTE]
> Rechazar direcciones IP puede evitar que otros servicios de Azure (por ejemplo, Azure Stream Analytics, Azure Virtual Machines o el Explorador de dispositivos del portal) interactúen con Service Bus.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Creación de una regla de red virtual con plantillas de Azure Resource Manager

La siguiente plantilla de Resource Manager permite agregar una regla de red virtual a un espacio de nombres de Service Bus.

Parámetros de plantilla:

- **ipFilterRuleName** debe ser una cadena exclusiva de hasta 128 caracteres alfanuméricos que no distinga mayúsculas de minúsculas.
- **ipFilterAction** es la acción **Rechazar** o **Aceptar** que se aplica a la regla de filtro IP.
- **ipMask** es una única dirección IPv4 o un bloque de direcciones IP en la notación CIDR. Por ejemplo, en notación CIDR 70.37.104.0/24 representa las 256 direcciones IPv4 de 70.37.104.0 a 70.37.104.255, donde 24 indica el número de bits de prefijo significativos para el intervalo.

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
          "ipFilterRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "ipFilterAction":{  
             "type":"string",
             "allowedValues": ["Reject", "Accept"],
             "metadata":{  
                "description":"IP Filter Action"
             }
          },
          "IpMask":{  
             "type":"string",
             "metadata":{  
                "description":"IP Mask"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('ipFilterRuleName'))]",
            "type": "Microsoft.ServiceBus/Namespaces/IPFilterRules",
            "properties": {
                "FilterName":"[parameters('ipFilterRuleName')]",
                "Action":"[parameters('ipFilterAction')]",              
                "IpMask": "[parameters('IpMask')]"
            }
        } 
    ]
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