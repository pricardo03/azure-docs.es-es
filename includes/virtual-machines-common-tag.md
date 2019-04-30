---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: ccc2b574ea054a1b0ecf32a1e59691050fb66fcf
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108311"
---
## <a name="tagging-a-virtual-machine-through-templates"></a>Etiquetado de una máquina virtual mediante plantillas
En primer lugar, veamos el etiquetado a través de plantillas. [Esta plantilla](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) coloca etiquetas en los siguientes recursos: Proceso (Máquina Virtual), almacenamiento (cuenta de almacenamiento) y red (dirección IP pública, red Virtual e interfaz de red). Esta plantilla es para una máquina virtual Windows, pero se puede adaptar a máquinas virtuales Linux.

Haga clic en el botón **Implementar en Azure** del [vínculo de la plantilla](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Esto le remitirá al [Portal de Azure](https://portal.azure.com/) , donde puede implementar esta plantilla.

![Implementación sencilla con etiquetas](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Esta plantilla incluye las siguientes etiquetas: *Departamento*, *aplicación*, y *creado por*. Estas etiquetas se pueden agregar o modificar directamente en la plantilla si se desea cambiar sus nombres.

![Etiquetas de Azure en una plantilla](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Como puede ver, las etiquetas se definen como pares de clave-valor, separados por dos puntos (:). Las etiquetas deben definirse con este formato:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Guarde el archivo de plantilla cuando termine de modificarlo con las etiquetas que prefiera.

A continuación, en la sección **Editar parámetros** , puede rellenar los valores de las etiquetas.

![Editar etiquetas en el Portal de Azure](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Haga clic en **Crear** para implementar esta plantilla con sus valores de etiqueta.

## <a name="tagging-through-the-portal"></a>Etiquetado a través del portal
Después de crear los recursos con etiquetas, puede ver, agregar y eliminar etiquetas en el portal.

Seleccione el icono de etiquetas para ver las etiquetas:

![Icono de etiquetas en el Portal de Azure](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Para agregar una nueva etiqueta desde el portal, defina su propio par de clave-valor y guárdela.

![Agregar etiqueta nueva en el Portal de Azure](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

La nueva etiqueta debería aparecer en la lista de etiquetas del recurso.

![Etiqueta nueva guardada en el Portal de Azure](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

