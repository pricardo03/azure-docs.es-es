---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 05/16/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 40857879826963f9a82cb5864b6980305c522679
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145707"
---
En esta sección se proporciona información sobre las generaciones anteriores de tamaños de máquina virtual. Se pueden seguir usando estos tamaños, pero hay generaciones más recientes disponibles. 

## <a name="f-series"></a>Serie F

La serie F se basa en el procesador Intel Xeon® E5-2673 v3 (Haswell) de 2,4 GHz, que puede alcanzar velocidades de reloj de hasta 3,1 GHz con Intel Turbo Boost Technology 2.0. Este es el mismo rendimiento de CPU que el de la serie Dv2 de máquinas virtuales.  

Las máquinas virtuales de la serie F son una opción excelente para cargas de trabajo que exigen CPU más rápidas, pero que no necesitan tanta memoria o almacenamiento temporal por vCPU.  Las cargas de trabajo, como análisis, servidores de juegos, servidores web y procesamiento por lotes se beneficiarán del valor de la serie F.

ACU: 210 - 250

Premium Storage:  No admitido

Caching de Premium Storage:  No admitido

| Tamaño         | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Rendimiento máximo de almacenamiento temporal: IOPS / MBps de lectura / MBps de escritura | Discos de datos máx. / rendimiento: E/S | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 4 / 4x500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 8 / 8x500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000 / 750 / 375                                        | 64 / 64x500                       | 8 / 12 000           |

## <a name="fs-series-sup1sup"></a>Serie Fs <sup>1</sup>

La serie Fs proporciona todas las ventajas de la serie F, además de Premium Storage.

ACU: 210 - 250

Premium Storage:  Compatible

Caching de Premium Storage:  Compatible

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |4 |4000 / 32 (12) |3200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |8 |8000 / 64 (24) |6400 / 96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |16 |16 000 / 128 (48) |12 800 / 192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |32 |32 000 / 256 (96) |25 600 / 384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |64 |64 000 / 512 (192) |51 200 / 768 |8 / 12 000 |

MBps = 10^6 bytes por segundo y GiB = 1024^3 bytes.

<sup>1</sup> El rendimiento de disco máximo (IOPS o MBps) posible con una máquina virtual de la serie Fs puede estar limitado por el número, el tamaño y la fragmentación de los discos conectados.  Para información detallada, consulte [Azure Premium Storage: Diseño de alto rendimiento](../articles/virtual-machines/windows/premium-storage-performance.md).  

## <a name="ls-series"></a>Serie Ls

La serie LS ofrece hasta 32 vCPU, con el [procesador Intel® Xeon® de la familia E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). La serie LS obtiene el mismo rendimiento de CPU que las series G/GS y viene con 8 GiB de memoria por vCPU.

La serie Ls no permite crear una memoria caché local para aumentar el número de IOPS que pueden alcanzar los discos de datos duraderos. El alto rendimiento e IOPS del disco local hace que las máquinas virtuales de la serie Ls ideal para los almacenes NoSQL, como Apache Cassandra y MongoDB que replican datos entre varias máquinas virtuales para lograr persistencia en el caso de una sola máquina virtual.

ACU: 180-240

Premium Storage:  Compatible

Caching de Premium Storage:  No admitido
 
| Tamaño          | vCPU | Memoria (GiB) | Almacenamiento temporal (GiB) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal (IOPS/MBps) | Rendimiento de disco no en caché máx. (E/S por segundo/Mbps) | Nº máx. de NIC/ancho de banda de red esperado (Mbps) | 
|----------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s   | 4  | 32  | 678   | 16 | 20 000/200 | 5000 / 125  | 2/4000  | 
| Standard_L8s   | 8  | 64  | 1,388 | 32 | 40 000/400 | 10 000 / 250 | 4/8000  | 
| Standard_L16s  | 16 | 128 | 2,807 | 64 | 80 000 / 800 | 20 000 / 500 | 8 / 16 000 | 
| Standard_L32s&nbsp;<sup>1</sup> | 32   | 256  | 5,630 | 64   | 160 000/1600   | 40 000 / 1000     | 8/20 000 | 

El rendimiento máximo de disco que es posible con una VM de la serie Ls puede estar limitado por el número, el tamaño y la fragmentación de cualquier disco asociado. Para información detallada, consulte [Azure Premium Storage: Diseño de alto rendimiento](../articles/virtual-machines/windows/premium-storage-performance.md).

<sup>1</sup> La instancia está aislada en el hardware dedicado a un solo cliente.

## <a name="nvv2-series-preview"></a>Serie NVv2 (versión preliminar)

**Recomendación de tamaño más reciente**: [Serie NVv3 (versión preliminar)](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu#nvv2-series-preview)

Las máquinas virtuales de la serie NVv2 cuentan con la tecnología de las GPU [Nvidia Test M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) y la tecnología GRID de NVIDIA con CPU Intel Broadwell. Estas máquinas virtuales están orientadas a escritorios virtuales y aplicaciones gráficas aceleradas mediante GPU donde los clientes desean ver sus datos, simular resultados para verlos, trabajar en CAD o representar y transmitir contenido. Además, estas máquinas virtuales pueden ejecutar cargas de trabajo de precisión única, como la codificación y la representación. Las máquinas virtuales NVv2 son compatibles con Premium Storage y traen el doble de memoria del sistema (RAM) si se comparan con la serie NV anterior.  

Cada GPU de las instancias de NVv2 viene con una licencia de GRID. Esta licencia le ofrece flexibilidad para utilizar una instancia de NV como estación de trabajo virtual para un solo usuario; también se pueden conectar 25 usuarios simultáneos a la VM para un escenario de aplicación virtual.

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC | Estaciones de trabajo virtuales | Aplicaciones virtuales | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6s_v2 |6 |112 |320 | 1 | 8 | 12 | 4 | 1 | 25 |
| Standard_NV12s_v2 |12 |224 |640 | 2 | 16 | 24 | 8 | 2 | 50 |
| Standard_NV24s_v2 |24 |448 |1280 | 4 | 32 | 32 | 8 | 4 | 100 |

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard_A0 - A4 con CLI y PowerShell

En el modelo de implementación clásica, algunos nombres de tamaños de VM varían ligeramente en la CLI y en PowerShell:

* Standard_A0 es ExtraSmall
* Standard_A1 es Small
* Standard_A2 es Medium
* Standard_A3 es Large
* Standard_A4 es ExtraLarge
