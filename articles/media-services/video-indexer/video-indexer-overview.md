---
title: ¿Qué es Video Indexer de Azure Media Services?
titleSuffix: Azure Media Services
description: En este tema se proporciona información general del servicio Video Indexer de Azure Media Services.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/06/2019
ms.author: juliako
ms.openlocfilehash: e3f60b5fb0693e40c9db040f7b14f487fce8f68e
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860245"
---
# <a name="what-is-video-indexer"></a>¿Qué es Video Indexer?

Video Indexer de Azure Media Services es una aplicación en la nube creada en Azure Media Analytics, Azure Search y Cognitive Services (como Face API, Microsoft Translator, Computer Vision API y Custom Speech Service). Permite extraer la información de los vídeos con los modelos de vídeo y audio de Video Indexer que se describen a continuación:
  
## <a name="video-insights"></a>Información de los vídeos

- **Detección de caras**: detecta y agrupa las caras que aparecen en el vídeo.
- **Identificación de celebridades**: Video Indexer identifica automáticamente más de un millón de famosos (como líderes mundiales, actores y actrices, atletas, investigadores y líderes empresariales y tecnológicos de todo el mundo). Los datos de estos famosos también pueden encontrarse en varios sitios web famosos, como IMDB y Wikipedia.
- **Identificación facial basada en cuentas**:  Video Indexer entrena un modelo para una cuenta específica. A continuación reconoce las caras en el vídeo según el modelo entrenado. Para más información, consulte [Customize a Person model from the Video Indexer website](customize-person-model-with-website.md) (Personalización de un modelo de persona desde el sitio web de Video Indexer) y [Customize a Person model with the Video Indexer API](customize-person-model-with-api.md) (Personalización de un modelo de persona con Video Indexer API).
- **Extracción de miniaturas de caras** ("mejor cara"): identifica automáticamente la mejor cara capturada en cada grupo de caras (según la calidad, el tamaño y la posición frontal) y la extrae como un recurso de imagen.
- **Reconocimiento de texto visual** (OCR): extrae el texto que se muestra visualmente en el vídeo.
- **Moderación de contenido visual**: Detecta los objetos visuales para adultos o subidos de tono.
- **Identificación de etiquetas**: identifica los objetos visuales y las acciones que se muestran.
- **Segmentación de escenas**: determina cuándo una escena cambia en el vídeo según indicaciones visuales. Una escena representa un evento único y se compone de una serie de cortes consecutivos que están semánticamente relacionadas. 
- **Detección de cortes**: determina cuándo una escena cambia en el vídeo según indicaciones visuales. Un corte es una serie de fotogramas tomados a partir de la misma cámara de imágenes en movimiento. Para más información, consulte [Scenes, shots, and keyframes](scenes-shots-keyframes.md) (Escenas, cortes y fotogramas clave).
- **Detección de fotogramas negros**: identifica los fotogramas negros presentados en el vídeo.
- **Extracción de fotogramas clave**: detecta los fotogramas clave estables en un vídeo.
- **Créditos de rodaje**: identifica el principio y el final de los créditos del final de las series de televisión y las películas.
- **Detección de personajes animados** (versión preliminar): detección, agrupación y reconocimiento de personajes en contenido animado a través de la integración con la [visión personalizada de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/). Para más información, consulte [Detección de personajes animados](animated-characters-recognition.md).
- **Detección del tipo de toma editorial**: etiquetado de capturas basado en su tipo (como plano general, plano medio, primer plano, primerísimo primer plano, dos capturas, varias personas, exterior e interior, etc.). Para más información, consulte [Detección del tipo de toma editorial](scenes-shots-keyframes.md#editorial-shot-type-detection).

## <a name="audio-insights"></a>Información de audio

- **Detección de idioma automática**: identifica automáticamente el idioma hablado dominante. Los idiomas admitidos son alemán, chino (simplificado), español, francés, inglés, italiano, japonés, portugués (brasileño) y ruso. Si el idioma hablado no se puede identificar con confianza, Video Indexer supone que es inglés. Para obtener más información, vea [Modelo de identificación de idiomas](language-identification-model.md).
- **Identificación y transcripción de voz en varios idiomas** (versión preliminar): identifica automáticamente el idioma que se habla en los diferentes segmentos del audio, el envío de cada segmento del archivo multimedia que se va a transcribir y la combinación de la transcripción para tener una transcripción unificada. Para más información, consulte [Identificación y transcripción automáticas del contenido de varios idiomas](multi-language-identification-transcription.md).
- **Transcripción de audio**: convierte la voz en texto en 12 idiomas y permite extensiones. Los idiomas admitidos son alemán, árabe, chino (simplificado), coreano, español, francés, hindi, inglés, italiano, japonés, portugués brasileño y ruso.
- **Subtítulos (CC)** : crea subtítulos en tres formatos: VTT, TTML y SRT.
- **Procesamiento de dos canales**: detecta, separa la transcripción y se combina en una única escala de tiempo de forma automática.
- **Reducción de ruido**: borra el audio telefónico o las grabaciones ruidosas (basado en los filtros de Skype).
- **Personalización de la transcripción** (CRIS): entrena modelos personalizados de conversión de voz en texto para crear transcripciones específicas del sector. Para más información, consulte [Customize a Language model from the Video Indexer website](customize-language-model-with-website.md) (Personalización de un modelo de lenguaje desde el sitio web de Video Indexer) y [Customize a Language model with the Video Indexer API](customize-language-model-with-api.md) (Personalización de un modelo de lenguaje con Video Indexer API).
- **Enumeración de altavoz**: asigna y comprende las palabras que transmitió cada altavoz y cuándo.
- **Estadísticas de altavoz**: proporciona las estadísticas de las relaciones de voz de los altavoces.
- **Moderación de contenido textual**: detecta texto explícito en la transcripción de audio.
- **Efectos de audio**: identifica los efectos de audio, como palmadas, voces y silencios.
- **Detección de emociones**: identifica emociones en función de la voz (lo que se dice) y el tono (cómo se dice).  La emoción podría ser: felicidad, tristeza, ira o miedo.
- **Traducción**: crea traducciones de la transcripción de audio en 54 idiomas diferentes.

## <a name="audio-and-video-insights-multi-channels"></a>Información de audio y vídeo (varios canales)

Al indexar por un canal, puede haber resultados parciales para esos modelos disponibles

- **Extracción de palabras clave**: extrae palabras clave a partir de voz y texto visual.
- **Extracción de entidades con nombre**: extrae marcas, ubicaciones y personas del lenguaje hablado y del texto visual mediante el procesamiento de lenguaje natural (NLP).
- **Inferencia de tema**: saca conclusiones de los temas principales a partir de las transcripciones. Se incluye la taxonomía IPTC de primer nivel.
- **Artefactos**: extrae una amplia variedad de artefactos de "detalles de siguiente nivel" para cada uno de los modelos.
- **Análisis de opiniones**: identifica opiniones positivas, negativas y neutras a partir de voz y texto visual.
 
Cuando Video Indexer termina de procesar y analizar, se puede revisar, ajustar, buscar y publicar la información de vídeo.

Tanto si su rol es el de administrador de contenido como el de desarrollador, el servicio Video Indexer puede satisfacer sus necesidades. Los administradores de contenido pueden utilizar el portal web de Video Indexer para consumir el servicio sin tener que escribir una sola línea de código; consulte el artículo sobre la [introducción al sitio web de Video Indexer](video-indexer-get-started.md). Los desarrolladores pueden aprovechar las API para procesar el contenido a escala; consulte [Uso de Video Indexer REST API](video-indexer-use-apis.md). El servicio también permite a los clientes utilizar widgets para publicar secuencias de vídeo y extraer información en sus propias aplicaciones; consulte el tema sobre la [inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md).

Puede suscribirse al servicio mediante una cuenta existente de AAD, LinkedIn, Facebook, Google o MSA. Para más información, consulte la [introducción](video-indexer-get-started.md).

## <a name="scenarios"></a>Escenarios

A continuación se presentan algunos escenarios en los que Video Indexer puede ser útil.

- Búsqueda: las información extraída del vídeo se puede utilizar para mejorar la experiencia de búsqueda en una biblioteca de vídeos. Por ejemplo, la indexación de palabras habladas y caras puede permitir la experiencia de búsqueda de momentos en un vídeo en los que una persona en particular ha pronunciado ciertas palabras o cuando se han visto juntas a dos personas. La búsqueda basada en esta información de los vídeos se puede aplicar a agencias de noticias, instituciones educativas, emisoras, propietarios de contenido de entretenimiento, aplicaciones de línea de negocio de empresas y, en general, a cualquier sector que tenga una biblioteca de vídeos en la que los usuarios necesiten buscar.
- Creación de contenido: información extraída de vídeos que ayuda a crear contenido eficazmente, como trailers, contenido de redes sociales, noticias, clips, etc., a partir de contenido existente en el archivo de la organización. 
- Monetización: Video Indexer puede ayudar a mejorar el valor de los vídeos. Por ejemplo, los sectores que dependen de los ingresos por publicidad (por ejemplo, los medios de comunicación, las redes sociales, etc.), pueden entregar anuncios más relevantes con los conocimientos extraídos como señales adicionales para el servidor de anuncios (la presentación de un anuncio de calzado deportivo es más relevante en medio de un partido de fútbol que en una competición de natación).
- Involucración del usuario: la información del vídeo se puede utilizar para mejorar la involucración de los usuarios al posicionar momentos de vídeo más relevantes para ellos. Como ejemplo, considere un vídeo educativo que explique las esferas durante los primeros 30 minutos y las pirámides durante los siguientes 30 minutos. El estudiante que lee sobre las pirámides se beneficiaría más si el vídeo se posiciona a partir del marcador de 30 minutos.

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para empezar a trabajar con Video Indexer. Para más información, consulte los siguientes artículos.

- [Introducción al sitio web de Video Indexer](video-indexer-get-started.md)
- [Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
- [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md)
