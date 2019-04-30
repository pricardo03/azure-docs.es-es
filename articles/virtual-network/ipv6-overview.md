---
title: Información general de IPv6 para la red Virtual de Azure (versión preliminar)
titlesuffix: Azure Virtual Network
description: Descripción de IPv6 de rutas de datos y los puntos de conexión de IPv6 en una red virtual de Azure.
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
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62131039"
---
# <a name="what-is-ipv6-for-azure-virtual-network-preview"></a>¿Qué es IPv6 para Azure Virtual Network?  (versión preliminar)

IPv6 para Azure Virtual Network (VNET) le permite hospedar aplicaciones en Azure con la conectividad de IPv6 e IPv4 en una red virtual y a y desde Internet. Debidos al agotamiento de direcciones IPv4 públicas, nuevas redes de movilidad e Internet de las cosas (IoT) a menudo se basan en IPv6. Long incluso establece redes móviles y ISP que se va a se transforman a IPv6. Servicios de sólo IPv4 pueden encontrarse en desventaja real en los mercados existentes y emergentes. Pila dual IPv4/IPv6 conectividad permite que los servicios hospedados de Azure atravesar esta brecha tecnológica con los servicios disponibles globalmente, pila doble que conectarse fácilmente con IPv4 existente y estos nuevos dispositivos de IPv6 y redes.

Conectividad de IPv6 original de Azure permite fácilmente proporcionar conectividad a Internet (IPv4/IPv6) de doble pila para las aplicaciones hospedadas en Azure. Permite la implementación simple de máquinas virtuales con conectividad de IPv6 con equilibrio de carga entrante y saliente de las conexiones iniciadas. Esta característica sigue estando disponible y hay disponible más información [aquí](../load-balancer/load-balancer-ipv6-overview.md).
IPv6 para la red virtual de Azure es mucho más completa destacado: habilitación de arquitecturas de soluciones de completa IPv6 para implementarse en Azure.

> [!Important]
> IPv6 para Azure Virtual Network está actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El siguiente diagrama muestra una implementación simple de pila doble (IPv4/IPv6) de Azure:

![Diagrama de implementación de red de IPv6](./media/ipv6-support-overview/ipv6-sample-diagram.png)

## <a name="benefits"></a>Ventajas

Ventajas de IPv6 de red Virtual de Azure:

- Ayuda a ampliar el alcance de sus aplicaciones hospedadas en Azure en la creciente móviles y los mercados de Internet de las cosas.
- Dos máquinas virtuales de IPv4/IPv6 apiladas proporcionan flexibilidad de implementación máximas del servicio. Una instancia de servicio solo puede conectarse con clientes de IPv4 e IPv6 con capacidad para Internet.
- Se basa en estable, arraigada Azure VM a Internet conectividad IPv6.
- Seguridad predeterminada, puesto que la conectividad IPv6 a Internet sólo se establece cuando se solicita explícitamente en la implementación.

## <a name="capabilities"></a>Capacidades

Compatibilidad con IPv6 para las máquinas virtuales incluye las siguientes funcionalidades:

- Los clientes de Azure pueden definir su propio espacio de direcciones IPv6 de red virtual para satisfacer las necesidades de sus aplicaciones, los clientes, o integrar sin problemas en su espacio IP en el entorno local.
- Pila dual (IPv4 e IPv6) las redes virtuales con subredes de la pila dual permiten a las aplicaciones conectarse con recursos IPv4 e IPv6 en su red virtual o - Internet.
- Proteger los recursos con las reglas de IPv6 para grupos de seguridad de red
- Personalizar el enrutamiento de tráfico de IPv6 en la red virtual con las rutas de definidos por el usuario - especialmente al aprovechamiento de las aplicaciones virtuales de red para aumentar la aplicación.
- Compatibilidad de equilibrador de carga de IPv6 para crear aplicaciones resistentes y escalables que incluye compatibilidad con DNS de Azure para registros AAAA para direcciones IP públicas de IPv6.
- Agregar fácilmente la conectividad IPv6 para las implementaciones existentes de sólo IPv4 con actualización en contexto.
- Crear aplicaciones de pila dual que se escalan automáticamente a la carga con conjuntos de escalado de máquinas virtuales.

## <a name="limitations"></a>Limitaciones
La versión preliminar de IPv6 para la red virtual de Azure tiene las siguientes limitaciones:
- IPv6 para la red virtual de Azure (versión preliminar) está disponible en todas las regiones globales de Azure, pero solo en Global Azure - no las nubes de gobierno.   
- Soporte técnico para la versión preliminar del portal se limita a ver solo para la configuración de IPv6 mucho, pero no todas, sin embargo, IPv6 de red virtual tiene soporte técnico completo y la documentación (ejemplos) para la implementación de IPv6 con Azure Powershell y la interfaz de línea de comandos (CLI).
- Compatibilidad de Network Watcher con la versión preliminar se limita a los registros de flujo NSG y las capturas de paquetes de red.
- Compatibilidad con equilibrio de carga para la versión preliminar se limita inicialmente a Load Balancer básico.
- Las direcciones IP públicas de nivel de instancia (direcciones IP públicas directamente en una máquina virtual) no se admiten en la versión preliminar.  
- Emparejamiento de redes virtuales (por regiones o globalmente) no se admite en la versión preliminar. 

## <a name="pricing"></a>Precios

Ancho de banda y los recursos de Azure de IPv6 se cobran a los mismos precios que IPv4. No hay cargos adicionales o diferentes para IPv6. Puede encontrar detalles sobre los precios de [direcciones IP públicas](https://azure.microsoft.com/pricing/details/ip-addresses/), [ancho de banda de red](https://azure.microsoft.com/pricing/details/bandwidth/), o [equilibrador de carga](https://azure.microsoft.com/pricing/details/load-balancer/).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [implementar una aplicación de pila doble de IPv6 con Azure PowerShell](virtual-network-ipv4-ipv6-dual-stack-powershell.md).
- Obtenga información sobre cómo [implementar una aplicación de doble pila IPv6 mediante la CLI de Azure](virtual-network-ipv4-ipv6-dual-stack-cli.md).
