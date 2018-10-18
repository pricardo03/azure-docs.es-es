---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 16e2a9cfbd9f08428fddade290117b27bc3401f7
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44369193"
---
Las máquinas virtuales de la serie H de Azure son las más nuevas en VM de informática de alto rendimiento destinadas a tratar cargas de trabajo como el procesamiento por lotes, los análisis, el modelado molecular y la dinámica de fluidos. Estas VM de 8 y 16 vCPU se basan en la tecnología de procesador Intel Haswell E5-2667 V3 con memoria DDR4 y almacenamiento temporal basado en SSD. 

Además de una potencia de CPU notable, la serie H ofrece varias opciones para las redes RDMA de baja latencia con FDR InfiniBand y varias configuraciones de memoria para admitir requisitos computacionales de uso intensivo de la memoria.



## <a name="h-series"></a>Serie H

ACU: 290-300

Premium Storage: no se admite

Almacenamiento en caché de Premium Storage: no se admite

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento de discos máx.: E/S por segundo | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |32 |32 x 500 |2  |
| Standard_H16 |16 |112 |2000 |64 |64 x 500 |4 |
| Standard_H8m |8 |112 |1000 |32 |32 x 500 |2  |
| Standard_H16m |16 |224 |2000 |64 |64 x 500 |4  |
| Standard_H16r <sup>1</sup> |16 |112 |2000 |64 |64 x 500 |4  |
| Standard_H16mr <sup>1</sup> |16 |224 |2000 |64 |64 x 500 |4 |

<sup>1</sup> Para las aplicaciones MPI, la red de back-end RDMA dedicada está habilitada por la red InfiniBand FDR, que ofrece una latencia sumamente baja y un alto ancho de banda.

<br>






