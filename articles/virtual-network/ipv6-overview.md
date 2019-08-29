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
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: 33078439e8f055d746fad9949a9b0d7651e120f7
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543808"
---
# <a name="what-is-ipv6-for-azure-virtual-network-preview"></a>¿Qué es IPv6 para Azure Virtual Network? (versión preliminar)

IPv6 para Azure Virtual Network (VNet) le permite hospedar aplicaciones en Azure con la conectividad IPv6 e IPv4, tanto en una red virtual como hacia y desde Internet. Debido al agotamiento de direcciones IPv4 públicas, las nuevas redes de movilidad e Internet de las cosas (IoT) a menudo se basan en IPv6. Incluso las redes móviles e ISP consolidadas se están transformando a IPv6. Los servicios que solo usan IPv4 pueden encontrarse en desventaja real en los mercados existentes y emergentes. La conectividad IPv4/IPv6 de pila doble permite que los servicios hospedados en Azure atraviesen esta brecha tecnológica con los servicios en pila doble disponibles globalmente que se conectan fácilmente con la conectividad IPv4 existente y con estos nuevos dispositivos y redes IPv6.

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

IPv6 para VNet incluye las siguientes características:

- Los clientes de Azure pueden definir su propio espacio de direcciones de red virtual IPv6 para satisfacer las necesidades de sus aplicaciones y clientes, o bien para la integración sin problemas en su espacio de IP local.
- Las redes virtuales (IPv4 e IPv6) de pila doble con subredes de pila doble permiten a las aplicaciones conectarse con recursos IPv4 e IPv6 en su red virtual o en Internet.
    > [!IMPORTANT]
    > Las subredes para IPv6 deben tener un tamaño exactamente de /64.  Esto garantiza la compatibilidad si decide habilitar el enrutamiento de la subred a una red local, ya que algunos enrutadores sólo pueden aceptar rutas /64 IPv6.  
- Proteja los recursos con reglas de IPv6 para grupos de seguridad de red.
- Personalice el enrutamiento del tráfico IPv6 en la red virtual con rutas definidas por el usuario (especialmente al utilizar aplicaciones virtuales de red para mejorar su aplicación).
- Permita a los clientes de Internet acceder sin problemas a la aplicación de pila dual con el protocolo de su elección con compatibilidad con Azure DNS para registros IPv6 (AAAA). 
- Compatibilidad IPv6 pública con Standard Load Balancer para crear aplicaciones resistentes y escalables, lo que incluye:
    - Sondeo de estado de IPv6 opcional para establecer qué instancias del grupo de back-end tienen un estado correcto y, por tanto, reciben flujos nuevos. .  
    - Reglas de salida opciones que proporcionan un control declarativo completo sobre la conectividad de salida para escalar y ajustar esta conectividad según sus necesidades específicas.
    - Distintas configuraciones de front-end opcionales que permiten que un único equilibrador de carga use varias direcciones IP públicas IPv6: el mismo protocolo y puerto de front-end pueden usarse entre las direcciones de front-end.
- La dirección IP pública a nivel de instancia proporciona conectividad IPv6 a Internet directamente a las máquinas virtuales individuales.
- Agregue fácilmente la conectividad IPv6 a implementaciones solo IPv4 existentes con la actualización in situ.
- Cree aplicaciones de pila doble que se escalen automáticamente a su carga con conjuntos de escalado de máquinas virtuales.
- El soporte técnico de Portal para la versión preliminar ahora incluye la creación, edición y eliminación interactivas de redes virtuales y subredes de pila dual (IPv4+IPv6), reglas de grupo de seguridad de red IPv6, rutas IPv6 definidas por el usuario y direcciones IP públicas IPv6.  

## <a name="limitations"></a>Limitaciones
La versión preliminar de IPv6 para Azure Virtual Network tiene las siguientes limitaciones:
- IPv6 para Azure Virtual Network (versión preliminar) está disponible en todas las regiones globales de Azure, pero solo en Azure global, no en las nubes públicas.
- La compatibilidad de Portal con los componentes de Standard Load Balancer es solo de vista.  Sin embargo, el soporte técnico completo y la documentación (con ejemplos) están disponibles para las implementaciones de Standard Load Balancer con Azure PowerShell y la interfaz de la línea de comandos (CLI).   
- La compatibilidad con Network Watcher para la versión preliminar se limita a los registros de flujo de NSG y a las capturas de paquetes de red.
- El emparejamiento de redes virtuales (por regiones o globalmente) no se admite en la versión preliminar.
- Al usar Standard Load Balancer con IPv6 externa, se aplican los siguientes límites: 
  - Las reglas de salida pueden hacer referencia a varias direcciones IP públicas de front-end, pero es posible que **no** hagan referencia a un prefijo público IPv6. El prefijo público de IP solo admite prefijos IPv4.
  - Las reglas de equilibrio de carga de IPv6 **no** pueden usar la característica de *IP flotante*. La reutilización de puertos en las instancias de back-end solo se admite con IPv4.
- La característica de prefijo de dirección IP pública de Azure no admite la reserva de un bloque de direcciones IPv6 accesibles desde Internet.

## <a name="pricing"></a>Precios

El ancho de banda y los recursos de Azure de IPv6 tienen el mismo precios que para IPv4. No se aplican cargos adicionales ni diferentes para IPv6. Puede encontrar detalles sobre precios de [direcciones IP públicas](https://azure.microsoft.com/pricing/details/ip-addresses/), [ancho de banda de red](https://azure.microsoft.com/pricing/details/bandwidth/) o [Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [implementar una aplicación de pila doble IPv6 con Azure PowerShell](virtual-network-ipv4-ipv6-dual-stack-powershell.md).
- Aprenda a [implementar una aplicación de pila doble IPv6 con la CLI de Azure](virtual-network-ipv4-ipv6-dual-stack-cli.md).
