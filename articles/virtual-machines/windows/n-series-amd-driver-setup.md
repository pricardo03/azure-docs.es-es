---
title: Instalación de controladores de GPU de AMD de la serie N de Azure para Windows
description: Instalación de controladores de GPU de NVIDIA para máquinas virtuales de la serie N que se ejecutan en Windows Server o Windows en Azure
services: virtual-machines-windows
author: vikancha
manager: jkabat
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/4/2019
ms.author: vikancha
ms.openlocfilehash: fdc6834f3fb5ee97f27a6397645b965863e90a6b
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190539"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Instalación de controladores de GPU de AMD en máquinas virtuales de la serie N con Windows

Para aprovechar las funcionalidades de GPU de las nuevas máquinas virtuales de la serie NVv4 de Azure que ejecutan Windows, deben instalarse controladores de GPU de AMD. La extensión del controlador AMD estará disponible en las próximas semanas. En este artículo se describen sistemas operativos compatibles, controladores y pasos de instalación y verificación manuales.

Para conocer las especificaciones básicas, las capacidades de almacenamiento y los detalles del disco, consulte [Tamaño de máquinas virtuales para GPU Windows](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

| SO | Controlador |
| -------- |------------- |
| Windows 10 EVD: compilación 1903 <br/><br/>Windows 10: compilación 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [19.Q4.1](https://download.microsoft.com/download/7/e/5/7e558ac0-3fff-413d-af62-800285a2fc53/Radeon-Pro-Software-for-Enterprise-19.Q4.1-Technical-Preview.exe) (.exe) |

## <a name="driver-installation"></a>Instalación del controlador

1. Conéctese mediante Escritorio remoto a cada máquina virtual de la serie NVv4.

1. Descargue y extraiga los archivos de instalación del controlador. Vaya a la carpeta y ejecute "setup. exe" para instalar el controlador compatible con el sistema operativo Windows.

## <a name="verify-driver-installation"></a>Comprobación de la instalación del controlador

Puede comprobar la instalación del controlador en el Administrador de dispositivos. En el ejemplo siguiente se muestra la configuración correcta de la tarjeta Radeon Instinct MI25 en una máquina virtual NVv4 de Azure.
<br />
![Propiedades del controlador de GPU](./media/n-series-amd-driver-setup/device-manager.png)

Puede usar dxdiag para comprobar las propiedades de presentación de GPU, incluida la RAM de vídeo. En el ejemplo siguiente se muestra la partición de 1/8 de la tarjeta Radeon Instinct MI25 en una máquina virtual NVv4 de Azure.
<br />
![Propiedades del controlador de GPU](./media/n-series-amd-driver-setup/dxdiag.png)
