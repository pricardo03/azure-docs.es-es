---
title: Usar una galería de imágenes compartidas en Azure Lab Services | Microsoft Docs
description: Aprenda a configurar una cuenta de laboratorio para usar una galería de imágenes compartidas para que un usuario pueda compartir una imagen con otro, y otro usuario pueda usar la imagen para crear una plantilla de máquina virtual en el laboratorio.
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
ms.date: 09/05/2019
ms.author: spelluru
ms.openlocfilehash: 65cc2d9ac2b96822f2c1b740f3180ba1d9eaf98c
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70390007"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Usar una galería de imágenes compartidas en Azure Lab Services
En este artículo se explica cómo los profesores y administradores del laboratorio cómo pueden guardar una imagen de máquina virtual de plantilla para que otros usuarios puedan reutilizarla. Estas imágenes se guardan en una [galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md) de Azure. Como primer paso, el administrador del laboratorio asocia una galería de imágenes compartidas existente a la cuenta de laboratorio. Una vez que se ha asociado la galería de imágenes compartidas, los laboratorios creados en la cuenta de laboratorio pueden guardar imágenes en la galería de imágenes compartidas. Otros profesores pueden seleccionar esta imagen de la galería de imágenes compartidas para crear una plantilla para sus clases. 

## <a name="prerequisites"></a>Requisitos previos
- Tener creada una galería de imágenes compartidas con [Azure PowerShell](../../virtual-machines/windows/shared-images.md) o con la [CLI de Azure](../../virtual-machines/linux/shared-images.md).
- Haber asociado la galería de imágenes compartidas a la cuenta de laboratorio. Para obtener instrucciones detalladas, vea [Asociar o desasociar una galería de imágenes compartidas en Azure Lab Services](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Guardar una imagen en la galería de imágenes compartidas
Una vez que se ha asociado una galería de imágenes compartidas, un administrador de cuenta de laboratorio o un profesor puede guardar una imagen en la galería de imágenes compartidas para que otros profesores la puedan reutilizar. 

1. En la página principal del laboratorio, seleccione **Save image** (Guardar imagen) en el icono de la sección **Template** (Plantilla).

    ![Botón Save image (Guardar imagen)](../media/how-to-use-shared-image-gallery/save-image-button.png)
2.  En la ventana **Save virtual machine image** (Guardar imagen de máquina virtual), escriba un nombre para la imagen y seleccione **Save** (Guardar). 

    ![Ventana Save virtual machine image (Guardar imagen de máquina virtual)](../media/how-to-use-shared-image-gallery/save-virtual-machine-image.png)
3. Compruebe el estado en el icono de laboratorio. 

    ![Estado de la operación de guardar la imagen](../media/how-to-use-shared-image-gallery/save-image-status.png)
4. Confirme que la operación se ha realizado correctamente.

    ![Operación de guardar imagen correcta](../media/how-to-use-shared-image-gallery/save-image-successful.png)

También puede cargar una imagen en la galería de imágenes compartidas fuera del contexto de un laboratorio. Para más información, consulte [Introducción a la galería de imágenes compartidas](../../virtual-machines/windows/shared-images.md). 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Usar una imagen de la galería de imágenes compartidas
Un profesor puede elegir una imagen personalizada disponible en la galería de imágenes compartidas para la plantilla durante la creación de un laboratorio.

![Usar una imagen de máquina virtual de la galería](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las galerías de imágenes compartidas, vea [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md).
