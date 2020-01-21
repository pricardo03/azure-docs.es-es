---
title: 'Suscripción a Video Indexer y carga del primer vídeo: Azure'
titleSuffix: Azure Media Services
description: Obtenga información acerca de cómo suscribirse y cargar el primer vídeo mediante el portal de Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: tutorial
ms.date: 01/13/2020
ms.author: juliako
ms.openlocfilehash: 3a5ddf5bd4614b68e97e7616173a3e0640007530
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75941553"
---
# <a name="quickstart-how-to-sign-up-and-upload-your-first-video"></a>Inicio rápido: Suscripción y carga del primer vídeo

En este tutorial introductorio se muestra cómo iniciar sesión en el sitio web de Video Indexer y cómo cargar su primer vídeo.

Al crear una cuenta de Video Indexer, puede elegir una cuenta de evaluación gratuita (donde obtendrá un número determinado de minutos gratuitos de indexación) o una opción de pago (donde no está limitado por la cuota). Con la versión de evaluación gratuita, Video Indexer proporciona hasta 600 horas de indexación gratuita a los usuarios de sitios web y hasta 2400 horas de indexación gratuita a los usuarios de API. Con la opción de pago, se crea una cuenta de Video Indexer que está [conectada a su suscripción de Azure y a una cuenta de Azure Media Services](connect-to-azure.md). Se paga por minutos de indexación y por los cargos relacionados con la cuenta de Azure Media Services. 

## <a name="sign-up-for-video-indexer"></a>Suscripción a Video Indexer

Para empezar a desarrollar con Video Indexer, vaya al sitio web de [Video Indexer](https://www.videoindexer.com) y suscríbase.

## <a name="upload-a-video-using-the-video-indexer-website"></a>Carga de un vídeo mediante el sitio web de Video Indexer

> [!NOTE]
> El nombre del vídeo no debe superar los 80 caracteres.

### <a name="supported-file-formats-for-video-indexer"></a>Formatos de archivo admitidos para Video Indexer

Consulte la sección [Formatos de archivo/contenedor de entrada](../latest/media-encoder-standard-formats.md#input-containerfile-formats) para una lista de los formatos de archivo que puede usar con Video Indexer.

### <a name="upload-a-video"></a>Carga de un vídeo

1. Inicie sesión en el sitio web de [Video Indexer](https://www.videoindexer.ai/).
2. Para cargar un vídeo, presione el botón **Upload** (Cargar) o el vínculo.

    ![Cargar](./media/video-indexer-get-started/video-indexer-upload.png)

    Una vez cargado el vídeo, Video Indexer inicia la indexación y el análisis del vídeo.

    ![Cargado](./media/video-indexer-get-started/video-indexer-uploaded.png) 

    Cuando Video Indexer ha terminado de analizar, recibirá una notificación con un vínculo a su vídeo y una breve descripción de lo que se ha encontrado en el vídeo. Por ejemplo: personas, temas u OCR.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Carga e indexación de los vídeos](upload-index-videos.md) para más detalles.

Tras la carga e indexación de un vídeo, puede empezar a usar el [sitio web de Video Indexer](video-indexer-view-edit.md) o el [Portal para desarrolladores de Video Indexer](video-indexer-use-apis.md) para ver la información detallada de este. 

## <a name="see-also"></a>Consulte también

[Introducción a Video Indexer](video-indexer-overview.md)

[Comience a usar las API](video-indexer-use-apis.md).

