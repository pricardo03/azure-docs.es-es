---
title: Usar las revisiones de contenido a través de la herramienta de revisión - Content Moderator
titlesuffix: Azure Cognitive Services
description: Información sobre cómo la herramienta de revisión permite a los moderadores humanos revisar imágenes en un portal web.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/15/2019
ms.author: sajagtap
ms.openlocfilehash: a482ecf4a0d321525ab7e392695d2c4c0eebeadc
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758485"
---
# <a name="create-human-reviews"></a>Crear revisiones humanas

En esta guía, obtendrá información sobre cómo configurar [revisa](../review-api.md#reviews) en el sitio Web de herramienta de revisión. Las revisiones de almacenan y mostrar el contenido para que moderadores humanos evaluar. Los moderadores pueden modificar las etiquetas aplicadas y aplicar sus propias etiquetas personalizadas según corresponda. Cuando un usuario completa una revisión, los resultados se envían a un extremo de devolución de llamada especificado y se quitará el contenido del sitio.

## <a name="prerequisites"></a>Requisitos previos

- Iniciar sesión o crear una cuenta de Content Moderator [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) sitio.

## <a name="image-reviews"></a>Revisiones de imágenes

1. Vaya a la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/), seleccione el **intente** y a cargar algunas imágenes para revisar.
1. Una vez hayan acabado de procesar las imágenes subidas, vaya a la **revisión** pestaña y seleccione **imagen**.

    ![Explorador Chrome que muestra la herramienta de revisión con la opción de revisión de imágenes resaltada](images/review-images-1.png)

    Se mostrarán las imágenes con las etiquetas que se han asignado por el proceso de moderación automática. Las imágenes que ha enviado a través de la herramienta de revisión no son visibles para otros revisores.

1. También puede mover el **revisiones para mostrar** (1) de control deslizante para ajustar el número de imágenes que se muestran en la pantalla. Haga clic en el **etiquetadas** o **sin etiquetar** botones (2) para ordenar las imágenes según corresponda. Haga clic en un panel de etiquetas (3), activar o desactivar.

    ![Explorador Chrome que muestra la herramienta de revisión con imágenes etiquetadas para su revisión](images/review-images-2.png)

1. Para obtener más información sobre una imagen, haga clic en el botón de puntos suspensivos en la miniatura y seleccione **ver detalles**. Puede asignar una imagen a un equipo secundario con el **mover a** opción (consulte la [equipos](./configure.md#manage-team-and-subteams) sección para obtener más información sobre equipos secundarios).

    ![Imagen con la opción Ver detalles resaltada](images/review-images-3.png)

1. Examine la información de moderación de imágenes en la página de detalles.

    ![Imagen con los detalles de moderación en un panel independiente](images/review-images-4.png)

1. Una vez que haya revisado y actualizado las asignaciones de etiqueta según sea necesario, haga clic en **Siguiente** para enviar sus revisiones. Una vez enviado, tiene unos cinco segundos para hacer clic en el botón **Anterior** para volver a la pantalla anterior y revisar las imágenes de nuevo. Después de eso, las imágenes ya no estarán en la cola de envío y el botón **Anterior** ya no estará disponible.

## <a name="text-reviews"></a>Revisiones del texto

Texto de revisiones de la función de forma similar a las revisiones de la imagen. En lugar de cargar contenido, escribir o pegar en texto (hasta 1.024 caracteres). A continuación, Content Moderator analiza el texto y aplica etiquetas (además de otra información de moderación, como blasfemias y los datos personales). En las revisiones del texto, puede activar o desactivar las etiquetas aplicadas o aplicar etiquetas personalizadas antes de enviar la revisión.

![Captura de pantalla de la herramienta de revisión que muestra el texto marcado en una ventana del explorador Chrome](../images/reviewresults_text.png)

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido a configurar y usar las revisiones de Content Moderator [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com). A continuación, consulte el [Guía de la API de REST](../try-review-api-review.md) o [Guía del SDK de .NET](../moderation-reviews-quickstart-dotnet.md) para aprender a crear mediante programación las revisiones.