---
title: Notas de la versión de Video Indexer de Azure Media Services | Microsoft Docs
description: Para mantenerse al día con los últimos desarrollos, en este artículo se proporcionan las actualizaciones más reciente en Video Indexer de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 06/25/2019
ms.author: juliako
ms.openlocfilehash: f1c5f43316292f17547b84d970a0f03a1a2c366f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454635"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Notas de la versión de Video Indexer de Azure Media Services

Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

* Versiones más recientes
* Problemas conocidos
* Corrección de errores
* Funciones obsoletas

## <a name="june-2019"></a>Junio de 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer se ha implementado en el Este de Japón

Ya puede crear una cuenta de pago de Video Indexer en la región del Este de Japón.

### <a name="create-and-repair-account-api-preview"></a>Crear y reparar la API de la cuenta (versión preliminar)

Se agregó una nueva API que le permite [actualizar el punto de conexión o la clave de la instancia Azure Media Services](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag).

### <a name="improve-error-handling-on-upload"></a>Mejorar el control de errores en la carga 

Se devuelve un mensaje descriptivo en caso de que haya una configuración incorrecta de la cuenta subyacente de Azure Media Services.

### <a name="player-timeline-keyframes-preview"></a>Versión preliminar de los fotogramas clave de la escala de tiempo del reproductor 

Ya puede ver una versión preliminar de la imagen de cada hora en la escala de tiempo del reproductor.

### <a name="editor-semi-select"></a>Selección parcial del editor

Ya puede ver una versión preliminar de todas las conclusiones que se seleccionaron como resultado de elegir un período de tiempo específico en el editor.

## <a name="may-2019"></a>Mayo de 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Actualizar el modelo de lenguaje personalizado desde el archivo de subtítulos

Las API para [crear modelos de lenguaje personalizados](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) y [actualizar los modelos de lenguaje personalizados](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) admiten los formatos de archivo VTT, SRT y TTML como entrada para los modelos de lenguaje.

Al llamar a la [API para actualizar la transcripción de vídeo](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript), la transcripción se agrega automáticamente. El modelo de aprendizaje asociado con el vídeo también se actualiza automáticamente. Para obtener información sobre cómo personalizar y entrenar sus modelos de lenguaje, consulte [Customize a Language model with Video Indexer](customize-language-model-overview.md) (Personalizar un modelo de lenguaje con el Video Indexer).

### <a name="new-download-transcript-formats--txt-and-csv"></a>Nuevos formatos de transcripción de descarga: TXT y CSV

Además del formato de subtítulos ya admitidos (SRT, VTT y TTML), Video Indexer ahora admite la descarga de la transcripción en los formatos TXT y CSV.

## <a name="next-steps"></a>Pasos siguientes

[Información general](video-indexer-overview.md)
