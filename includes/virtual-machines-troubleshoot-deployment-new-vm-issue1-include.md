---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 2ae72045ae18d84eac2a6d619d94e3a9e49415ae
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186313"
---
## <a name="issue-custom-image-provisioning-errors"></a>Problema: imagen personalizada; errores de aprovisionamiento
Los errores de aprovisionamiento surgen si carga o capturar una imagen de máquina virtual generalizada como una imagen de máquina virtual especializada o viceversa. El primero producirá un error de tiempo de espera de aprovisionamiento y el último provocará un error de aprovisionamiento. Para implementar la imagen personalizada sin errores, debe asegurarse de que el tipo de la imagen no cambia durante el proceso de captura.

En la tabla siguiente se enumeran las posibles combinaciones de imágenes generalizadas y especializadas, el tipo de error que puede encontrarse y lo que necesita hacer para solucionar los errores.

