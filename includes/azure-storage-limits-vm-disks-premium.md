---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: e7e57c6a821731874dcb1d99a3133b6ede1da26e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66148187"
---
**Discos de máquinas virtuales de no administrados Premium: Límites por cuenta**

| Recurso | Límite predeterminado |
| --- | --- |
| Capacidad total de disco por cuenta |35 TB |
| Capacidad total de instantáneas por cuenta |10 TB |
| Ancho de banda máximo por cuenta (entrada + salida)<sup>1</sup> |<=50 Gbps |

<sup>1</sup>*entrada* hace referencia a todos los datos de las solicitudes que se envían a una cuenta de almacenamiento. *Salida* hace referencia a todos los datos de las respuestas que se reciben de una cuenta de almacenamiento.

**Discos de máquinas virtuales de no administrados Premium: Límites por disco**

| Tipo de disco de Premium Storage | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| Tamaño del disco |128 GB |512 GB |1,024 GiB (1 TB) |2048 GB (2 TB)|4,095 GiB (4 TB)|
| Número máximo de IOPS por disco |500 |2,300 |5.000 |7500 |7500 |
| Rendimiento máximo por disco |100 MB/s | 150 MB/s |200 MB/s |250 MB/s |250 MB/s |
| Número máximo de discos por cuenta de almacenamiento |280 |70 |35 | 17 | 8 |

**Discos de máquinas virtuales de no administrados Premium: Límites por máquina virtual**

| Recurso | Límite predeterminado |
| --- | --- |
| Número máximo de IOPS por máquina virtual |80 000 IOPS con máquina virtual GS5 |
| Rendimiento máximo por máquina virtual |2000 MB/s con máquina virtual GS5 |

