---
title: archivo de inclusión
description: archivo de inclusión
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 10/29/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 5517cd236c503edce88ab45edbeff5604ef9322b
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50254729"
---
| | SKU Estándar | SKU Básico |
| --- | --- | --- |
| Tamaño de grupo de back-end | Admite hasta 1000 instancias | Admite hasta 100 instancias |
| Puntos de conexión del grupo de back-end | Cualquier máquina virtual en una única red virtual, lo que incluye la combinación de máquinas virtuales, conjuntos de disponibilidad y grupos de escalado de máquinas virtuales. | Máquinas virtuales en un único conjunto de disponibilidad o conjunto de escalado de máquinas virtuales. |
| [Sondeos de mantenimiento](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Comportamiento del sondeo de mantenimiento](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | Las conexiones TCP permanecen activas en el sondeo de la instancia __y__ en todos los sondeos | Las conexiones TCP permanecen activas en el sondeo de la instancia. Todas las conexiones de TCP terminan cuando todos los sondeos están inactivos. |
| Zonas de disponibilidad | En SKU de nivel Estándar, front-end zonales y con redundancia de zona para la entrada y la salida, asignaciones de flujos de salida, supervivencia a errores de zona, equilibrio de carga entre zonas. | No disponible |
| Diagnóstico | Azure Monitor, métricas multidimensionales, incluidos bytes y contadores de paquetes, estado de sondeo de mantenimiento, intentos de conexión (TCP SYN), mantenimiento de la conexión de salida (flujos SNAT correctos e incorrectos), medidas de planos de datos activos. | Azure Log Analytics solo para Load Balancer público, alerta de agotamiento de SNAT, recuento de mantenimientos del grupo back-end. |
| Puertos HA | Equilibrador de carga interno | No disponible |
| Seguro de forma predeterminada | Use la entrada predeterminada cerrada para las direcciones IP y los puntos de conexión de Load Balancer públicos y un grupo de seguridad de red para crear explícitamente una lista blanca que permita que el tráfico fluya. | Abierto de forma predeterminada, grupo de seguridad de red opcional. |
| [Conexiones salientes](../articles/load-balancer/load-balancer-outbound-connections.md) | Puede definir explícitamente un protocolo NAT de salida basado en grupos con [reglas de salida](../articles/load-balancer/load-balancer-outbound-rules-overview.md). Puede utilizar varios front-end con la deshabilitación de envíos en cada regla de equilibrio de carga. Un escenario de salida _debe_ crearse explícitamente para que la máquina virtual pueda usar la conectividad de salida.  Los puntos de conexión del servicio de red virtual son accesibles sin conectividad de salida y no se cuentan como datos procesados.  Las direcciones IP públicas, incluidos los servicios de PaaS de Azure que no están disponibles como puntos de conexión de servicio de red virtual, deben ser accesibles mediante conectividad de salida y cuentan como datos procesados. Si solo hay un equilibrador de carga interno que da servicio a una máquina virtual, no habrá conexiones de salida disponibles con el protocolo SNAT predeterminado; utilice [reglas de salida](../articles/load-balancer/load-balancer-outbound-rules-overview.md) en su lugar. La programación de SNAT de salida es específica del protocolo de transporte de la regla de equilibrio de carga de entrada. | Único front-end, seleccionado de forma aleatoria, cuando hay varios front-ends.  Cuando solo un equilibrador de carga interno da servicio a una máquina virtual, se usa SNAT predeterminada. |
| [Reglas de salida](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Configuración declarativa del protocolo NAT de salida, incluidas la dirección IP pública o el prefijo de IP público, el tiempo de espera de inactividad de salida y la asignación de puertos de SNAT personalizados. | No disponible |
|  [Restablecimiento de TCP en tiempo de espera de inactividad](../articles/load-balancer/load-balancer-tcp-reset.md) | Habilite en cualquier regla el restablecimiento de TCP (TCP RST) en tiempo de espera de inactividad. | No disponible |
| [Varios servidores front-end](../articles/load-balancer/load-balancer-multivip-overview.md) | Entrada y [salida](../articles/load-balancer/load-balancer-outbound-connections.md) | Solo de entrada |
| Operaciones de administración | La mayoría de las operaciones en menos de 30 segundos | Normalmente, entre 60 y 90 segundos. |
| Contrato de nivel de servicio | 99,99 % para la ruta de acceso a los datos con dos máquinas virtuales correctas. | [Implícito en el Acuerdo de Nivel de Servicio de las máquinas virtuales](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). | 
| Precios | Se cobra según el número de reglas y los datos procesados de entrada o salida asociados con el recurso.  | Sin cargo |
|  |  |  |
