---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 2ae72045ae18d84eac2a6d619d94e3a9e49415ae
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123849"
---
## <a name="issue-custom-image-provisioning-errors"></a>Problema: Imagen personalizada; errores de aprovisionamiento
Los errores de aprovisionamiento surgen si carga o capturar una imagen de máquina virtual generalizada como una imagen de máquina virtual especializada o viceversa. El primero producirá un error de tiempo de espera de aprovisionamiento y el último provocará un error de aprovisionamiento. Para implementar la imagen personalizada sin errores, debe asegurarse de que el tipo de la imagen no cambia durante el proceso de captura.

En la tabla siguiente se enumeran las posibles combinaciones de imágenes generalizadas y especializadas, el tipo de error que puede encontrarse y lo que necesita hacer para solucionar los errores.

<!--Update_Description: wording update, update link-->