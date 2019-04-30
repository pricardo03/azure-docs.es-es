---
title: Solución de problemas de discos asociados a máquinas virtuales de Azure | Microsoft Docs
description: Proporciona vínculos a recursos de solución de problemas para discos duros virtuales (VHD) de máquinas virtuales de Azure.
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 10/31/2018
ms.date: 12/10/2018
ms.author: v-jay
ms.openlocfilehash: bf1aa75399f28b8dd1732bc9ec09c15e46132939
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098106"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Solución de problemas de discos asociados a máquinas virtuales de Azure 

Las máquinas virtuales (VM) de Azure usan discos duros virtuales (VHD) para el disco del sistema operativo y cualquier disco de datos asociado. Los discos duros virtuales se almacenan como blobs en páginas en una o varias cuentas de Azure Storage. Este artículo le dirige al contenido de solución de problemas comunes que pueden surgir con los discos duros virtuales. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Solución de problemas de errores de eliminación de almacenamiento en una máquina virtual

En algunos casos, puede encontrar un error al eliminar un recurso de almacenamiento cuando una máquina virtual de una implementación de Resource Manager contiene discos duros virtuales no asociados. Para obtener ayuda para resolver este problema, consulte uno de los siguientes artículos: 

  * En máquinas virtuales de Linux: [Storage deletion errors in Resource Manager deployment](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * En las máquinas virtuales de Windows: [Storage deletion errors in Resource Manager deployment](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Solución de problemas de reinicios inesperados de máquinas virtuales con discos duros virtuales asociados

Si se encuentra con que una máquina virtual con un gran número de discos duros virtuales asociados se reinicia inesperadamente, consulte uno de los siguientes artículos:

  * En máquinas virtuales de Linux: [Reinicios inesperados de las máquinas virtuales con discos duros virtuales conectados](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * En las máquinas virtuales de Windows: [Reinicios inesperados de las máquinas virtuales con discos duros virtuales conectados](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
