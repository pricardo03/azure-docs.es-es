---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 12/12/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7d1f75df9318c53a6d9e38c4d7b68587cf9a0d4b
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54057422"
---
**HDD de máquina virtual administrados estándar**

| Tipo de disco Estándar  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60 *             | S70 *             | S80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamaño de disco en GiB          | 32             | 64             | 128            | 256  | 512            | 1024    | 2 048     | 4095    | 8192     | 16 384     | 32 767     |
| IOPS por disco       | Hasta 500              | Hasta 500              | Hasta 500              | Hasta 500 | Hasta 500              | Hasta 500              | Hasta 500             | Hasta 500              | Hasta 1300              | Hasta 2000              | Hasta 2000              |
| Rendimiento de disco | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s| Hasta 300 MiB/s | Hasta 500 MiB/s | Hasta 500 MiB/s |

**SSD de máquina virtual administrados estándar**

| Tipo de disco SSD estándar  | E10               | E15               | E20             | E30              | E40              | E50              | E60 *             | E70 *             | E80 *             |
|---------------------|---------------------|---------------------|-----------------|------------------|------------------|------------------|-------------------|-------------------|-------------------|
| Tamaño de disco en GiB    | 128                 | 256                 | 512             | 1024            | 2 048            | 4095            | 8192             | 16 384            | 32 767            |
| IOPS por disco       | Hasta 500           | Hasta 500           | Hasta 500       | Hasta 500        | Hasta 500        | Hasta 500        | Hasta 1300       | Hasta 2000       | Hasta 2000       |
| Rendimiento de disco | Hasta 60 MB/s     | Hasta 60 MB/s     | Hasta 60 MB/s | Hasta 60 MB/s  | Hasta 60 MB/s  | Hasta 60 MB/s  | Hasta 300 MiB/s | Hasta 500 MiB/s | Hasta 500 MiB/s |

**Discos de máquina virtual administrados Premium: por los límites de disco**

| Tipo de disco premium  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60 *             | P70 *             | P80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamaño de disco en GiB           | 32             | 64             | 128            | 256  | 512            | 1024    | 2 048     | 4095    | 8192     | 16 384     | 32 767     |
| IOPS por disco       | Hasta 120 | Hasta 240              | Hasta 500              | Hasta 1100 | Hasta 2300              | Hasta 5000              | Hasta 7500             | Hasta 7500              | Hasta 12 500              | Hasta 15 000              | Hasta 20 000              |
| Rendimiento de disco | Hasta 25 MiB/s | Hasta 50 MiB/s | Hasta 100 MiB/s | Hasta 125 MiB/s | Hasta 150 MiB/s | Hasta 200 MiB/s | Hasta 250 MiB/s | Hasta 250 MiB/s| Hasta 480 MiB/s | Hasta 750 MiB/s | Hasta 750 MiB/s |

**Discos de máquina virtual administrados Premium: por límites de máquina virtual**

| Recurso | Límite predeterminado |
| --- | --- |
| Máximo de IOPS por VM |80 000 IOPS con máquina virtual GS5 |
| Rendimiento máximo por VM |2000 MB/s con máquina virtual GS5 |
