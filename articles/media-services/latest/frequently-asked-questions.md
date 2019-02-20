---
title: Preguntas más frecuentes de Azure Media Services v3 | Microsoft Docs
description: En este artículo se ofrecen respuestas a las preguntas más frecuentes de Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/05/2019
ms.author: juliako
ms.openlocfilehash: a447c359c38c2173ea42b6d717067fc8b3a88f9a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875498"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Preguntas más frecuentes sobre Azure Media Services v3

En este artículo se ofrecen respuestas a las preguntas más frecuentes de Azure Media Services (AMS) v3.

## <a name="v3-apis"></a>API de la versión v3

### <a name="how-do-i-configure-media-reserved-units"></a>¿Cómo se pueden configurar las unidades reservadas de multimedia?

Para los trabajos de análisis de audio y vídeo desencadenados por Media Services v3 o Video Indexer, se recomienda encarecidamente aprovisionar la cuenta con 10 MRU S3. Si necesita más de 10 MRU S3, abra una incidencia de soporte técnico desde [Azure Portal](https://portal.azure.com/).

Para más información, consulte [Escalado de procesamiento de medios con la CLI](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>¿Cuál es el método recomendado para procesar vídeos?

Es recomendable que envíe los trabajos mediante una dirección URL de HTTP(s) que apunte al vídeo. Para más información, consulte [Ingesta HTTP(s)](job-input-from-http-how-to.md). No es necesario que cree un recurso con el vídeo de entrada antes de que este pueda procesarse.

### <a name="how-does-pagination-work"></a>¿Cómo funciona la paginación?

Al usar la paginación, siempre debe usar el vínculo siguiente para enumerar la colección y no tener que depender de un tamaño de página determinado. Para obtener información detallada y ejemplos, consulte [Filtrado, ordenación y paginación](entities-overview.md).

## <a name="live-streaming"></a>Streaming en directo 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>¿Cómo insertar pausas o vídeos y caretas de imagen durante una transmisión en directo?

La codificación en directo de Media Services v3 no admite aún la inserción de caretas de vídeo o de imagen durante una transmisión en directo. 

Puede usar un [codificador local en directo](recommended-on-premises-live-encoders.md) para cambiar el vídeo de origen. Muchas aplicaciones proporcionan la posibilidad de cambiar los orígenes como, por ejemplo, Telestream Wirecast, Switcher Studio (en iOS), OBS Studio (aplicación gratuita), etc.

## <a name="media-services-v2-vs-v3"></a>Comparación entre las versiones v2 y v3 de Media Services 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>¿Puedo usar Azure Portal para administrar los recursos de v3?

Todavía no. Puede usar uno de los SDK compatibles. Consulte los tutoriales y ejemplos de este conjunto de documentos.

### <a name="is-there-an-assetfile-concept-in-v3"></a>¿Existe el concepto de AssetFile en la versión v3?

Los AssetFiles se eliminaron de la API de AMS para separar la dependencia del SDK de Media Services de Storage. Ahora Storage y no Media Services mantiene la información que pertenece a Storage. 

Para más información, consulte [Migración a Media Services v3](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>¿Qué hay del cifrado del almacenamiento del lado del cliente?

Actualmente, es recomendable utilizar el cifrado de almacenamiento en el lado de servidor (que está activo de forma predeterminada). Para más información, consulte [Cifrado del servicio Azure Storage para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a Media Services v3](media-services-overview.md)
