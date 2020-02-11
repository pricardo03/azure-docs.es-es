---
title: archivo de inclusión
description: archivo de inclusión
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 02/08/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 4219df03f74f737c5f2435f9bc0842189dc1fd49
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76909195"
---
| | SKU Estándar | SKU Básico |
| --- | --- | --- |
| Tamaño de grupo de back-end | Admite hasta 1000 instancias. | Admite hasta 300 instancias. |
| Puntos de conexión del grupo de back-end | Cualquier máquina virtual en una única red virtual, lo que incluye las combinaciones de máquinas virtuales, conjuntos de disponibilidad y grupos de escalado de máquinas virtuales. | Máquinas virtuales en un único conjunto de disponibilidad o conjunto de escalado de máquinas virtuales. |
| [Sondeos de mantenimiento](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Comportamiento del sondeo de mantenimiento](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | Las conexiones TCP permanecen activas en el sondeo de la instancia __y__ en todos los sondeos. | Las conexiones TCP permanecen activas en un sondeo de instancia. Todas las conexiones TCP finalizan cuando todos los sondeos están inactivos. |
| Zonas de disponibilidad | Servidores front-end con redundancia de zona y zonas para el tráfico entrante y saliente. Las asignaciones de flujos salientes sobreviven a errores de zona. Equilibrio de carga entre zonas. | No disponible |
| Diagnóstico | Azure Monitor. Métricas multidimensionales, incluidos los contadores de bytes y paquetes. Estado del sondeo de mantenimiento. Intentos de conexión (TCP SYN). Mantenimiento de la conexión saliente (los flujos de SNAT correctos y con errores). Medidas activas del plano de datos. | Análisis de registros Azure solo para Load Balancer público. Alerta de agotamiento de SNAT. Recuento de mantenimiento del grupo de back-end. |
| Puertos HA | Equilibrador de carga interno | No disponible |
| Seguro de forma predeterminada | Las direcciones IP públicas, los puntos de conexión de Load Balancer públicos y los puntos de conexión de Load Balancer internos se cierran en los flujos de entrada a menos que los permita un grupo de seguridad de red. Tenga en cuenta que se permite el tráfico interno de la red virtual al equilibrador de carga interno. | Abrir de forma predeterminada. Grupo de seguridad de red opcional. |
| [Conexiones salientes](../articles/load-balancer/load-balancer-outbound-connections.md) | Puede definir explícitamente un protocolo NAT de salida basado en grupos con [reglas de salida](../articles/load-balancer/load-balancer-outbound-rules-overview.md). Puede usar varios servidores front-end con la exclusión voluntaria de la regla de equilibrio de carga. Para usar la conectividad de salida, _debe_ crearse explícitamente un escenario de salida para la máquina virtual, o el conjunto de disponibilidad y el conjunto de escalado de máquinas virtuales. Los puntos de conexión de servicio de red virtual son accesibles sin conectividad de salida y no se cuentan como datos procesados. Las direcciones IP públicas, incluidos los servicios de PaaS de Azure que no están disponibles como puntos de conexión de servicio de red virtual, deben ser accesibles mediante conectividad de salida y cuentan como datos procesados. Si solo hay una instancia de Load Balancer interno que atiende una máquina virtual, un conjunto de disponibilidad o un conjunto de escalado de máquinas virtuales, no habrá conexiones de salida disponibles mediante el protocolo SNAT predeterminado. Use [reglas de salida](../articles/load-balancer/load-balancer-outbound-rules-overview.md) en su lugar. La programación de SNAT saliente depende del Protocolo de transporte de la regla de equilibrio de carga de entrada. | Único front-end, seleccionado de forma aleatoria, cuando hay varios front-ends. Cuando solo Load Balancer interno atiende una máquina virtual, un conjunto de disponibilidad o un conjunto de escalado de máquinas virtuales, se usa SNAT de forma predeterminada. |
| [Reglas de salida](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Configuración de NAT de salida declarativa, mediante direcciones IP públicas o prefijos de dirección IP pública o ambos. Tiempo de espera de inactividad de salida configurable (4-120 minutos). Asignación de puerto SNAT personalizada | No disponible |
| [Restablecimiento de TCP en tiempo de espera de inactividad](../articles/load-balancer/load-balancer-tcp-reset.md) | Habilite en cualquier regla el restablecimiento de TCP (TCP RST) en tiempo de espera de inactividad. | No disponible |
| [Varios servidores front-end](../articles/load-balancer/load-balancer-multivip-overview.md) | Entrada y [salida](../articles/load-balancer/load-balancer-outbound-connections.md) | Solo de entrada |
| Operaciones de administración | La mayoría de las operaciones en menos de 30 segundos | Normalmente, entre 60 y 90 segundos |
| Contrato de nivel de servicio | 99,99 % para la ruta de acceso a los datos con dos máquinas virtuales correctas. | No aplicable | 
| Precios | Se cobra según el número de reglas y los datos procesados de entrada y salida asociados con el recurso. | Sin cargo |
|  |  |  |
