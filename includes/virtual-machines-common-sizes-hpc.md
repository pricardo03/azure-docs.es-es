---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/26/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 4b7cda593bd4dd39a7220aa282529535c6a63bea
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64914558"
---
Serie H máquinas virtuales (VM) están diseñadas para proporcionar un rendimiento de clase de liderazgo, escalabilidad MPI y mayor rentabilidad para una variedad de cargas de trabajo HPC reales.

Las máquinas virtuales de la serie HB están optimizadas para aplicaciones que funcionan con ancho de banda de la memoria, como dinámica de fluidos, análisis explícito de elementos finitos y modelos de clima. Las VM HB presentan núcleos de procesador 60 AMD EPYC 7551, con 4 GB de RAM por núcleo de CPU y no tienen hyperthreading. La plataforma AMD EPYC proporciona un ancho de banda de memoria de más de 260 GB/s.

Máquinas virtuales de serie HC están optimizadas para aplicaciones orientadas a cálculo densa, como análisis de elementos finitos implícita, dinámica molecular y química computacional. Las VM HC presentan núcleos de procesador 44 Intel Xeon Platinum 8168, 8 GB de RAM por núcleo de CPU y no tienen hyperthreading. La plataforma de Intel Xeon Platinum admite rico ecosistema de herramientas de software como la biblioteca Intel Math Kernel Library de Intel.

100 Gb/seg. de la característica de HB y máquinas virtuales HC Mellanox EDR InfiniBand en fat sin bloqueo de árbol configuración para un rendimiento coherente RDMA. HB y HC las máquinas virtuales admiten controladores Mellanox/OFED estándar de forma que todos los tipos MPI y las versiones, así como verbos RDMA, también se admiten.

Las máquinas virtuales de serie H están optimizadas para aplicaciones orientadas a las frecuencias de CPU elevadas o gran cantidad de memoria por los requisitos principales. Intel Xeon E5 2667 v3 procesador de serie H máquinas virtuales característica 8 o 16 núcleos, 7 o 14 GB de RAM por núcleo de CPU y no hyperthreading. Características de la serie H 56 Gb/s Mellanox FDR InfiniBand en fat sin bloqueo de árbol configuración para un rendimiento coherente RDMA. Las máquinas virtuales de serie H compatible con Intel MPI 5.x y MS-MPI.

## <a name="hb-series"></a>HB-series

Premium Storage: Admite Premium Storage de almacenamiento en caché: Compatible

| Tamaño | vCPU | Procesador | Memoria (GB) | Ancho de banda de memoria GB/s | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de núcleo (GHz, pico) | Rendimiento de RDMA (GB/s) | Compatibilidad con MPI | Almacenamiento temporal (GB) | Discos de datos máx. | Ethernet máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYC 7551 | 240 | 263 | 2.0 | 2.55 | 2.55 | 100 | Todo | 700 | 4 | 1 |

<br>

## <a name="hc-series"></a>HC-series

Premium Storage: Admite Premium Storage de almacenamiento en caché: Compatible


| Tamaño | vCPU | Procesador | Memoria (GB) | Ancho de banda de memoria GB/s | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de núcleo (GHz, pico) | Rendimiento de RDMA (GB/s) | Compatibilidad con MPI | Almacenamiento temporal (GB) | Discos de datos máx. | Ethernet máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HC44rs | 44 | Intel Xeon Platinum 8168 | 352 | 191 | 2.7 | 3.4 | 3.7 | 100 | Todo | 700 | 4 | 1 |


<br>

## <a name="h-series"></a>Serie H

ACU: 290-300

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

| Tamaño | vCPU | Procesador | Memoria (GB) | Ancho de banda de memoria GB/s | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de núcleo (GHz, pico) | Rendimiento de RDMA (GB/s) | Compatibilidad con MPI | Almacenamiento temporal (GB) | Discos de datos máx. | Ethernet máx. NIC |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 | 8 | Intel Xeon E5 2667 v3 | 56 | 40 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16 | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 |  56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H8m | 8 | Intel Xeon E5 2667 v3 | 112 | 40 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H16r <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 | 56 | 2000 | Intel 5.x, MS-MPI | 64 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | 56 | 2000 | Intel 5.x, MS-MPI | 64 | 4 |

<sup>1</sup> Para las aplicaciones MPI, la red de back-end RDMA dedicada está habilitada por la red InfiniBand FDR, que ofrece una latencia sumamente baja y un alto ancho de banda.

<br>