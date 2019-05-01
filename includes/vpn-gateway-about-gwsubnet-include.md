---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0334f9fd2d749b88580ff3857d705de2ae961902
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60407800"
---
La puerta de enlace de red virtual usa una subred concreta llamada la subred de la puerta de enlace. Esta subred forma parte del intervalo de direcciones IP de red virtual que se especifican al configurar una red virtual. Contiene las direcciones IP que usan los recursos y servicios de puerta de enlace de la red virtual. La subred debe llamarse "GatewaySubnet" para que Azure implemente los recursos de la puerta de enlace. No se puede especificar otra subred en la que implementar los recursos de la puerta de enlace. Si no dispone de una subred llamada "GatewaySubnet", al crear la puerta de enlace de VPN, se producirá un error.

Al crear la subred de puerta de enlace, especifique el número de direcciones IP que contiene la subred. El número de direcciones IP que se necesitan depende de la configuración de puerta de enlace de VPN que se desea crear. Algunas configuraciones requieren más direcciones IP que otras. Es aconsejable crear una subred de puerta de enlace que use /27 o /28.

Si ve un error en el que se indica que el espacio de direcciones se solapa con una subred o que la subred no se encuentra dentro del espacio de direcciones de la red virtual, compruebe el intervalo de direcciones de la red virtual. Es posible que no tenga suficientes direcciones IP disponibles en el intervalo de direcciones que creó para la red virtual. Por ejemplo, si la subred predeterminada engloba todo el intervalo de direcciones, no quedan direcciones IP para crear más subredes. Puede ajustar las subredes en el espacio de direcciones existente para liberar direcciones IP o especificar un intervalo de direcciones adicionales y crear en él la subred de la puerta de enlace.