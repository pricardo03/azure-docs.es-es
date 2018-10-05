---
title: Restablecimiento de TCP inactivo en Load Balancer | Microsoft Docs
description: Load Balancer con paquetes RST de TCP bidireccionales al agotarse el tiempo de espera de inactividad
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: kumud
ms.openlocfilehash: 0b533a48e94db880f23d42decc5c3fb39a27e5ac
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395651"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Load Balancer con restablecimiento de TCP inactivo (versión preliminar pública)

Puede usar [Standard Load Balancer](load-balancer-standard-overview.md) si desea crear un comportamiento de aplicación más predecible para los escenarios con restablecimientos de TCP bidireccionales (paquete RST de TCP) para cada tiempo de espera de inactividad configurable.  El comportamiento predeterminado de Load Balancer es descartar silenciosamente los flujos cuando se alcanza el tiempo de inactividad de un flujo.

![Restablecimiento de TCP en Load Balancer](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>Load Balancer con la funcionalidad de restablecimiento de TCP al agotarse el tiempo de espera de inactividad está disponible como versión preliminar pública en este momento y disponible en un conjunto limitado de [regiones](#regions). Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
Puede cambiar este comportamiento predeterminado y habilitar el envío de restablecimientos de TCP al agotarse el tiempo de espera de inactividad en las reglas NAT de entrada, en las reglas de equilibrio de carga y en las [reglas de salida](https://aka.ms/lboutboundrules).  Cuando se habilita en cada regla, Load Balancer envía restablecimientos TCP bidireccionales (paquetes RST de TCP) tanto a los puntos de conexión del cliente como a los del servidor en el momento en que se agota el tiempo de espera de inactividad para todos los flujos que correspondan.

Los puntos de conexión que reciben los paquetes RST de TCP cierran inmediatamente el socket correspondiente. De este modo, se proporciona una notificación inmediata a los puntos de conexión para indicar que se ha liberado la conexión y cualquier comunicación posterior en la misma conexión TCP generará un error.  Las aplicaciones pueden purgar las conexiones cuando el socket se cierra y restablecer las conexiones según sea necesario sin tener que esperar que la conexión TCP, finalmente, agote el tiempo de espera.

En muchos escenarios, esto puede evitar que haya que enviar paquetes keepalive de TCP (o en la capa de la aplicación) para actualizar el tiempo de inactividad de un flujo. 

Si la duración de la inactividad supera la permitida por la configuración o la aplicación muestra un comportamiento no deseado con los restablecimientos de TCP habilitados, es posible que siga teniendo que usar paquetes keepalive de TCP (o de la capa de la aplicación) para supervisar el estado de las conexiones TCP.  Además, los paquetes keepalive también pueden seguir siendo útiles cuando la conexión atraviesa un proxy en algún lugar de la ruta de acceso, especialmente en el caso de los paquetes keepalive de la capa de la aplicación.  

Examine con cuidado todo el escenario completo para decidir si le beneficia habilitar los restablecimientos de TCP y ajustar el tiempo de espera de inactividad, y si es posible que se requieran pasos adicionales para garantizar el comportamiento deseado de la aplicación.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Habilitación del restablecimiento de TCP al agotarse el tiempo de espera de inactividad

Con la API versión 2018-07-01, puede habilitar el envío de restablecimientos de TCP bidireccionales al agotarse el tiempo de espera de inactividad en cada regla:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="regions"></a> Disponibilidad en regiones

Este parámetro está vigente actualmente en las siguientes regiones.  En las regiones que no aparecen aquí, el parámetro no tiene ningún efecto.

| Region |
|---|
| Sudeste de Asia |
| Oeste de Europa |
| Este de EE. UU. - 2 |
| Norte de Reino Unido |
| Oeste de EE. UU. |

Esta tabla se actualizará a medida que la versión preliminar se expanda a otras regiones.  

## <a name="limitations"></a>Limitaciones

- [Disponibilidad limitada en las regiones](#regions).
- El portal no se puede usar para configurar o ver el restablecimiento de TCP.  Use plantillas, API REST, Az CLI 2.0 o PowerShell en su lugar.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Standard Load Balancer](load-balancer-standard-overview.md).
- Más información sobre [reglas de salida](load-balancer-outbound-rules-overview.md).
