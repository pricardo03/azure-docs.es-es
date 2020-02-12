---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: include
ms.date: 04/26/2019
ms.author: azcspmt;jonbeck;cynthn;amverma
ms.custom: include file
ms.openlocfilehash: 29e453dd6a9ea7ac83d84adb7eb0f3d998c1eaaf
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76961435"
---
Las máquinas virtuales (VM) optimizadas para HPC de Azure están diseñadas para ofrecer un rendimiento de primer nivel, escalabilidad de MPI y rentabilidad para diversas aplicaciones del mundo real.
 
## <a name="infiniband-networking-for-large-scale-hpc"></a>Redes InfiniBand para HPC de gran escala
Las máquinas virtuales HBv2 cuentan con la característica Mellanox HDR InfiniBand a 200 GB/s, mientras que las máquinas virtuales HB y HC cuentan con la característica MeliBox EDR InfiniBand a 100 GB/s. Cada uno de estos tipos de máquinas virtuales están conectados en un árbol FAT sin bloqueos para un rendimiento de RDMA optimizado y coherente.

Las máquinas virtuales HBv2 admiten el enrutamiento adaptativo y el transporte conectado dinámico (DCT, además de los transportes estándar RC y UD). Estas características mejoran el rendimiento, la escalabilidad y la coherencia de las aplicaciones, y se recomienda totalmente su uso.  

Las máquinas virtuales HBv2, HB y HC son compatibles con los controladores estándar Mellanox/OFED. Como tal, todos los verbos RDMA y tipos de MPI son compatibles. Cada uno de estos tipos de máquinas virtuales también admite la descarga basada en hardware de MPI colectiva para aumentar el rendimiento de la aplicación.
 
Las máquinas virtuales de la serie H original incluyen la característica 56 GB/s Mellanox FDR InfiniBand. Para aprovechar la interfaz InfiniBand en la serie H, los clientes deben implementar mediante imágenes oficialmente específicas para este tipo de VM desde Azure Marketplace. 


## <a name="hbv2-series"></a>Serie HBv2
Las máquinas virtuales de la serie HBv2 están optimizadas para aplicaciones impulsadas por el ancho de banda de la memoria, como la dinámica de fluidos, el análisis de elementos finitos y la simulación de yacimientos. Las máquinas virtuales HBv2 cuentan con 120 núcleos de procesador AMD EPYC 7742, 4 GB de RAM por núcleo de CPU y sin multithreading simultáneo. Cada máquina virtual de HBv2 proporciona hasta 340 GB/s de ancho de banda de memoria y hasta 4 teraFLOPS de proceso FP64. 

Premium Storage: Compatible

| Size | vCPU | Procesador | Memoria (GB) | Ancho de banda de memoria, en GB/s | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de cada núcleo (GHz, pico) | Rendimiento de RDMA (GB/s) | Compatibilidad con MPI | Almacenamiento temporal (GB) | Discos de datos máx. | NIC Ethernet máx. |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB120rs_v2 | 120 | AMD EPYC 7V12 | 480 | 350 | 2.45 | 3.1 | 3.3 | 200 | All | 480 + 960 | 8 | 1 |

<br>

## <a name="hb-series"></a>Serie HB
Las máquinas virtuales de la serie HB están optimizadas para aplicaciones que funcionan con ancho de banda de la memoria, como la dinámica de fluidos, el análisis explícito de elementos finitos y los modelos de clima. Las máquinas virtuales HB cuentan con 60 núcleos de procesador AMD EPYC 7551, 4 GB de RAM por núcleo de CPU y ningún multithreading simultáneo. Una máquina virtual HB proporciona hasta 260 GB/s de ancho de banda de memoria.  

ACU: 199-216

Premium Storage: Compatible

Almacenamiento en caché de Premium Storage: Compatible

| Size | vCPU | Procesador | Memoria (GiB) | Ancho de banda de memoria (GiB/s) | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de cada núcleo (GHz, pico) | Rendimiento de RDMA (GiB/s) | Compatibilidad con MPI | Almacenamiento temporal (GiB) | Discos de datos máx. | NIC Ethernet máx. |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYC 7551 | 240 | 263 | 2.0 | 2.55 | 2.55 | 100 | All | 700 | 4 | 1 |

<br>

## <a name="hc-series"></a>Serie HC
Las máquinas virtuales de la serie HC están optimizadas para aplicaciones basadas en cálculos intensivos, como el análisis implícito de elementos finitos, la dinámica molecular y la química computacional. Las máquinas virtuales HC cuentan con 44 núcleos de procesador Intel Xeon Platinum 8168, 8 GB de RAM por núcleo de CPU y no tienen hyperthreading. La plataforma Intel Xeon Platinum admite el rico ecosistema de herramientas de software de Intel, como la biblioteca Intel Math Kernel Library. 

ACU: 297-315

Premium Storage: Compatible

Almacenamiento en caché de Premium Storage: Compatible


| Size | vCPU | Procesador | Memoria (GiB) | Ancho de banda de memoria (GiB/s) | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de cada núcleo (GHz, pico) | Rendimiento de RDMA (GiB/s) | Compatibilidad con MPI | Almacenamiento temporal (GiB) | Discos de datos máx. | NIC Ethernet máx. |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HC44rs | 44 | Intel Xeon Platinum 8168 | 352 | 191 | 2.7 | 3.4 | 3.7 | 100 | All | 700 | 4 | 1 |


<br>

## <a name="h-series"></a>Serie H
Las máquinas virtuales de la serie H están optimizadas para aplicaciones basadas en frecuencias alta de CPU o requisitos de gran cantidad de memoria por núcleo. Las máquinas virtuales de la serie H cuentan con 8 o 16 núcleos de procesador Intel Xeon E5 2667 v3, hasta 14 GB de RAM por núcleo de CPU y no tienen hyperthreading. Una máquina virtual de la serie H proporciona hasta 80 GB/s de ancho de banda de memoria.

ACU: 290-300

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

| Size | vCPU | Procesador | Memoria (GiB) | Ancho de banda de memoria (GiB/s) | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de cada núcleo (GHz, pico) | Rendimiento de RDMA (GiB/s) | Compatibilidad con MPI | Almacenamiento temporal (GiB) | Discos de datos máx. | NIC Ethernet máx. |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 | 8 | Intel Xeon E5 2667 v3 | 56 | 40 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16 | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 |  - | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H8m | 8 | Intel Xeon E5 2667 v3 | 112 | 40 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H16r <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |

<sup>1</sup> Para las aplicaciones MPI, la red de back-end RDMA dedicada está habilitada por la red InfiniBand FDR.

<br>
