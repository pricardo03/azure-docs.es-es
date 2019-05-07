---
title: Rendimiento de la red de máquinas virtual de Azure | Microsoft Docs
description: Aprenda acerca del rendimiento de la red de máquinas virtuales de Azure.
services: virtual-network
documentationcenter: na
author: steveesp
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/26/2019
ms.author: kumud,steveesp, mareat
ms.openlocfilehash: 9d74e53c754367ecfa63642514db93354fcadf25
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153724"
---
# <a name="virtual-machine-network-bandwidth"></a>Ancho de banda de la red de máquinas virtuales

Azure ofrece una variedad de tamaños y tipos de máquinas virtuales, cada uno con una combinación diferente de funcionalidades de rendimiento. Una de ellas es el rendimiento de red (o ancho de banda) medido en megabits por segundo (Mbps). Dado que las máquinas virtuales se hospedan en hardware compartido, la capacidad de red debe compartirse equitativamente entre las máquinas virtuales que compartan el mismo hardware. A las máquinas virtuales más grandes se les asigna relativamente más ancho de banda que las más pequeñas.
 
El ancho de banda de red asignado a cada máquina virtual se mide en el tráfico de salida de la máquina virtual. Todo el tráfico de red que deja la máquina virtual se cuenta para el límite asignado, independientemente del destino. Por ejemplo, si una máquina virtual tiene un límite de Mbps 1000, ese límite se aplica si el tráfico saliente está destinado a otra máquina virtual en la misma red virtual, o fuera de Azure.
 
La entrada no se mide o no está directamente limitada. Sin embargo, hay otros factores, como los límites de la CPU y de almacenamiento, que pueden afectar la capacidad de una máquina virtual de procesar los datos entrantes.

Las redes aceleradas son una característica diseñada para mejorar el rendimiento de red, incluida la utilización de la CPU, el rendimiento y la latencia. Mientras que las redes aceleradas pueden mejorar el rendimiento de una máquina virtual, puede hacerlo solo hasta el ancho de banda asignado de la máquina virtual. Para más información sobre las redes aceleradas, consulte los temas sobre redes aceleradas para máquinas virtuales [Windows](create-vm-accelerated-networking-powershell.md) o [Linux](create-vm-accelerated-networking-cli.md).
 
Las máquinas virtuales de Azure deben tener una interfaz de red, pero pueden tener varias conectadas a ellas. El ancho de banda asignado a una máquina virtual es la suma de todo el tráfico saliente en todas las interfaces de red conectadas a una máquina virtual. En otras palabras, el ancho de banda asignado es por máquina virtual, independientemente de la cantidad de interfaces de red conectadas a la máquina virtual. Para obtener información sobre la cantidad de interfaces de red que admiten los distintos tamaños de máquinas virtuales de Azure, vea los tamaños de máquinas virtuales [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) y [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

## <a name="expected-network-throughput"></a>Rendimiento esperado de la red

El rendimiento de salida esperado y el número de interfaces de red compatibles con cada tamaño de máquina virtual se detallan en los tamaños de máquinas virtuales [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) y [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de Azure. Seleccione un tipo, como fin general, y luego seleccione una serie de tamaños en la página resultante, como la serie Dv2. Cada serie tiene una tabla con las especificaciones de red en la última columna con el nombre **N.º máx. NIC/rendimiento de red esperado (Mbps)**. 

El límite de rendimiento se aplica a la máquina virtual. El rendimiento no se ve afectado por los siguientes factores:
- **Número de interfaces de red**: El límite de ancho de banda es acumulativo de todo el tráfico saliente desde la máquina virtual.
- **Las redes aceleradas**: Aunque la característica puede ser útil para lograr el límite publicado, no cambia el límite.
- **Destino del tráfico**: Todos los destinos se cuentan para el límite de salida.
- **Protocolo**: Todo el tráfico saliente a través de todos los protocolos cuenta para el límite.

## <a name="network-flow-limits"></a>Límites de flujo de red

Además de ancho de banda, el número de conexiones de red presentes en una máquina virtual en un momento dado puede afectar al rendimiento de su red. La pila de red Azure mantiene el estado para cada dirección de una conexión TCP/UDP en estructuras de datos denominado 'flujos'. Una conexión TCP/UDP típica tendrá 2 flujos creados, uno para la entrada y otro para la dirección de salida. 

Transferencia de datos entre los puntos de conexión requiere la creación de varios flujos además de los que realizar la transferencia de datos. Algunos ejemplos son los flujos creados para la resolución DNS y los flujos creados para los sondeos de estado del equilibrador de carga. También tenga en cuenta que la red de aplicaciones virtuales (NVA) como puertas de enlace, los servidores proxy, firewalls, verá los flujos que se crea para las conexiones que terminan en el dispositivo y se originan en el dispositivo. 

![Recuento de flujo de conversación de TCP a través de una aplicación de reenvío](media/virtual-machine-network-throughput/flow-count-through-network-virtual-appliance.png)

## <a name="flow-limits-and-recommendations"></a>Límites de flujo y recomendaciones

Hoy en día, la pila de red de Azure admite flujos de red total de 250K con buen rendimiento para las máquinas virtuales con más de 8 núcleos de CPU y 100 k fluye total con buen rendimiento para las máquinas virtuales con menos de 8 núcleos de CPU. Más allá de esta red de límite rendimiento descenderá para flujos adicionales hasta un límite máximo de 1 millón de total de 500 K entrante y 500, flujos de salida después de qué flujos adicionales se quitan K.

||Las máquinas virtuales con < 8 núcleos de CPU|Máquinas virtuales con más de 8 núcleos de CPU|
|---|---|---|
|<b>Buen rendimiento</b>|Flujos de 100K |Flujos de 250K|
|<b>Disminución del rendimiento</b>|Superior a 100k fluye|Superior a 250K fluye|
|<b>Límite del flujo</b>|1M flujos|1M flujos|

Las métricas están disponibles en [Azure Monitor](../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines) para realizar un seguimiento del número de flujos de red y la velocidad de creación de flujo en las instancias de VM o VMSS.

![azure-monitor-flow-metrics.png](media/virtual-machine-network-throughput/azure-monitor-flow-metrics.png)

Las tasas de establecimiento y finalización de conexión también pueden afectar al rendimiento de red como conexión establecimiento y finalización recursos compartidos de CPU con rutinas de procesamiento de paquetes. Se recomienda evaluar cargas de trabajo en los patrones de tráfico esperado y escalar horizontalmente cargas de trabajo adecuadamente para satisfacer las necesidades de rendimiento. 

## <a name="next-steps"></a>Pasos siguientes

- [Optimización del rendimiento de red en un sistema operativo de máquina virtual](virtual-network-optimize-network-bandwidth.md)
- [Prueba de rendimiento de red](virtual-network-bandwidth-testing.md) para una máquina virtual.
