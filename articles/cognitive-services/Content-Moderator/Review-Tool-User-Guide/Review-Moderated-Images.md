---
title: 'Revisión de imágenes etiquetadas: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Información sobre cómo la herramienta de revisión permite a los moderadores humanos revisar imágenes en un portal web.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: e096e65e3016f33361f772a75ab8f71603970a5f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58096612"
---
# <a name="let-human-reviewers-review-images"></a>Permitir que revisores humanos revisen imágenes

Después de registrarse en Content Moderator y obtener una clave de suscripción, puede probar las características de revisión de imágenes.

1. Abra la [herramienta de revisión humana](https://contentmoderator.cognitive.microsoft.com/) e inicie sesión. 
2. Haga clic en la pestaña Probar y cargue algunas imágenes para revisar.
3. Haga clic en la pestaña Revisar y seleccione Imagen.

   ![Explorador Chrome que muestra la herramienta de revisión con la opción de revisión de imágenes resaltada](images/review-images-1.png)

   Las imágenes se muestran con las etiquetas asignadas por la herramienta de revisión. Las imágenes no están disponibles para otros revisores de su equipo mientras las esté revisando.

4. Mueva el control deslizante "Reviews to display" (Revisiones para mostrara para mostrar) (1) para ajustar el número de imágenes que se muestran en la pantalla. Haga clic en los botones etiquetados o no etiquetados (2) para ordenar las imágenes en consecuencia. Haga clic en una etiqueta (3) para activarla o desactivarla.

   ![Explorador Chrome que muestra la herramienta de revisión con imágenes etiquetadas para su revisión](images/review-images-2.png)
 
5. Para más información acerca de una imagen, haga clic en el botón de puntos suspensivos en una vista en miniatura, seguido por la opción **Ver detalles**. Para asignar la imagen a un equipo secundario, seleccione la opción **Mover a**.
 
   ![Imagen con la opción Ver detalles resaltada](images/review-images-3.png)

6. Examine la información de moderación de imágenes en la página de detalles.

   ![Imagen con los detalles de moderación en un panel independiente](images/review-images-4.png)
 
7. Una vez que haya revisado y actualizado las asignaciones de etiqueta según sea necesario, haga clic en **Siguiente** para enviar sus revisiones.

Una vez enviado, tiene unos cinco segundos para hacer clic en el botón **Anterior** para volver a la pantalla anterior y revisar las imágenes de nuevo. Después de eso, las imágenes ya no estarán en la cola de envío y el botón **Anterior** ya no estará disponible.
