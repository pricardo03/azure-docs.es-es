---
title: Introducción al enrutamiento de contenido basado en la dirección URL con Azure Application Gateway
description: En esta página se ofrece información general sobre el enrutamiento de contenido basado en la dirección URL de Azure Application Gateway, la configuración de UrlPathMap y la regla PathBasedRouting.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 1/8/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 2f0bd0a20b866d342802014f1034a7ed9d939d53
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792249"
---
# <a name="url-path-based-routing-overview"></a>Información general del enrutamiento basado en URL

El enrutamiento basado en URL le permite enrutar el tráfico a los grupos de servidores back-end en función de las direcciones URL de la solicitud. 

Por ejemplo, puede enrutar las solicitudes de diferentes tipos de contenido a diferentes grupos de servidores back-end.

En el ejemplo siguiente, Application Gateway atiende el tráfico de contoso.com desde tres grupos de servidores back-end: VideoServerPool, ImageServerPool y DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Las solicitudes de http\://contoso.com/video/* se enrutan a VideoServerPool y las de http\://contoso.com/images/* a ImageServerPool. DefaultServerPool se selecciona si ninguno de los patrones de ruta de acceso coincide.

> [!IMPORTANT]
> Las reglas se procesan en el orden en que aparecen en el portal. Es muy recomendable configurar a los agentes de escucha multisitio antes de configurar un agente de escucha básico.  De esta forma se asegura de que el tráfico se enruta al back-end adecuado. Si un agente de escucha básico aparece en primer lugar y coincide con una solicitud entrante, lo procesa ese agente de escucha.

## <a name="urlpathmap-configuration-element"></a>Elemento de configuración UrlPathMap

El elemento UrlPathMap se utiliza para especificar patrones de ruta de acceso para las asignaciones de grupos de servidores back-end. A continuación se muestra el fragmento de código del elemento urlPathMap del archivo de plantilla.

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

### <a name="pathpattern"></a>PathPattern

PathPattern es la lista de patrones de ruta de acceso con los que se buscan coincidencias. Cada uno de ellos debe comenzar con / y el único lugar donde se permite un carácter "*" es al final, después de un carácter "/". La cadena que se suministra al comprobador de rutas de acceso no incluye texto después del primer "?" o "#", y esos caracteres no se permiten. En caso contrario, todos los caracteres admitidos en una dirección URL se permiten en PathPattern.

Los patrones admitidos dependen de si se implementa Application Gateway v1 o v2:

#### <a name="v1"></a>v1

Las reglas de ruta de acceso no distinguen mayúsculas de minúsculas.

|Patrón de ruta de acceso de v1  |¿Se admite?  |
|---------|---------|
|`/images/*`     |Sí|
|`/images*`     |no|
|`/images/*.jpg`     |no|
|`/*.jpg`     |no|
|`/Repos/*/Comments/*`     |no|
|`/CurrentUser/Comments/*`     |Sí|

#### <a name="v2"></a>v2

Las reglas de ruta de acceso distinguen mayúsculas de minúsculas.

|Patrón de ruta de acceso de v2  |¿Se admite?  |
|---------|---------|
|`/images/*`     |Sí|
|`/images*`     |Sí|
|`/images/*.jpg`     |no|
|`/*.jpg`     |no|
|`/Repos/*/Comments/*`     |no|
|`/CurrentUser/Comments/*`     |Sí|

Para obtener más información, puede consultar una [plantilla de Resource Manager que use el enrutamiento basado en URL](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) .

## <a name="pathbasedrouting-rule"></a>Regla de PathBasedRouting

RequestRoutingRule de tipo PathBasedRouting se usa para enlazar un agente de escucha a un elemento urlPathMap. Todas las solicitudes que se reciben para este agente de escucha se enrutan según la directiva especificada en urlPathMap.
Fragmento de código de la regla PathBasedRouting:

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que conoce el enrutamiento de contenido basado en URL, vaya a [Create an application gateway using URL based routing](application-gateway-create-url-route-portal.md) (Creación de una puerta de enlace de aplicaciones mediante el enrutamiento basado en URL) para crear una puerta de enlace de aplicaciones con reglas de enrutamiento de direcciones URL.
