---
title: Use una galería de imágenes compartidas en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo configurar una cuenta de laboratorio para usar una galería de imágenes compartidas para que un usuario puede compartir una imagen con otro y otro usuario puede usar la imagen para crear una plantilla de máquina virtual en el laboratorio.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 93136c7d685bd9fc8ec4bcdea3a900b28029059b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60695208"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Use una galería de imágenes compartidas en Azure Lab Services
Este artículo muestra cómo los profesores/laboratorio administrador puede guardar una imagen de máquina virtual de la plantilla para que pueda ser reutilizada por otros usuarios. Estas imágenes se guardan en Azure [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md). Como primer paso, el Administrador de laboratorio asocia una galería de imágenes compartidas existentes a la cuenta de laboratorio. Una vez que se adjunta a la Galería de imágenes compartidas, laboratorio creado en la cuenta de laboratorio puede guardar imágenes en la Galería de imágenes compartidas. Otros profesores pueden seleccionar esta imagen de la Galería de imágenes compartidas para crear una plantilla para sus clases. 

## <a name="prerequisites"></a>Requisitos previos
Crear una galería de imágenes compartidas mediante [Azure PowerShell](../../virtual-machines/windows/shared-images.md) o [CLI de Azure](../../virtual-machines/linux/shared-images.md).

## <a name="attach-a-shared-image-gallery-to-a-lab-account"></a>Adjuntar una galería de imágenes compartidas a una cuenta de laboratorio
El siguiente procedimiento muestra cómo adjuntar una galería de imágenes compartidas a una cuenta de laboratorio. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda. Seleccione **Lab Services** en la sección **DEVOPS**. Si selecciona la estrella (`*`) junto a **Lab Services**, se agrega a la sección **FAVORITOS** en el menú de la izquierda. A partir de la próxima vez, seleccione **Lab Services** en **FAVORITOS**.

    ![Todos los servicios -> Lab Services](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Seleccione la cuenta de laboratorio para ver el **cuenta de laboratorio** página. 
4. Seleccione **Galería de imágenes compartidas** en el menú de la izquierda y seleccione **adjuntar** en la barra de herramientas. 

    ![Compartir la Galería de imágenes: Agregar botón](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. En el **adjuntar una galería de imágenes existente Shared** , seleccione la Galería de imágenes compartidas y seleccione **Aceptar**.

    ![Seleccione una galería existente](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. Vea la siguiente pantalla: 

    ![Mi galería en la lista](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    En este ejemplo, no hay ninguna imagen en la Galería de imágenes compartidas todavía.

Identidad de Azure Lab Services se agrega como colaborador a la Galería de imágenes compartidas que se adjunta al laboratorio. Permite a los profesores / imágenes del Administrador de TI para guardar la máquina virtual a la Galería de imágenes compartidas. Todos los laboratorios creados en esta cuenta de laboratorio tienen acceso a la Galería de imágenes compartidas adjunto. 

Todas las imágenes en la Galería de imágenes compartidas conectados están habilitadas de forma predeterminada. Puede habilitar o deshabilitar las imágenes seleccionadas seleccionándolos en la lista y utilizando el **habilitar las imágenes seleccionadas** o **deshabilitar las imágenes seleccionadas** botón. 

## <a name="detach-a-shared-image-gallery"></a>Desasociar una galería de imágenes compartidas
Solo una galería de imágenes compartidas puede asociarse a un laboratorio. Si desea adjuntar otro Galería de imágenes compartidas, separar lo actual antes de adjuntar una nueva. Para desasociar una galería de imágenes compartidas en el laboratorio, seleccione **Detach** en la barra de herramientas y confirme la operación de desasociación. 

## <a name="save-an-image-to-the-shared-image-gallery"></a>Guardar una imagen en la Galería de imágenes compartidas
Una vez que se adjunta una galería de imágenes compartidas, un profesor puede guardar una imagen de plantilla en la Galería de imágenes compartidas para que se pueda reutilizar en otros profesores.

![Guardar imagen de máquina virtual en la Galería](../media/how-to-use-shared-image-gallery/save-virtual-machine.png)

## <a name="use-an-image-from-the-shared-image-gallery"></a>Usar una imagen desde la Galería de imágenes compartidas
Un profesor puede elegir una imagen personalizada disponible en la Galería de imágenes compartidas para la plantilla durante la creación del nuevo laboratorio.

![Usar la imagen de máquina virtual desde la Galería](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información acerca de las galerías de imágenes compartidas, consulte [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md).
