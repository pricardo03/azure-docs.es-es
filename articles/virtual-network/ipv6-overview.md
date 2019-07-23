---
title: Información general de IPv6 para Azure Virtual Network (versión preliminar)
titlesuffix: Azure Virtual Network
description: Descripción de IPv6 de los puntos de conexión y las rutas de acceso a datos de IPv6 en una instancia de Azure Virtual Network.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 0ec650880a45f6383b24b5ac810fc2ee745806b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "62131039"
---
# <a name="what-is-ipv6-for-azure-virtual-network-preview"></a>¿Qué es IPv6 para Azure Virtual Network? (versión preliminar)

IPv6 para Azure Virtual Network (VNET) le permite hospedar aplicaciones en Azure con la conectividad IPv6 e IPv4, tanto en una red virtual como hacia y desde Internet. Debido al agotamiento de direcciones IPv4 públicas, las nuevas redes de movilidad e Internet de las cosas (IoT) a menudo se basan en IPv6. Incluso las redes móviles e ISP consolidadas se están transformando a IPv6. Los servicios que solo usan IPv4 pueden encontrarse en desventaja real en los mercados existentes y emergentes. La conectividad IPv4/IPv6 de pila doble permite que los servicios hospedados en Azure atraviesen esta brecha tecnológica con los servicios en pila doble disponibles globalmente que se conectan fácilmente con la conectividad IPv4 existente y con estos nuevos dispositivos y redes IPv6.

La conectividad IPv6 original de Azure permite proporcionar fácilmente conectividad a Internet (IPv4/IPv6) de doble pila para las aplicaciones hospedadas en Azure. Permite la implementación simple de VM con conectividad IPv6 con equilibrio de carga para las conexiones iniciadas tanto entrantes como salientes. Esta característica todavía está disponible y se puede encontrar más información [aquí](../load-balancer/load-balancer-ipv6-overview.md).
La conectividad IPv6 para Azure Virtual Network es mucho más completa: permite implementar arquitecturas de soluciones IPv6 completas en Azure.

> [!Important]
> La conectividad IPv6 para Azure Virtual Network se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El siguiente diagrama representa una implementación simple de pila doble (IPv4/IPv6) en Azure:

![Diagrama de implementación de red IPv6](./media/ipv6-support-overview/ipv6-sample-diagram.png)

## <a name="benefits"></a>Ventajas

Ventajas de IPv6 para Azure Virtual Network:

- Ayuda a ampliar el alcance de sus aplicaciones hospedadas en Azure en los mercados móvil y de Internet de las cosas en desarrollo.
- Las VM IPv4/IPv6 con pila doble proporcionan la máxima flexibilidad de implementación del servicio. Una sola instancia del servicio se puede conectar con clientes de Internet que admitan tanto IPv4 como IPv6.
- Se basa en la consolidada y estable conectividad IPv6 de Azure Virtual Machines con Internet.
- Ofrece seguridad de forma predeterminada, puesto que la conectividad IPv6 a Internet solo se establece cuando se solicita explícitamente en la implementación.

## <a name="capabilities"></a>Capacidades

La compatibilidad con IPv6 para las máquinas virtuales incluye las siguientes funcionalidades:

- Los clientes de Azure pueden definir su propio espacio de direcciones de red virtual IPv6 para satisfacer las necesidades de sus aplicaciones y clientes, o bien para la integración sin problemas en su espacio de IP local.
- Las redes virtuales (IPv4 e IPv6) de pila doble con subredes de pila doble permiten a las aplicaciones conectarse con recursos IPv4 e IPv6 en su red virtual o en Internet.
- Protección de sus recursos con reglas de IPv6 para grupos de seguridad de red
- Personalice el enrutamiento del tráfico IPv6 en la red virtual con rutas definidas por el usuario (especialmente al utilizar aplicaciones virtuales de red para mejorar su aplicación).
- Compatibilidad del equilibrador de carga de IPv6 para crear aplicaciones resistentes y escalables, que incluye compatibilidad con Azure DNS para registros AAAA y direcciones IP públicas IPv6.
- Agregue fácilmente la conectividad IPv6 a implementaciones solo IPv4 existentes con la actualización in situ.
- Cree aplicaciones de pila doble que se escalen automáticamente a su carga con conjuntos de escalado de máquinas virtuales.

## <a name="limitations"></a>Limitaciones
La versión preliminar de IPv6 para Azure Virtual Network tiene las siguientes limitaciones:
- IPv6 para Azure Virtual Network (versión preliminar) está disponible en todas las regiones globales de Azure, pero solo en Azure global, no en las nubes públicas.   
- La compatibilidad del portal con la versión preliminar se limita solo a la visualización de buena parte de configuración de IPv6, pero no toda; sin embargo, IPv6 para la red virtual ofrece compatibilidad total y documentación (con ejemplos) para la implementación de IPv6 con Azure PowerShell y la interfaz de la línea de comandos (CLI).
- La compatibilidad con Network Watcher para la versión preliminar se limita a los registros de flujo de NSG y a las capturas de paquetes de red.
- La compatibilidad con el equilibrio de carga para la versión preliminar se limita inicialmente a la versión básica de Load Balancer.
- Las direcciones IP públicas de nivel de instancia (direcciones IP públicas directamente en una VM) no se admiten en la versión preliminar.  
- El emparejamiento de redes virtuales (por regiones o globalmente) no se admite en la versión preliminar. 

## <a name="pricing"></a>Precios

El ancho de banda y los recursos de Azure de IPv6 tienen el mismo precios que para IPv4. No se aplican cargos adicionales ni diferentes para IPv6. Puede encontrar detalles sobre precios de [direcciones IP públicas](https://azure.microsoft.com/pricing/details/ip-addresses/), [ancho de banda de red](https://azure.microsoft.com/pricing/details/bandwidth/) o [Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [implementar una aplicación de pila doble IPv6 con Azure PowerShell](virtual-network-ipv4-ipv6-dual-stack-powershell.md).
- Aprenda a [implementar una aplicación de pila doble IPv6 con la CLI de Azure](virtual-network-ipv4-ipv6-dual-stack-cli.md).
