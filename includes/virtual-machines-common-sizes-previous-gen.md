---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 07/06/2018
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: c16483f4b8ef160c78ff95582faf54c9a9d24a04
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57964287"
---
En este artículo se proporciona información sobre las generaciones anteriores de tamaños de máquina virtual. Se pueden seguir usando estos tamaños, pero hay generaciones más recientes disponibles.


## <a name="ds-series"></a>Serie DS

ACU: 160-250 <sup>1</sup>

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |4 |4000 / 32 (43) |3200 / 32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |8 |8000 / 64 (86) |6400 / 64 |2 / 1000 |
| Standard_DS3 |4 |14 |28 |16 |16 000 / 128 (172) |12 800 / 128 |4 / 2000 |
| Standard_DS4 |8 |28 |56 |32 |32 000 / 256 (344) |25 600 / 256 |8 / 4000 |

<sup>1</sup> familia de máquinas virtuales se pueden ejecutar en uno de la CPU siguiente: 2.2 GHz Intel Xeon® E5-2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) de 2,3 GHz Intel XEON® o E5-2673 v4 (Broadwell)
<br>

## <a name="ds-series---memory-optimized"></a>Serie DS: con optimización para memoria

ACU: 160-250 <sup>1,2</sup>

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8000 / 64 (72) |6400 / 64 |2 / 1000 |
| Standard_DS12 |4 |28 |56 |16 |16 000 / 128 (144) |12 800 / 128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32 000 / 256 (288) |25 600 / 256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64 000 / 512 (576) |51 200 / 512 |8 / 8000 |

<sup>1</sup> El rendimiento de disco máx. (E/S por segundo o Mbps) posible con una VM de la serie DS puede estar limitado por el número, el tamaño y la fragmentación de los discos conectados.  Para información detallada, consulte [Azure Premium Storage: Diseño de alto rendimiento](../articles/virtual-machines/windows/premium-storage-performance.md).

<sup>2</sup> familia de máquinas virtuales se pueden ejecutar en uno de la CPU siguiente: 2.2 GHz Intel Xeon® E5-2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) de 2,3 GHz Intel XEON® o E5-2673 v4 (Broadwell)

## <a name="d-series"></a>Serie D 

ACU: 160-250 <sup>1</sup>

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

| Tamaño         | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Rendimiento máximo de almacenamiento temporal: IOPS / MBps de lectura / MBps de escritura | Discos de datos máx. / rendimiento: E/S | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 4 / 4x500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 8 / 8x500                         | 2 / 1000                     |
| Standard_D3  | 4         | 14          | 200            | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 2000                     |
| Standard_D4  | 8         | 28          | 400            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 4000                     |

<sup>1</sup> familia de máquinas virtuales se pueden ejecutar en uno de la CPU siguiente: 2.2 GHz Intel Xeon® E5-2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) de 2,3 GHz Intel XEON® o E5-2673 v4 (Broadwell)
<br>

## <a name="d-series---memory-optimized"></a>Serie D: con optimización para memoria

ACU: 160-250 <sup>1</sup>

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

| Tamaño         | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Rendimiento máximo de almacenamiento temporal: IOPS / MBps de lectura / MBps de escritura | Discos de datos máx. / rendimiento: E/S | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8x500                         | 2 / 1000                     |
| Standard_D12 | 4         | 28          | 200            | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48000 / 750 / 375                                        | 64 / 64x500                       | 8 / 8000                |

<sup>1</sup> familia de máquinas virtuales se pueden ejecutar en uno de la CPU siguiente: 2.2 GHz Intel Xeon® E5-2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) de 2,3 GHz Intel XEON® o E5-2673 v4 (Broadwell)
<br>

## <a name="a-series---compute-intensive-instances"></a>Serie A: instancias de proceso intensivo

ACU: 225

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

Los tamaños A8-A11 los de la serie H también se conocen como *instancias de proceso intensivo*. El hardware que ejecuta estos tamaños está diseñado y optimizado para aplicaciones de proceso intensivo que consumen muchos recursos de red, incluidas las aplicaciones de clúster de proceso de alto rendimiento (HPC), el modelado y las simulaciones. La serie A8-A11 utiliza Intel Xeon E5-2670 a 2,6 GHz y la serie H, Intel Xeon E5-2667 v3 a 3,2 GHz.  

| Tamaño | vCPU | Memoria: GiB | Almacenamiento temporal (HDD): GiB | Discos de datos máx. | Rendimiento de discos de datos máx.: E/S | Nº máx. NIC|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 <sup>1</sup> |8 |56 |382 |32 |32x500 |2 |
| Standard_A9 <sup>1</sup> |16 |112 |382 |64 |64x500 |4 |
| Standard_A10 |8 |56 |382 |32 |32x500 |2  |
| Standard_A11 |16 |112 |382 |64 |64x500 |4 |

<sup>1</sup>Para las aplicaciones MPI, la red de back-end RDMA dedicada está habilitada por la red InfiniBand FDR, que ofrece una latencia sumamente baja y un alto ancho de banda.

<br>

## <a name="a-series"></a>Serie A

ACU: 50-100

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

| Tamaño | vCPU | Memoria: GiB | Almacenamiento temporal (HDD): GiB | Discos de datos máx. | Rendimiento de discos de datos máx.: E/S | Nº máx. de NIC/ancho de banda de red esperado (Mbps)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 <sup>1</sup> |1 |0,768 |20 |1 |1x500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2 x 500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4 |4x500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8x500 |2 / 1000 |
| Standard_A4 |8 |14 |605 |16 |16x500 |4 / 2000 |
| Standard_A5 |2 |14 |135 |4 |4x500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8x500 |2 / 1000 |
| Standard_A7 |8 |56 |605 |16 |16x500 |4 / 2000 |

<br>

<sup>1</sup> La suscripción del tamaño A0 es excesiva en el hardware físico. Solo en este tamaño específico, las implementaciones de otros clientes podrían afectar el rendimiento de la carga de trabajo en ejecución. A continuación, se indica el rendimiento relativo como la línea base esperada, sujeta a una variabilidad aproximada de 15 por ciento.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard_A0 - A4 con CLI y PowerShell

En el modelo de implementación clásica, algunos nombres de tamaños de VM varían ligeramente en la CLI y en PowerShell:

* Standard_A0 es ExtraSmall 
* Standard_A1 es Small
* Standard_A2 es Medium
* Standard_A3 es Large
* Standard_A4 es ExtraLarge

## <a name="basic-a"></a>A básico

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

Los tamaños de niveles básicos se utilizan sobre todo para cargas de trabajo de desarrollo y otras aplicaciones que no requieren equilibrio de carga, escalado automático o máquinas virtuales de uso intensivo de memoria.

|Tamaño – Tamaño\nombre | vCPU |Memoria|NICs (Máx)|Tamaño máximo del disco temporal |Máx. Discos de datos (1023 GB cada uno)|Máx. E/S (300 por disco)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1x300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2x300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4x300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8x300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16x300|
 


