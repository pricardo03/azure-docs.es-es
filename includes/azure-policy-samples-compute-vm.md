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
ms.openlocfilehash: b6587a3928c2ddf0d00c90e07643525314690e42
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318291"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Permitir una imagen de máquina virtual personalizada desde un grupo de recursos](../articles/governance/policy/samples/allow-custom-vm-image.md) |  Requiere que las imágenes personalizadas provengan de un grupo de recursos aprobado. Se especifica el nombre del grupo de recursos aprobado. |
| [SKU permitidas para cuentas de almacenamiento y máquinas virtuales](../articles/governance/policy/samples/allowed-skus-storage.md) | Requiere que las cuentas de almacenamiento y las máquinas virtuales usen las SKU aprobadas. Se usan directivas integradas para garantizar SKU aprobadas. Se especifica una matriz de las SKU de máquinas virtuales aprobadas y una matriz de las SKU de cuentas de almacenamiento aprobadas. |
| [Imágenes de máquina virtual aprobadas](../articles/governance/policy/samples/allowed-custom-images.md) | Requiere que solo las imágenes personalizadas aprobadas se puedan implementar en su entorno. Se especifica una matriz de identificadores de las imágenes aprobadas. |
| [Auditar si la extensión no existe](../articles/governance/policy/samples/audit-extension-not-exist.md) | Audita si una extensión no se implementa con una máquina virtual. Se especifica el tipo y el publicador de la extensión para comprobar si se implementó. |
| [Extensiones de máquina virtual no permitidas](../articles/governance/policy/samples/not-allowed-vm-extension.md) | Prohíbe el uso de las extensiones especificadas. Se especifica una matriz que contiene los tipos de extensiones prohibidos. |