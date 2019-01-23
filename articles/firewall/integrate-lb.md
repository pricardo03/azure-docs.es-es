---
title: Integración de Azure Firewall con Azure Standard Load Balancer
description: Información sobre la integración de Azure Firewall con Azure Standard Load Balancer
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 1/14/2019
ms.author: victorh
ms.openlocfilehash: 079790952263ae2ef68abc8e426b0330fef1c53f
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321779"
---
# <a name="integrate-azure-firewall-with-azure-standard-load-balancer"></a>Integración de Azure Firewall con Azure Standard Load Balancer

Puede integrar una instancia de Azure Firewall en una red virtual con una instancia de Azure Standard Load Balancer (pública o interna). Sin embargo, debe tener en cuenta un problema de ruta asimétrica que puede interrumpir la funcionalidad con el escenario del equilibrador de carga público.

Para más información sobre Azure Load Balancer, consulte [¿Qué e Azure Load Balancer?](../load-balancer/load-balancer-overview.md)

## <a name="public-load-balancer"></a>Equilibrador de carga público

Con un equilibrador de carga público, el equilibrador de carga se implementa con una dirección IP de front-end público.

### <a name="asymmetric-routing"></a>Ruta asimétrica

La ruta asimétrica se produce cuando un paquete toma una ruta al destino y toma otra ruta cuando vuelve al origen. Este problema se produce cuando una subred tiene una ruta predeterminada que va a la dirección IP privada del firewall, y usted usa un equilibrador de carga público. En este caso, el tráfico entrante del equilibrador de carga se recibe a través de su dirección IP pública, pero la ruta de vuelta pasa a través de la dirección IP privada del firewall. Dado que el firewall es con estado, quita el paquete de vuelta porque el firewall no tiene conocimiento de este tipo de sesión establecida.

### <a name="fix-the-routing-issue"></a>Corrección del problema de enrutamiento

Al implementar una instancia de Azure Firewall en una subred, un paso es crear una ruta predeterminada para la subred que dirige los paquetes a través de la dirección IP privada del firewall ubicado en AzureFirewallSubnet. Para más información, consulte [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md#create-a-default-route).

Al introducir el firewall en el escenario del equilibrador de carga, quiere que su tráfico de Internet llegue a través de la dirección IP pública del firewall. Desde allí, el firewall aplica sus reglas de firewall y traduce las direcciones de red de los paquetes a la dirección IP pública del equilibrador de carga. Allí es donde se produce el problema. Los paquetes llegan a la dirección IP pública del firewall, pero vuelven al firewall a través de la dirección IP privada (mediante la ruta predeterminada).
Para evitar este problema, cree una ruta de host adicional para la dirección IP pública del firewall. Los paquetes que van a la dirección IP pública del firewall se enrutan a través de Internet. Esto evita que tomen la ruta predeterminada a la dirección IP privada del firewall.

Por ejemplo, las rutas siguientes son para un firewall en la dirección IP pública 13.86.122.41 y dirección IP privada 10.3.1.4.

![Tabla de rutas](media/integrate-lb/route-table.png)

## <a name="internal-load-balancer"></a>Equilibrador de carga interno

Con un equilibrador de carga interno, el equilibrador de carga se implementa con una dirección IP de front-end privado.

No hay ningún problema de rutas asimétricas con este escenario. Los paquetes entrantes llegan a la dirección IP pública del firewall, se traducen a la dirección IP privada del equilibrador de carga y, luego, devuelven a la dirección IP privada del firewall con la misma ruta de vuelta.

Por lo tanto, puede implementar este escenario similar para el escenario del equilibrador de carga público, pero sin necesidad de la ruta de host de direcciones IP públicas del firewall.

## <a name="additional-security"></a>Seguridad adicional

Para mejorar la seguridad de su escenario de equilibrio de carga, puede usar grupos de seguridad de red (NSG).

Por ejemplo, puede crear un NSG en la subred de back-end donde se encuentran las máquinas virtuales de carga equilibrada. Permita el tráfico entrante procedente de la dirección IP/puerto del firewall.

Para más información sobre NSG, consulte [Grupos de seguridad](../virtual-network/security-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md).