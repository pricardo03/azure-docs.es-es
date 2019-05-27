---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 2ae72045ae18d84eac2a6d619d94e3a9e49415ae
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66160226"
---
## <a name="issue-custom-image-provisioning-errors"></a>Problema: Imagen personalizada; errores de aprovisionamiento
Los errores de aprovisionamiento surgen si carga o capturar una imagen de máquina virtual generalizada como una imagen de máquina virtual especializada o viceversa. El primero producirá un error de tiempo de espera de aprovisionamiento y el último provocará un error de aprovisionamiento. Para implementar la imagen personalizada sin errores, debe asegurarse de que el tipo de la imagen no cambia durante el proceso de captura.

En la tabla siguiente se enumeran las posibles combinaciones de imágenes generalizadas y especializadas, el tipo de error que puede encontrarse y lo que necesita hacer para solucionar los errores.

