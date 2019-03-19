---
title: 'Uso de etiquetas personalizadas para moderación de contenido: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Content Moderator incluye etiquetas predeterminadas, pero puede crear etiquetas personalizadas para moderar el contenido específico de su negocio.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: fa3b7be6ac7cc398685bf5ad7d21173bcb072b69
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58118415"
---
# <a name="create-and-use-moderation-tags"></a>Creación y uso de etiquetas de moderación

Además de las dos etiquetas predeterminadas **isadult** (**a**) e **isracy** (**r**), puede crear etiquetas personalizadas para un examen más dirigido. Estas etiquetas personalizadas están luego disponibles para que los revisores humanos las asignen a imágenes o texto.

## <a name="create-tags"></a>Creación de etiquetas

1. Seleccione las etiquetas en la pestaña Settings (Configuración).

   ![Etiquetas de moderación de contenido](images/tags-1.png)

2. Escriba un código corto de dos letras para la etiqueta.
3. Escriba un nombre para la etiqueta. Mantenga el nombre corto y descriptivo. Por ejemplo, **isbullying**.
4. Escriba una descripción.
5. Haga clic en Agregar.
6. Cuando haya terminado de crear las etiquetas, haga clic en Save (Guardar).

![Definición de etiquetas de moderación de contenido](images/tags-2-define.png)

## <a name="using-custom-tags"></a>Uso de etiquetas personalizadas

Se pueden usar etiquetas personalizadas durante la revisión humana. Se muestran en la vista previa y el revisor las selecciona haciendo clic en ellas.

![Uso de etiquetas de moderación de contenido](images/tags-3-use.png)

Puede desactivar distintas etiquetas para distintas revisiones con solo activar o desactivar su visibilidad.
 
![Deshabilitación de etiquetas de moderación de contenido](images/tags-4-disable.png)

Aunque no puede eliminar las dos etiquetas predeterminadas, **isadult** e **isracy**, puede eliminar cualquier etiqueta personalizada que haya definido. Haga clic en la papelera junto a la etiqueta que quiere eliminar.

![Eliminación de etiquetas de moderación de contenido](images/tags-5-delete.png)

## <a name="next-steps"></a>Pasos siguientes

Para información sobre cómo usar etiquetas para la moderación de imágenes, consulte [Revisión de imágenes moderadas](Review-Moderated-Images.md).
