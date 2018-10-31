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
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: dccbc6e57e970ec7089f81fccb33b741b9c00e74
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376727"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Preguntas más frecuentes sobre Azure Media Services v3

En este artículo se ofrecen respuestas a las preguntas más frecuentes de Azure Media Services (AMS) v3.

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>¿Puedo usar Azure Portal para administrar los recursos de v3?

Todavía no. Puede usar uno de los SDK compatibles. Consulte los tutoriales y ejemplos de este conjunto de documentos.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>¿Hay una API para configurar unidades reservadas de multimedia?

El equipo de Media Services está en proceso de eliminación de las RU en v3. Sin embargo, el trabajo de servicio necesario no está finalizado. Hasta entonces, los clientes deben usar Azure Portal o las API de AMS v2 para establecer las RU (como se describe en [Escalado del procesamiento multimedia](../previous/media-services-scale-media-processing-overview.md). 

Al usar **VideoAnalyzerPreset** o **AudioAnalyzerPreset**, establezca la cuenta de Media Services en 10 unidades reservadas de multimedia de S3.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>¿Un recurso de V3 tiene el concepto de AssetFile?

Los AssetFiles se eliminaron de la API de AMS para separar la dependencia del SDK de Media Services de Storage. Ahora Storage y no Media Services mantiene la información que pertenece a Storage. 

## <a name="where-did-client-side-storage-encryption-go"></a>¿Qué hay del cifrado del almacenamiento del lado del cliente?

Ahora se recomienda el cifrado del almacenamiento en el servidor (que está activado de forma predeterminada). Para más información, consulte [Azure Storage Service Encryption para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>¿Cuál es el método de carga recomendado?

Se recomienda el uso de la ingesta HTTP(s). Para más información, consulte [Ingesta HTTP(s)](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>¿Cómo funciona la paginación?

Media Services admite $top para los recursos que admiten OData, pero el valor pasado a $top debe ser inferior a 1000 (por ejemplo, el tamaño de página para la paginación).

Esto permite obtener una pequeña muestra de los elementos usando $top (por ejemplo, los 100 elementos más recientes) o página entre todos los elementos mediante el uso de la paginación. 

Media Services no admite la paginación en los datos con un tamaño de página especificado por el usuario.

Para más información, consulte [Filtrado, ordenación y paginación](assets-concept.md#filtering-ordering-paging)

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>¿Cómo se recupera una entidad de Media Services v3?

v3 se basa en una superficie de API unificada que expone la funcionalidad de administración y de operaciones basada en **Azure Resource Manager**. De acuerdo con **Azure Resource Manager**, los nombres de los recursos siempre son únicos. Por lo tanto, puede usar cualquier cadena de identificador único (por ejemplo, GUID) para los nombres de recursos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a Media Services v3](media-services-overview.md)
