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
ms.date: 12/19/2019
ms.author: juliako
ms.openlocfilehash: a8f4174fca1a8703bb112c19e785d4d9686a82f5
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453305"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Notas de la versión de Video Indexer de Azure Media Services

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, copie y pegue esta URL (`https://docs.microsoft.com/api/search/rss?search=%22Azure+Media+Services+Video+Indexer+release+notes%22&locale=en-us`) en el lector de fuentes RSS.

Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

* Versiones más recientes
* Problemas conocidos
* Corrección de errores
* Funciones obsoletas

## <a name="november-2019"></a>Noviembre de 2019
 
* Compatibilidad con modelos de lenguaje personalizado en coreano

    Video Indexer ahora admite modelos de lenguaje personalizados en coreano (`ko-KR`) tanto en la API como en el portal. 
* Nuevos idiomas compatibles con Conversión de voz en texto (STT)

    Las API de Video Indexer ahora admiten STT en árabe levantino (ar-SY), dialecto inglés del Reino Unido (en-GB) y dialecto australiano inglés (en-AU).
    
    En el caso de la carga de vídeo, reemplazamos zh-HANS por zh-CN, se admiten ambos, pero zh-CN es el recomendado y el más preciso.
    
## <a name="october-2019"></a>Octubre de 2019
 
* Búsqueda de personajes animados en la galería

    Ahora, al indexar personajes animados, puede buscarlos en la galería de vídeos de la cuenta. Para más información, consulte [Reconocimiento de personajes animados](animated-characters-recognition.md).

## <a name="september-2019"></a>Septiembre de 2019
 
En la feria IBC 2019, se anunciaron varios avances:
 
* Reconocimiento de caracteres animados (versión preliminar pública)

    Posibilidad de detectar, agrupar y reconocer caracteres en el contenido animado, gracias a la integración con Custom Vision. Para más información, consulte [Detección de personajes animados](animated-characters-recognition.md).
* Identificación de varios idiomas (versión preliminar pública)

    Detecte segmentos en varios idiomas en la pista de audio y cree una transcripción multilingüe a partir de ellos. Compatibilidad inicial: inglés, español, alemán y francés. Para más información, consulte [Identificación y transcripción automáticas del contenido de varios idiomas](multi-language-identification-transcription.md).
* Extracción de entidades con nombre para personas y ubicaciones

    extrae marcas, ubicaciones y personas del lenguaje hablado y del texto visual mediante el procesamiento de lenguaje natural (NLP).
* Clasificación del tipo de toma editorial

    Etiquetado de tomas con tipos editoriales como cierre, toma media, dos tomas, interior, exterior, etc. Para más información, consulte [Detección del tipo de toma editorial](scenes-shots-keyframes.md#editorial-shot-type-detection).
* Mejora de la inferencia de temas: ahora abarca el nivel 2
    
    El modelo de inferencia de temas ahora admite una granularidad más profunda de la taxonomía IPTC. Lea todos los detalles en [Innovación con inteligencia artificial de Azure Media Services](https://azure.microsoft.com/blog/azure-media-services-new-ai-powered-innovation/).

## <a name="august-2019"></a>Agosto de 2019
 
### <a name="video-indexer-deployed-in-uk-south"></a>Video Indexer implementado en Sur de Reino Unido

Ya puede crear una cuenta de pago de Video Indexer en la región Sur de Reino Unido.

### <a name="new-editorial-shot-type-insights-available"></a>Nueva información disponible sobre los tipos de capturas editoriales

Las nuevas etiquetas agregadas a las capturas de vídeo proporcionan "tipos de captura" editoriales para identificarlas con las frases editoriales que se suelen emplear en el flujo de trabajo de creación de contenido, como primer plano extremo, primer plano, plano general, plano medio, dos tomas, exteriores, interiores, cara izquierda y cara derecha (disponible en el JSON).

### <a name="new-people-and-locations-entities-extraction-available"></a>Extracción de entidades de nuevas personas y ubicaciones disponible

Video Indexer identifica las ubicaciones y las personas con nombre a través del procesamiento de lenguaje natural (NLP) desde el OCR y la transcripción del vídeo. Video Indexer usa el algoritmo de aprendizaje automático para reconocer cuándo se está haciendo mención a ubicaciones específicas (por ejemplo, la Torre Eiffel) o a personas (por ejemplo, John Doe) en un vídeo.

### <a name="keyframes-extraction-in-native-resolution"></a>Extracción de fotogramas clave en resolución nativa

Los fotogramas clave extraídos por Video Indexer están disponibles en la resolución original del vídeo.
 
### <a name="ga-for-training-custom-face-models-from-images"></a>Disponibilidad general para entrenar modelos de caras personalizadas a partir de imágenes

El entrenamiento de caras a partir de imágenes ha pasado del modo de versión preliminar a disponibilidad general (disponible mediante la API y en el portal).

> [!NOTE]
> La transición de versión preliminar a disponibilidad general no tiene ninguna repercusión en el precio.

### <a name="hide-gallery-toggle-option"></a>Ocultar opción de alternancia de la galería

El usuario puede optar por ocultar la pestaña de la galería en el portal (es similar a ocultar la pestaña de ejemplos).
 
### <a name="maximum-url-size-increased"></a>Aumento del tamaño máximo de la dirección URL

Compatibilidad con la cadena de consulta de dirección URL 4096 (en lugar de 2048) en la indexación de un vídeo.
 
### <a name="support-for-multi-lingual-projects"></a>Compatibilidad con proyectos multilingües

Ahora se pueden crear proyectos basados en vídeos indexados en distintos idiomas (solo API).

## <a name="july-2019"></a>Julio de 2019

### <a name="editor-as-a-widget"></a>Editor como un widget

El editor Video Indexer AI ahora está disponible como widget para insertarse en las aplicaciones de los clientes.

### <a name="update-custom-language-model-from-closed-caption-file-from-the-portal"></a>Actualización del modelo de lenguaje personalizado desde el archivo de subtítulos cerrados desde el portal

Los clientes pueden proporcionar los formatos de archivo VTT, SRT y TTML como entrada para los modelos de idioma en la página de personalización del portal.

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
