---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: eadff6f61bd9e5979ae835230de53562ff8542d1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004006"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Permitir una imagen de máquina virtual personalizada desde un grupo de recursos](../articles/governance/policy/samples/allow-custom-vm-image.md) |  Requiere que las imágenes personalizadas provengan de un grupo de recursos aprobado. Se especifica el nombre del grupo de recursos aprobado. |
| [SKU permitidas para cuentas de almacenamiento y máquinas virtuales](../articles/governance/policy/samples/allowed-skus-storage.md) | Requiere que las cuentas de almacenamiento y las máquinas virtuales usen las SKU aprobadas. Se usan directivas integradas para garantizar SKU aprobadas. Se especifica una matriz de las SKU de máquinas virtuales aprobadas y una matriz de las SKU de cuentas de almacenamiento aprobadas. |
| [Imágenes de máquina virtual aprobadas](../articles/governance/policy/samples/allowed-custom-images.md) | Requiere que solo las imágenes personalizadas aprobadas se puedan implementar en su entorno. Se especifica una matriz de identificadores de las imágenes aprobadas. |
| [Auditar si la extensión no existe](../articles/governance/policy/samples/audit-ext-not-exist.md) | Audita si una extensión no se implementa con una máquina virtual. Se especifica el tipo y el publicador de la extensión para comprobar si se implementó. |
| [Extensiones de máquina virtual no permitidas](../articles/governance/policy/samples/not-allowed-vm-ext.md) | Prohíbe el uso de las extensiones especificadas. Se especifica una matriz que contiene los tipos de extensiones prohibidos. |