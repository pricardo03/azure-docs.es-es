---
title: ¿Qué es Video Indexer?
titlesuffix: Azure Cognitive Services
description: En este tema se proporciona información general del servicio Video Indexer.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 10/10/2018
ms.author: nolachar
ms.openlocfilehash: 9936cd301cc8dad88ce1a2431f9253a15ab2da7f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251260"
---
# <a name="what-is-video-indexer"></a>¿Qué es Video Indexer?

Azure Video Indexer es una aplicación en la nube creada en Azure Media Analytics, Azure Search y Cognitive Services (como Face API, Microsoft Translator, Computer Vision API y Custom Speech Service). Permite extraer la información de los vídeos con los modelos de Video Indexer que se describen a continuación:
 
- **Detección de idioma automática**: identifica automáticamente el idioma hablado dominante. Los idiomas admitidos son alemán, chino (simplificado), español, francés, inglés, italiano, japonés, portugués (brasileño) y ruso. Volverá al inglés cuando no se pueda detectar ningún idioma.
- **Transcripción de audio**: convierte voz en texto en 10 idiomas y permite extensiones. Los idiomas admitidos son alemán, árabe, chino (simplificado), español, francés, inglés, italiano, japonés, portugués (brasileño) y ruso.
- **Subtítulos (CC)**: crea subtítulos en los formatos VTT, TTML y SRT.
- **Procesamiento de dos canales**: detecta, separa la transcripción y se combina en una única escala de tiempo de forma automática.
- **Reducción de ruido**: borra el audio telefónico o las grabaciones ruidosas (basado en los filtros de Skype).
- **Personalización de la transcripción (CRIS)**: entrena y ejecuta modelos personalizados y extendidos de conversión de voz en texto para crear transcripciones específicas del sector.
- **Enumeración de altavoz**: asigna y comprende las palabras que transmitió cada altavoz y cuándo.
- **Estadísticas de altavoz**: proporciona las estadísticas de las relaciones de voz de los altavoces.
- **Reconocimiento de texto visual (OCR)**: extrae el texto que se muestra visualmente en el vídeo.
- **Extracción de fotogramas clave**: detecta los fotogramas clave estables en un vídeo.
- **Análisis de opiniones**: identifica opiniones positivas, negativas y neutras a partir de voz y texto visual.
- **Moderación de contenido visual**: detecta los elementos visuales explícitos o para adultos.
- **Extracción de palabras clave**: extrae palabras clave a partir de voz y texto visual.
- **Identificación de etiquetas**: identifica los objetos visuales y las acciones que se muestran.
- **Extracción de marcas**: extrae marcas a partir de voz y texto visual.
- **Detección de caras**: detecta y agrupa las caras que aparecen en el vídeo.
- **Extracción de miniatura de caras ("mejor cara")**: identifica automáticamente la mejor cara capturada en cada grupo de caras (según la calidad, el tamaño y la posición frontal) y la extrae como un recurso de imagen.
- **Identificación de celebridades**: Video Indexer identifica automáticamente más de un millón de (como líderes mundiales, actores y actrices, atletas, investigadores y líderes empresariales y tecnológicos de todo el mundo). Los datos de estos famosos también pueden encontrarse en varios sitios web famosos, como IMDB y Wikipedia.
- **Identificación facial basada en cuentas**: Video Indexer entrena un modelo para una cuenta específica. Luego, reconoce caras en el vídeo según el modelo entrenado específicamente para vídeos de esa cuenta.
- **Moderación de contenido textual**: detecta texto explícito en la transcripción de audio.
- **Detección de cortes**: determina cuándo cambia una escena en el vídeo.
- **Detección de fotogramas negros**: identifica los fotogramas negros presentados en el vídeo.
- **Efectos de audio**: identifica los efectos de audio, como palmadas, voces y silencios.
- **Inferencia de tema**: saca conclusiones de los temas principales a partir de las transcripciones. Se incluye la taxonomía [IPTC](https://iptc.org/standards/media-topics/) de primer nivel.
- **Detección de emociones**: identifica emociones en función de las entradas de audio y voz. La emoción podría ser: felicidad, tristeza, ira o miedo.
- **Artefactos**: extrae una amplia variedad de artefactos de "detalles de siguiente nivel" para cada uno de los modelos.
- **Traducción**: crea traducciones de la transcripción de audio en 54 idiomas diferentes.

Cuando Video Indexer termina de procesar y analizar, se puede revisar, ajustar, buscar y publicar la información de vídeo.

Tanto si su rol es el de administrador de contenido como el de desarrollador, el servicio Video Indexer puede satisfacer sus necesidades. Los administradores de contenido pueden utilizar el portal web de Video Indexer para consumir el servicio sin tener que escribir una sola línea de código; consulte el artículo sobre la [introducción al sitio web de Video Indexer](video-indexer-get-started.md). Los desarrolladores pueden aprovechar las API para procesar el contenido a escala; consulte [Uso de Video Indexer REST API](video-indexer-use-apis.md). El servicio también permite a los clientes utilizar widgets para publicar secuencias de vídeo y extraer información en sus propias aplicaciones; consulte el tema sobre la [inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md).

Puede suscribirse al servicio mediante una cuenta existente de AAD, LinkedIn, Facebook, Google o MSA. Para más información, consulte la [introducción](video-indexer-get-started.md).

## <a name="scenarios"></a>Escenarios

A continuación se presentan algunos escenarios en los que Video Indexer puede ser útil.

- Búsqueda: las información extraída del vídeo se puede utilizar para mejorar la experiencia de búsqueda en una biblioteca de vídeos. Por ejemplo, la indexación de palabras habladas y caras puede permitir la experiencia de búsqueda de momentos en un vídeo en los que una persona en particular ha pronunciado ciertas palabras o cuando se han visto juntas a dos personas. La búsqueda basada en esta información de los vídeos se puede aplicar a agencias de noticias, instituciones educativas, emisoras, propietarios de contenido de entretenimiento, aplicaciones de línea de negocio de empresas y, en general, a cualquier sector que tenga una biblioteca de vídeos en la que los usuarios necesiten buscar.

- Monetización: Video Indexer puede ayudar a mejorar el valor de los vídeos. Por ejemplo, los sectores que dependen de los ingresos por publicidad (por ejemplo, los medios de comunicación, las redes sociales, etc.), pueden entregar anuncios más relevantes con los conocimientos extraídos como señales adicionales para el servidor de anuncios (la presentación de un anuncio de calzado deportivo es más relevante en medio de un partido de fútbol que en una competición de natación).

- Involucración del usuario: la información del vídeo se puede utilizar para mejorar la involucración de los usuarios al posicionar momentos de vídeo más relevantes para ellos. Como ejemplo, considere un vídeo educativo que explique las esferas durante los primeros 30 minutos y las pirámides durante los siguientes 30 minutos. El estudiante que lee sobre las pirámides se beneficiaría más si el vídeo se posiciona a partir del marcador de 30 minutos.

Para más información, consulte [este blog](https://aka.ms/videoindexerblog).

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para empezar a trabajar con Video Indexer. Para más información, consulte los siguientes artículos.

- [Introducción al sitio web de Video Indexer](video-indexer-get-started.md)
- [Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
- [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md)
