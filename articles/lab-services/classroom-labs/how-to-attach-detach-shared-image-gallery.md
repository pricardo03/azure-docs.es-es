---
title: Asociar o desasociar una galería de imágenes compartidas en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo adjuntar una galería de imágenes compartidas a un laboratorio en Azure Lab Services.
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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: de4e9fb4b15f4c346926fe46f23255c668204c2e
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413889"
---
# <a name="attach-or-detach-a-shared-image-gallery-in-azure-lab-services"></a>Asociar o desasociar una galería de imágenes compartidas en Azure Lab Services
Administrador de profesores/laboratorio puede guardar una imagen de máquina virtual de plantilla en Azure [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md) para que pueda ser reutilizada por otros usuarios. Como primer paso, el Administrador de laboratorio asocia una galería de imágenes compartidas existentes a la cuenta de laboratorio. Una vez que se adjunta a la Galería de imágenes compartidas, laboratorio creado en la cuenta de laboratorio puede guardar imágenes en la Galería de imágenes compartidas. Otros profesores pueden seleccionar esta imagen de la Galería de imágenes compartidas para crear una plantilla para sus clases. 

Este artículo muestra cómo asociar o desasociar una galería de imágenes compartidas en una cuenta de laboratorio. 

## <a name="configure-at-the-time-of-lab-account-creation"></a>Configurar en el momento de creación de la cuenta de laboratorio
Al crear una cuenta de laboratorio, puede adjuntar una galería de imágenes compartidas para la cuenta de laboratorio. Puede seleccionar una galería de imágenes compartidas existentes en la lista desplegable o crear uno nuevo. Para crear y adjuntar una galería de imágenes compartidas para la cuenta de laboratorio, seleccione **crear nuevo**, escriba un nombre para la galería y escriba **Aceptar**. 

![Configurar la Galería de imágenes compartidas en el momento de creación de la cuenta de laboratorio](../media/how-to-use-shared-image-gallery/new-lab-account.png)

## <a name="configure-after-the-lab-account-is-created"></a>Configurar una vez creada la cuenta de laboratorio
Una vez creada la cuenta de laboratorio, puede realizar las tareas siguientes:

- Crear y adjuntar una galería de imágenes compartidas
- Adjuntar una galería de imágenes compartidas para la cuenta de laboratorio
- Desasociar una galería de imágenes compartidas de la cuenta de laboratorio

## <a name="create-and-attach-a-shared-image-gallery"></a>Crear y adjuntar una galería de imágenes compartidas
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda. Seleccione **Lab Services** en la sección **DEVOPS**. Si selecciona la estrella (`*`) junto a **Lab Services**, se agrega a la sección **FAVORITOS** en el menú de la izquierda. A partir de la próxima vez, seleccione **Lab Services** en **FAVORITOS**.

    ![Todos los servicios -> Lab Services](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Seleccione la cuenta de laboratorio para ver el **cuenta de laboratorio** página. 
4. Seleccione **Galería de imágenes compartidas** en el menú de la izquierda y seleccione **+ crear** en la barra de herramientas.  

    ![Crear el botón de la Galería de imágenes compartidas](../media/how-to-use-shared-image-gallery/new-shared-image-gallery-button.png)
5. En el **crear Galería de imágenes compartidas** ventana, escriba un **nombre** para la galería y escriba **Aceptar**. 

    ![Crear ventana de la Galería de imágenes compartidas](../media/how-to-use-shared-image-gallery/create-shared-image-gallery-window.png)

    Azure Lab Services crea la Galería de imágenes compartidas y lo adjunta a la cuenta de laboratorio. Todos los laboratorios creados en esta cuenta de laboratorio tienen acceso a la Galería de imágenes compartidas adjunto. 

    ![Galería de la imagen adjunta](../media/how-to-use-shared-image-gallery/image-gallery-in-list.png)

    En el panel inferior, verá imágenes en la Galería de imágenes compartidas. En esta galería nueva, no hay ninguna imagen. Al cargar imágenes en la galería, verlos en esta página.     

    Todas las imágenes en la Galería de imágenes compartidas conectados están habilitadas de forma predeterminada. Puede habilitar o deshabilitar las imágenes seleccionadas seleccionándolos en la lista y utilizando el **habilitar las imágenes seleccionadas** o **deshabilitar las imágenes seleccionadas** botón.

## <a name="attach-an-existing-shared-image-gallery"></a>Adjuntar una galería de imágenes compartidas existentes
El siguiente procedimiento muestra cómo adjuntar una galería de imágenes compartidas existente a una cuenta de laboratorio. 

1. En el **cuenta de laboratorio** página, seleccione **Galería de imágenes compartidas** en el menú de la izquierda y seleccione **adjuntar** en la barra de herramientas. 

    ![Compartir la Galería de imágenes: Agregar botón](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. En el **adjuntar una galería de imágenes existente Shared** , seleccione la Galería de imágenes compartidas y seleccione **Aceptar**.

    ![Seleccione una galería existente](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. Vea la siguiente pantalla: 

    ![Mi galería en la lista](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    En este ejemplo, no hay ninguna imagen en la Galería de imágenes compartidas todavía.

    Identidad de Azure Lab Services se agrega como colaborador a la Galería de imágenes compartidas que se adjunta al laboratorio. Permite a los profesores / imágenes del Administrador de TI para guardar la máquina virtual a la Galería de imágenes compartidas. Todos los laboratorios creados en esta cuenta de laboratorio tienen acceso a la Galería de imágenes compartidas adjunto. 

    Todas las imágenes en la Galería de imágenes compartidas conectados están habilitadas de forma predeterminada. Puede habilitar o deshabilitar las imágenes seleccionadas seleccionándolos en la lista y utilizando el **habilitar las imágenes seleccionadas** o **deshabilitar las imágenes seleccionadas** botón. 

## <a name="save-an-image-to-the-shared-image-gallery"></a>Guardar una imagen en la Galería de imágenes compartidas
Una vez que se adjunta una galería de imágenes compartidas, un administrador de cuenta de laboratorio o un profesor puede guardar o cargar una imagen en la Galería de imágenes compartidas para que se pueda reutilizar en otros profesores. Para obtener instrucciones para cargar una imagen en la Galería de imágenes compartidas, consulte [información general de la Galería de imágenes compartidas](../../virtual-machines/windows/shared-images.md). 

> [!NOTE]
> Actualmente, la interfaz de usuario (UI) de laboratorios de clase no admite guardar una imagen de laboratorio en la Galería de imágenes compartidas. 

## <a name="detach-a-shared-image-gallery"></a>Desasociar una galería de imágenes compartidas
Solo una galería de imágenes compartidas puede asociarse a un laboratorio. Si desea adjuntar otro Galería de imágenes compartidas, separar lo actual antes de adjuntar una nueva. Para desasociar una galería de imágenes compartidas en el laboratorio, seleccione **Detach** en la barra de herramientas y confirme la operación de desasociación. 

![Desasociar de la Galería de imágenes compartidas de la cuenta de laboratorio](../media/how-to-use-shared-image-gallery/detach.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener información acerca de cómo guardar una imagen de laboratorio en la Galería de imágenes compartidas o utilizar una imagen desde la Galería de imágenes compartidas para crear una máquina virtual, consulte [cómo usar la Galería de imágenes compartidas](how-to-use-shared-image-gallery.md).

Para obtener más información sobre los compartidos galerías de imágenes en general, consulte [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md).
