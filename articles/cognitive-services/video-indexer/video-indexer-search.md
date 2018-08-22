---
title: Uso de Azure Video Indexer para la búsqueda de momentos exactos dentro de vídeos | Microsoft Docs
description: En este tema se muestra cómo buscar momentos exactos dentro de vídeos.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 1cffa067d8028adab4dbcc82c529f77d980ce6be
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397550"
---
# <a name="find-exact-moments-within-videos"></a>Búsqueda de momentos exactos dentro de vídeos

En este tema se muestran las opciones de búsqueda que le permiten encontrar momentos exactos dentro de vídeos.

1. Inicie sesión en la cuenta de [Video Indexer](https://api-portal.videoindexer.ai/).
2. Busque en todos los vídeos de la cuenta.

    En el ejemplo siguiente, hemos buscado todos los vídeos creados por Channel 9 con Scott Hanselman.

    ![Search](./media/video-indexer-search/video-indexer-search01.png)
    
3. Busque la información resumida del vídeo.

    Después, puede buscar en un vídeo haciendo clic en **Reproducir** en el vídeo. A continuación, puede buscar en el vídeo seleccionando la pestaña **Buscar**. Por ejemplo, hemos buscado todos los lugares donde se utiliza el texto "identity protection". 

    ![Search](./media/video-indexer-search/video-indexer-search02.png)

    Si se hace clic en uno de los resultados, el reproductor le llevará a ese momento en el vídeo. Puede lograr la vista del reproductor o información detallada y la sincronización en la aplicación. Para más información, consulte [Inserción de widgets de Video Indexer en las aplicaciones](video-indexer-embed-widgets.md). 

    
4. Busque el desglose detallado del vídeo.

    Si desea crear su propio desglose basado en el vídeo que ha encontrado, presione el botón **Editar**. En esta página se muestra el desglose completo de un vídeo. Puede buscar dentro del desglose para mostrar solo las líneas en las que esté interesado. Para más información, consulte [Visualización y edición de la información detallada de Video Indexer](video-indexer-view-edit.md).

    En este ejemplo, hemos buscado el texto "identity protection". También se aplican filtros adicionales, como se muestra en la siguiente pantalla.

    ![Search](./media/video-indexer-search/video-indexer-search03.png)

## <a name="next-steps"></a>Pasos siguientes 

Cuando se encuentre el vídeo con el que desea trabajar, puede continuar procesando el vídeo, tal y como se describe en uno de estos temas: 

- [Creación de información detallada de vídeo a partir de vídeos existentes](video-indexer-create-new.md)
- [Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
- [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Otras referencias

[Introducción a Video Indexer](video-indexer-overview.md)
