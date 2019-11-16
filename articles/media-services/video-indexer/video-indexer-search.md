---
title: 'Búsqueda de momentos exactos en los vídeos: Video Indexer'
titleSuffix: Azure Media Services
description: En este tema se muestra cómo buscar momentos exactos en los vídeos mediante Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 8ebf7606d1a0932cc2254e14a40a2fff550fd35f
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73833790"
---
# <a name="find-exact-moments-within-videos"></a>Búsqueda de momentos exactos dentro de vídeos

En este tema se muestran las opciones de búsqueda que le permiten encontrar momentos exactos dentro de vídeos.

1. Vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) e inicie sesión.
2. Busque en todos los vídeos de la cuenta.

    En el siguiente ejemplo buscamos todos los vídeos que tratan sobre la seguridad y en los que aparece Satya.

    ![Search](./media/video-indexer-search/video-indexer-search01.png)
3. Busque la información resumida del vídeo.

    Después, puede buscar en un vídeo haciendo clic en **Reproducir** en el vídeo. A continuación, puede buscar en el vídeo seleccionando la pestaña **Buscar**. 

    En el siguiente ejemplo buscamos "secure" en el vídeo seleccionado.

    ![Search](./media/video-indexer-search/video-indexer-search02.png)

    Si se hace clic en uno de los resultados, el reproductor le llevará a ese momento en el vídeo. Puede lograr la vista del reproductor o información detallada y la sincronización en la aplicación. Para más información, consulte [Inserción de widgets de Video Indexer en las aplicaciones](video-indexer-embed-widgets.md). 
4. Busque el desglose detallado del vídeo.
    
    Si desea crear su propio clip basado en el vídeo que ha encontrado, presione el botón **Editar**. En esta página se muestra el vídeo junto con su información como filtros. Para más información, consulte [Visualización y edición de la información detallada de Video Indexer](video-indexer-view-edit.md). 

    Puede buscar dentro del vídeo para mostrar únicamente las líneas que le interesen y usar la información lateral para filtrar las partes que desee ver. Cuando haya terminado, puede obtener la vista previa del clip y presionar **publicar** para crear el clip que aparecerá en la galería.
    
    En el siguiente ejemplo se busca el texto "mixed reality". También se aplican filtros adicionales, como se muestra en la siguiente pantalla.
    
    ![Search](./media/video-indexer-search/video-indexer-search03.png)

## <a name="next-steps"></a>Pasos siguientes 

Cuando se encuentre el vídeo con el que desea trabajar, puede continuar procesando el vídeo, tal y como se describe en uno de estos temas: 

- [Uso de información detallada propia en los vídeos](use-editor-create-project.md)
- [Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
- [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Otras referencias

[Introducción a Video Indexer](video-indexer-overview.md)
