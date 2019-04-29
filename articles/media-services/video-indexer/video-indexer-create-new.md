---
title: 'Creación de información detallada de vídeo a partir de vídeos existentes: Azure'
titlesuffix: Azure Media Services
description: En este tema se muestra cómo crear y publicar información detallada de vídeos basada en archivos de vídeo existentes.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 4a65e88e3f94f64a56bde882b535030968ae354d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60560236"
---
# <a name="create-highlights-from-existing-videos"></a>Creación de información detallada a partir de vídeos existentes

En este tema se muestra cómo crear y publicar información detallada de vídeo basada en algún otro vídeo.

1. Vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) e inicie sesión.
2. Busque un vídeo desde el que va a crear la información detallada.
3. Presione **Reproducir**.

    La página muestra la información resumida del vídeo. 

    ![Información detallada](./media/video-indexer-create-new/video-indexer-summarized-insights.png)
3. Presione el botón **Editar**.

    En esta página se muestra el desglose completo de un vídeo. El desglose se divide en bloques. Los bloques sirven para facilitar el recorrido por los datos. Por ejemplo, el bloque puede dividirse en función de cuándo cambian los oradores o de si hay una pausa larga. Puede crear su propia lista de reproducción que contenga solo las líneas que desee. Para mostrar solo partes específicas del vídeo de origen, puede filtrar por temas o palabras clave, opiniones, personas u oradores. Puede elegir ver solo la transcripción del vídeo o un reconocimiento óptico de caracteres.    

    ![Información detallada](./media/video-indexer-create-new/video-indexer-create-new-playlist.png)
4. Cree la lista de reproducción.

    Para agregar o quitar líneas hacia y desde la lista de reproducción, presione **+**/**-**.
5. Obtenga una vista previa de la lista de reproducción.

    Cuando haya terminado de crear la lista de reproducción, presione **Preview** (Vista previa).
6. Publique la lista de reproducción.

    Después de obtener una vista previa de la lista de reproducción, puede publicarla.

    Cuando publica la lista de reproducción, se agrega a la lista de la información detallada del vídeo.


## <a name="next-steps"></a>Pasos siguientes 

Una vez creada la nueva lista de reproducción, puede seguir procesándola, tal como se describe en uno de estos temas: 

- [Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
- [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Vea también

[Introducción a Video Indexer](video-indexer-overview.md) 
