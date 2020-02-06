---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 16a0fa29c067262f1794528b16abfce662d05605
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76748854"
---
<!-- F-series, Fs-series* -->

Los tamaños de las máquinas virtuales optimizadas para proceso tienen una elevada relación entre CPU y memoria. Estos tamaños son idóneos para servidores web de tráfico medio, aplicaciones de red, procesos por lotes y servidores de aplicaciones. En este artículo se proporciona información sobre el número de vGPU, discos de datos y NIC. También incluye información sobre el rendimiento del almacenamiento y el ancho de banda de red para cada tamaño de esta agrupación.

La serie Fsv2 se basa en el procesador Intel® Xeon® Platinum 8168. Presenta una velocidad de reloj turbo sostenida de todos los núcleos de hasta 3,4 GHz y una velocidad de reloj turbo de un solo núcleo máxima de 3,7 GHz. Las instrucciones de Intel® AVX-512 son nuevas en los procesadores escalables de Intel. Estas instrucciones pueden llegar a duplicar el rendimiento en las cargas de trabajo de procesamiento vectorial en las operaciones de número de punto flotante de precisión individual y doble. En otras palabras, son muy rápidas para cualquier carga de trabajo de cálculo.

Dado que su precio por hora es inferior, la serie Fsv2 tiene la mejor relación precio/rendimiento de la cartera de Azure según la unidad de proceso de Azure (ACU) por vCPU.

## <a name="fsv2-series-sup1sup"></a>Serie Fsv2 <sup>1</sup>

ACU: 195 - 210

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

| Size             | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|------------------|--------|-------------|----------------|----------------|--------------------------|--------------------------|-------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000 / 31 (32)           | 3200 / 47                | 2 / 875                 |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000 / 63 (64)           | 6400 / 95                | 2/1750               |
| Standard_F8s_v2  | 8      | 16          | 64             | 16             | 16000 / 127 (128)        | 12800 / 190              | 4/3500               |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000 / 255 (256)        | 25600 / 380              | 4/7000               |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000 / 512 (512)        | 51200 / 750              | 8/14000              |
| Standard_F48s_v2 | 48     | 96          | 384            | 32             | 96000 / 768 (768)        | 76800 / 1100             | 8 / 21000              |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000 / 1024 (1024)     | 80000 / 1100             | 8/28000              |
| Standard_F72s_v2<sup>2,&nbsp;3</sup> | 72 | 144 | 576         | 32             | 144000 / 1152 (1520)     | 80000 / 1100             | 8 / 30 000              |

<sup>1</sup> Las máquinas virtuales de la serie Fsv2 cuentan con la tecnología Hyper-Threading de Intel®.

<sup>2</sup> El uso de más de 64 vCPU requiere uno de estos sistemas operativos invitados compatibles:
- Windows Server 2016 o posterior
- Ubuntu 16.04 LTS o posterior, con kernel ajustado para Azure (kernel 4.15 o posterior)
- SLES 12 SP2 o posterior
- RHEL o CentOS, versiones 6.7 a 6.10, con la versión 4.3.1 (o posterior) del paquete LIS proporcionado por Microsoft instalada
- RHEL o CentOS, versión 7.3, con la versión 4.2.1 (o posterior) del paquete LIS proporcionado por Microsoft instalada
- RHEL o CentOS, versión 7.6 o posterior
- Oracle Linux con UEK4 o posterior
- Debian 9 con el kernel de backports, Debian 10 o posterior
- CoreOS con un kernel 4.14 o posterior

<sup>3</sup> La instancia está aislada en el hardware dedicado a un solo cliente.
