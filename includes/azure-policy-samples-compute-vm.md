---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: eedce1ef3a4e7d56cfaf3142a72e370c96f9f575
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664649"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Permitir una imagen de máquina virtual personalizada desde un grupo de recursos](../articles/azure-policy/scripts/allow-custom-vm-image.md) |  Requiere que las imágenes personalizadas provengan de un grupo de recursos aprobado. Se especifica el nombre del grupo de recursos aprobado. |
| [SKU permitidas para cuentas de almacenamiento y máquinas virtuales](../articles/azure-policy/scripts/allowed-skus-storage.md) | Requiere que las cuentas de almacenamiento y las máquinas virtuales usen las SKU aprobadas. Se usan directivas integradas para garantizar SKU aprobadas. Se especifica una matriz de las SKU de máquinas virtuales aprobadas y una matriz de las SKU de cuentas de almacenamiento aprobadas. |
| [Imágenes de máquina virtual aprobadas](../articles/azure-policy/scripts/allowed-custom-images.md) | Requiere que solo las imágenes personalizadas aprobadas se puedan implementar en su entorno. Se especifica una matriz de identificadores de las imágenes aprobadas. |
| [Auditar si la extensión no existe](../articles/azure-policy/scripts/audit-ext-not-exist.md) | Audita si una extensión no se implementa con una máquina virtual. Se especifica el tipo y el publicador de la extensión para comprobar si se implementó. |
| [Extensiones de máquina virtual no permitidas](../articles/azure-policy/scripts/not-allowed-vm-ext.md) | Prohíbe el uso de las extensiones especificadas. Se especifica una matriz que contiene los tipos de extensiones prohibidos. |