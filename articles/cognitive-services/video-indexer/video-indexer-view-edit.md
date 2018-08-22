---
title: Visualización y edición de la información detallada de Azure Video Indexer | Microsoft Docs
description: En este tema se muestra cómo ver y editar la información detallada de Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 797c09d72402cfc1ee2524e7792cc1310a53fb1e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399484"
---
# <a name="view-and-edit-video-indexer-insights"></a>Visualización y edición de la información detallada de Video Indexer

En este tema se muestra cómo ver y editar la información detallada de Video Indexer de un vídeo.

1. Inicie sesión en la cuenta de [Video Indexer](https://api-portal.videoindexer.ai/).
2. Busque un vídeo desde el que va a crear la información detallada de Video Indexer. Para más información, consulte [Búsqueda de momentos exactos dentro de vídeos](video-indexer-search.md).
3. Presione **Reproducir**.

    La página muestra la información resumida del vídeo. 

    ![Información detallada](./media/video-indexer-view-edit/video-indexer-summarized-insights.png)

4. Vea la información resumida del vídeo. 

    La información resumida muestra una vista agregada de los datos: caras, palabras clave, opiniones. Por ejemplo, puede ver las caras de las personas y los intervalos de tiempo en los que aparece cada cara y el porcentaje del tiempo que se muestra.

    El reproductor y la información detallada están sincronizados. Por ejemplo, si hace clic en una palabra clave o en la línea de transcripción, el reproductor le lleva a ese momento del vídeo. Puede lograr la vista del reproductor o información detallada y la sincronización en la aplicación. Para más información, consulte [Inserción de widgets de Azure Video Indexer en la aplicación](video-indexer-embed-widgets.md). 

3. Edite la información detallada de Video Indexer.

    Presione Edit (Editar) en el vídeo. La página que muestra el desglose completo de un vídeo. El desglose se divide en bloques. Los bloques sirven para facilitar el recorrido por los datos. Por ejemplo, el bloque puede dividirse en función de cuándo cambian los oradores o de si hay una pausa larga. Puede crear su propia lista de reproducción que contenga solo las líneas que desee. Para mostrar solo partes específicas del vídeo de origen, puede filtrar por temas o palabras clave, opiniones, personas u oradores. Puede elegir ver solo la transcripción del vídeo o un reconocimiento óptico de caracteres.  

    ![Información detallada](./media/video-indexer-view-edit/video-indexer-create-new-playlist.png)

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre cómo crear su propia información detallada de Video Indexer basada en otro vídeo](video-indexer-create-new.md).

## <a name="see-also"></a>Otras referencias

[Introducción a Video Indexer](video-indexer-overview.md)

