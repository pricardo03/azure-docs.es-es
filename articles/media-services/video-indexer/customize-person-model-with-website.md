---
title: 'Uso del sitio web de Video Indexer para personalizar un modelo de persona: Azure'
titlesuffix: Azure Media Services
description: En este artículo se muestra cómo personalizar un modelo de persona con el sitio web de Video Indexer.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: anzaman
ms.openlocfilehash: 29c32126a5bbe792dd6fe88ae189e09c511d2a22
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997074"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Personalización de un modelo de persona con el sitio web de Video Indexer

Video Indexer admite la detección de caras y el reconocimiento de celebridades en contenido de vídeo. La característica de reconocimiento de celebridades abarca aproximadamente un millón de caras basadas en orígenes de datos comúnmente solicitados, como IMDB, Wikipedia y personas con más influencia de LinkedIn. Las caras que no reconoce la característica de reconocimiento de celebridades se detectan, pero se dejan sin nombre. Después de cargar un vídeo en Video Indexer y recibir los resultados, puede volver y poner nombre a las caras que no se reconocieron. Cuando se etiqueta una cara con un nombre, la cara y el nombre se agregan al modelo de persona de la cuenta. Así, Video Indexer reconocerá esta cara en los vídeos futuros y pasados.

Puede usar el sitio web de Video Indexer para editar las caras que se detectaron en un vídeo, como se describe en este tema. También puede usar la API, como se describe en [Personalización del modelo de persona mediante las API](customize-person-model-with-api.md).

## <a name="edit-a-face"></a>Edición de una cara

> [!NOTE]
> Los nombres son únicos para los modelos de persona, así que si da a dos caras diferentes el mismo nombre, Video Indexer considera que las caras son de la misma persona y las hace coincidir después de volver a indexar el vídeo. Si ve que Video Indexer ha detectado varias apariciones distintas de la misma cara, asígneles el mismo nombre y vuelva a indexar el vídeo.
> Puede actualizar una cara que Video Indexer haya reconocido como una celebridad con un nuevo nombre. El nuevo nombre que proporcione tendrá prioridad sobre el reconocimiento de celebridades integrado.

1. Vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) e inicie sesión.
2. Busque un vídeo que quiera ver y edítelo en su cuenta.
3. Para editar una cara del vídeo, vaya a la pestaña **Insights** y haga clic en el icono de lápiz en la esquina superior derecha de la ventana.

    ![Personalización del modelo de persona](./media/customize-face-model/customize-face-model.png)

4. Haga clic en cualquiera de las caras detectadas y cambie sus nombres "#X desconocido", o los que asignara previamente a la cara, por otros.
5. Después de escribir el nuevo nombre, haga clic en el icono de verificación junto a él. Esta acción guarda el nuevo nombre, y todas las apariciones de esta cara en otros vídeos actuales y en los vídeos futuros que cargue se reconocerán y nombrarán. El reconocimiento de la cara en los otros vídeos actuales puede tardar algún tiempo en aplicarse ya que se trata de un proceso por lotes. 

    ![Guardar la actualización](./media/customize-face-model/save-update.png)

## <a name="delete-a-face"></a>Eliminación de un archivo

Para eliminar una cara detectada en el vídeo, vaya al panel **Insights** y haga clic en el icono de lápiz en la esquina superior derecha del panel. Haga clic en la opción **Delete** (Eliminar) debajo del nombre de la cara. Esta acción quitará esa cara detectada del vídeo. La cara se seguirá detectando en los otros vídeos en los que aparezca, pero también puede eliminar la cara de esos vídeos después de que se hayan indexado. La cara también seguirá existiendo en el modelo de persona de su cuenta si le asignó un nombre antes de eliminarla.

## <a name="next-steps"></a>Pasos siguientes

[Personalización del modelo de persona mediante las API](customize-person-model-with-api.md)
