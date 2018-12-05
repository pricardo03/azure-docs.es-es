---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11/27/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: f58d08629fcde791186d27ae09e7417453faf8ad
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52585828"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

### <a name="nvidia-tesla-cuda-drivers"></a>Controladores NVIDIA Tesla (CUDA)

Los controladores NVIDIA Tesla (CUDA) para máquinas virtuales de las series NC, NCv2, NCv3, ND y NDv2 (y opcionales para la serie NV) solo se admiten en las distribuciones de sistemas operativos enumeradas en la tabla siguiente. Los vínculos de descarga de controladores están actualizados en el momento de la publicación. Para ver los controladores más recientes, visite el sitio web de [NVIDIA](http://www.nvidia.com/).

> [!TIP]
> Como alternativa a la instalación manual de controladores de CUDA en una máquina virtual de Windows Server, puede implementar una imagen de [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) de Azure. Las ediciones de DSVM para Windows Server 2016 preinstalan los controladores NVIDIA CUDA, la biblioteca CUDA Deep Neural Network Library y otras herramientas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>Controladores de NVIDIA GRID

Microsoft redistribuye los instaladores del controlador NVIDIA GRID para VM de las series NV y NVv2 que se emplean como estaciones de trabajo virtuales o para aplicaciones virtuales. Instale estos controladores GRID en máquinas virtuales de la serie NV de Azure y solo en los sistemas operativos enumerados en la tabla siguiente. Estos controladores incluyen licencias del software GRID Virtual GPU en Azure. No es necesario configurar un servidor de licencias de software vGPU NVIDIA.

| SO | Controlador |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [GRID 7 (411.81)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe) |
| Windows Server 2012 R2 | [GRID 7 (411.81)](https://go.microsoft.com/fwlink/?linkid=874184) (.exe)  |
