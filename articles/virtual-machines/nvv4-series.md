---
title: 'Serie NVv4: máquinas virtuales de Azure'
description: Especificaciones de las máquinas virtuales de la serie NVv4.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 3b1deceb8d5c6e2a22254b2719449e5adb069def
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565404"
---
# <a name="nvv4-series-preview"></a>Serie NVv4 (versión preliminar)

Las máquinas virtuales de la serie NVv4 usan la tecnología de las GPU [AMD Radeon instinto MI25](https://www.amd.com/en/products/professional-graphics/instinct-mi25) y las CPU AMD EPYC 7V12 (Rome). Con la serie NVv4, Azure introduce máquinas virtuales con GPU parciales. Elija la máquina virtual del tamaño adecuado para aplicaciones con aceleración gráfica por GPU y escritorios virtuales a partir de un octavo de una GPU con un búfer de cuadros de 2 GiB y hasta una GPU completa con un búfer de cuadros de 16 GiB. Las máquinas virtuales NVv4 actualmente solo admiten el sistema operativo invitado Windows.

[Regístrese y acceda a estas máquinas durante la versión preliminar](https://aka.ms/nvv4signup).
<br>

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV4as_v4 |4 |14 |88 | 1/8 | 2 | 4 | 2 |
| Standard_NV8as_v4 |8 |28 |176 | 1/4 | 4 | 8 | 4 |
| Standard_NV16as_v4 |16 |56 |352 | 1/2 | 8 | 16 | 8 |
| Standard_NV32as_v4 |32 |112 |704 | 1 | 16 | 32 | 8 |

<sup>1</sup> las máquinas virtuales de la serie NVv4 incluyen tecnología multithreading simultánea de AMD

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

Para aprovechar las funciones de GPU de las máquinas virtuales de la serie N de Azure que ejecutan Windows, deben instalarse controladores de GPU de NVIDIA o AMD.

La [extensión de controlador de GPU de NVIDIA](/extensions/hpccompute-gpu-windows.md) instala los controladores CUDA de NVIDIA o GRID adecuados en una máquina virtual Windows de la serie N. Instale o administre la extensión mediante Azure Portal o con herramientas como las plantillas de Azure PowerShell o Azure Resource Manager. Consulte la [documentación de la extensión de controlador de GPU de NVIDIA](/extensions/hpccompute-gpu-windows.md) para los sistemas operativos compatibles y los pasos de implementación. Para una información general sobre las extensiones de máquina virtual, consulte [Características y extensiones de las máquinas virtuales de Azure](/extensions/overview.md).

Si decide instalar manualmente los controladores de GPU de NVIDIA, consulte [Instalación de controladores de GPU de la serie N para Windows](/windows/n-series-driver-setup.md) para obtener información sobre los sistemas operativos y controladores compatibles, así como sobre los pasos de instalación y verificación.

Para instalar manualmente los controladores de GPU de AMD, vea [Instalación de controladores de GPU de AMD de la serie N para Windows](/windows/n-series-amd-driver-setup.md) para obtener información sobre los sistemas operativos y controladores compatibles, así como sobre los pasos de instalación y verificación.

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.