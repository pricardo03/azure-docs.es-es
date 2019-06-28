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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 8d8b6fffe197d4180b091518dcd1615d0e0b9d19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65412850"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Usar una galería de imágenes compartidas en Azure Lab Services
En este artículo se explica cómo los profesores y administradores del laboratorio cómo pueden guardar una imagen de máquina virtual de plantilla para que otros usuarios puedan reutilizarla. Estas imágenes se guardan en una [galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md) de Azure. Como primer paso, el administrador del laboratorio asocia una galería de imágenes compartidas existente a la cuenta de laboratorio. Una vez que se ha asociado la galería de imágenes compartidas, los laboratorios creados en la cuenta de laboratorio pueden guardar imágenes en la galería de imágenes compartidas. Otros profesores pueden seleccionar esta imagen de la galería de imágenes compartidas para crear una plantilla para sus clases. 

## <a name="prerequisites"></a>Requisitos previos
- Tener creada una galería de imágenes compartidas con [Azure PowerShell](../../virtual-machines/windows/shared-images.md) o con la [CLI de Azure](../../virtual-machines/linux/shared-images.md).
- Haber asociado la galería de imágenes compartidas a la cuenta de laboratorio. Para obtener instrucciones detalladas, vea [Asociar o desasociar una galería de imágenes compartidas en Azure Lab Services](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Guardar una imagen en la galería de imágenes compartidas
Una vez que se ha asociado una galería de imágenes compartidas, un profesor puede guardar o cargar una imagen a la galería de imágenes compartidas para que otros profesores la puedan reutilizar. Para obtener instrucciones sobre cómo cargar una imagen en la galería de imágenes compartidas, vea [Información general de la galería de imágenes compartidas](../../virtual-machines/windows/shared-images.md). 

> [!NOTE]
> Actualmente, la interfaz de usuario (IU) de Laboratorios educativos no permite que se guarde una imagen de laboratorio en la galería de imágenes compartidas. 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Usar una imagen de la galería de imágenes compartidas
Un profesor puede elegir una imagen personalizada disponible en la galería de imágenes compartidas para la plantilla durante la creación de un laboratorio.

![Usar una imagen de máquina virtual de la galería](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las galerías de imágenes compartidas, vea [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md).
