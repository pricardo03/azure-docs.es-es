---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ab68fc6533be5e3241de2e49652251fea5fe2f7d
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75780636"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

### <a name="nvidia-tesla-cuda-drivers"></a>Controladores NVIDIA Tesla (CUDA)

Los controladores NVIDIA Tesla (CUDA) para máquinas virtuales de las series NC, NCv2, NCv3, ND y NDv2 (y opcionales para la serie NV) solo se admiten en las distribuciones de sistemas operativos enumeradas en la tabla siguiente. Los vínculos de descarga de controladores están actualizados en el momento de la publicación. Para ver los controladores más recientes, visite el sitio web de [NVIDIA](https://www.nvidia.com/).

> [!TIP]
> Como alternativa a la instalación manual de controladores de CUDA en una máquina virtual de Windows Server, puede implementar una imagen de [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) de Azure. Las ediciones de DSVM para Windows Server 2016 preinstalan los controladores NVIDIA CUDA, la biblioteca CUDA Deep Neural Network Library y otras herramientas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>Controladores de NVIDIA GRID

Microsoft redistribuye los instaladores del controlador NVIDIA GRID para VM de las series NV y NVv3 que se emplean como estaciones de trabajo virtuales o para aplicaciones virtuales. Instale estos controladores GRID en VM de la serie NV de Azure y solo en los sistemas operativos enumerados en la tabla siguiente. Estos controladores incluyen licencias del software GRID Virtual GPU en Azure. No es necesario configurar un servidor de licencias de software vGPU NVIDIA.

Tenga en cuenta que la extensión de NVIDIA siempre instalará el controlador más reciente. Aquí se proporcionan vínculos a la versión anterior para los clientes que tienen dependencias de una versión anterior.

En Windows Server 2019, Windows Server 2016 y Windows 10 (hasta la compilación 1909):
- [GRID 10.0 (441.66)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe)
- [GRID 9.0 (431.02)](https://download.microsoft.com/download/8/C/C/8CC88D54-EB07-44D3-8FA9-B797B173ED04/431.02_grid_win10_server2016_server2019_64bit_international.exe) (.exe) 

Para Windows Server 2012 R2, Windows Server 2008 R2, Windows 8 y Windows 7: 
- [GRID 10.0 (441.66)](https://go.microsoft.com/fwlink/?linkid=874184) (.exe)
- [GRID 9.1 (431.79)](https://download.microsoft.com/download/8/6/e/86ef2daa-b31e-43ad-90f2-bd795384b71e/431.79_grid_win7_win8_server2008R2_server2012R2_64bit_international.exe) (.exe)  
