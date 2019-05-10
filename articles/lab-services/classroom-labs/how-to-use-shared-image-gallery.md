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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 8d8b6fffe197d4180b091518dcd1615d0e0b9d19
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412850"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Use una galería de imágenes compartidas en Azure Lab Services
Este artículo muestra cómo los profesores/laboratorio administrador puede guardar una imagen de máquina virtual de la plantilla para que pueda ser reutilizada por otros usuarios. Estas imágenes se guardan en Azure [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md). Como primer paso, el Administrador de laboratorio asocia una galería de imágenes compartidas existentes a la cuenta de laboratorio. Una vez que se adjunta a la Galería de imágenes compartidas, laboratorio creado en la cuenta de laboratorio puede guardar imágenes en la Galería de imágenes compartidas. Otros profesores pueden seleccionar esta imagen de la Galería de imágenes compartidas para crear una plantilla para sus clases. 

## <a name="prerequisites"></a>Requisitos previos
- Crear una galería de imágenes compartidas mediante [Azure PowerShell](../../virtual-machines/windows/shared-images.md) o [CLI de Azure](../../virtual-machines/linux/shared-images.md).
- La Galería de imágenes compartidas se ha vinculado a la cuenta de laboratorio. Para obtener instrucciones detalladas, consulte [asociar o desasociar Galería de imágenes compartidas](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Guardar una imagen en la Galería de imágenes compartidas
Una vez que se adjunta una galería de imágenes compartidas, un profesor puede guardar o cargar una imagen en la Galería de imágenes compartidas para que se pueda reutilizar en otros profesores. Para obtener instrucciones para cargar una imagen en la Galería de imágenes compartidas, consulte [información general de la Galería de imágenes compartidas](../../virtual-machines/windows/shared-images.md). 

> [!NOTE]
> Actualmente, la interfaz de usuario (UI) de laboratorios de clase no admite guardar una imagen de laboratorio en la Galería de imágenes compartidas. 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Usar una imagen desde la Galería de imágenes compartidas
Un profesor puede elegir una imagen personalizada disponible en la Galería de imágenes compartidas para la plantilla durante la creación del nuevo laboratorio.

![Usar la imagen de máquina virtual desde la Galería](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información acerca de las galerías de imágenes compartidas, consulte [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md).
