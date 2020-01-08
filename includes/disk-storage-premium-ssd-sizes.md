---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 10/24/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c88cba57ed987074c0dde3c8b251f54393667740
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75002642"
---
| Tamaños de SSD Premium | P1* | P2* | P3* | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60 | P70 | P80 |
|-------------------|----|----|----|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Tamaño de disco en GiB | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1024 | 2 048 | 4 096 | 8192 | 16 384 | 32 767 |
| IOPS por disco | 120 | 120 | 120 | 120 | 240 | 500 | 1100 | 2,300 | 5\.000 | 7500 | 7500 | 16 000 | 18 000 | 20.000 |
| Rendimiento de disco. | 25 MiB/s | 25 MiB/s | 25 MiB/s | 25 MiB/s | 50 MiB/s | 100 MiB/s | 125 MiB/s | 150 MiB/s | 200 MiB/s | 250 MiB/s | 250 MiB/s| 500 MiB/s | 750 MiB/s | 900 MiB/s |
| Máximo de IOPS de ráfaga por disco** | 3500 | 3500 | 3500 | 3500 | 3500 | 3500 | 3500 | 3500 |
| Rendimiento máximo de ráfaga por disco** | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s |
| Duración máxima de ráfaga** | 30 min  | 30 min  | 30 min  | 30 min  | 30 min  | 30 min  | 30 min  | 30 min  |

\*Indica un tamaño de disco que se encuentra actualmente en versión preliminar; para información sobre la disponibilidad regional, consulte [Nuevos tamaños de disco: Administrados y no administrados](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks#new-disk-sizes-managed-and-unmanaged).

\*\*Indica una característica que se encuentra actualmente en versión preliminar; consulte [Envío de ráfagas en disco](https://docs.microsoft.com/azure/virtual-machines/linux/disk-bursting#regional-availability) para más información.
