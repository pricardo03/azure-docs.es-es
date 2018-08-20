---
title: Introducción a Azure Video Indexer | Microsoft Docs
description: En este tema se proporciona información general del servicio Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 07/25/2018
ms.author: nolachar
ms.openlocfilehash: f52c4af29d0c7de8b5edbe869640ffc5dddb5c5e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397898"
---
# <a name="what-is-video-indexer-preview"></a>¿Qué es Video Indexer? (versión preliminar)

Video Indexer es una aplicación en nube creada con Azure Media Analytics, Cognitive Services (como Face API, Microsoft Translator, Computer Vision API y Custom Speech Service) y Azure Search. Le permite extraer la siguiente información de sus vídeos mediante tecnologías de inteligencia artificial:

- **Detección automática del idioma**: Video Indexer puede detectar automáticamente el idioma del vídeo. La detección automática del idioma actualmente admite inglés, español, francés, alemán, italiano, chino (simplificado), japonés y ruso. Volverá al inglés cuando no se pueda detectar ningún idioma.
- **Transcripción de audio**: Video Indexer tiene la funcionalidad de voz a texto, que permite a los clientes obtener una transcripción de las palabras habladas. Los idiomas admitidos son inglés, español, francés, alemán, italiano, chino (simplificado), portugués (Brasil), japonés y ruso (y muchos más que se incluirán en el futuro). 
- **Seguimiento e identificación facial**: las tecnologías de reconocimiento facial permiten la detección de las caras en un vídeo. Las caras detectadas se comparan con una base de datos de celebridades para evaluar cuáles están presentes en el vídeo. Los clientes también pueden etiquetar caras que no coinciden con una celebridad. Video Indexer crea un modelo de cara basado en esas etiquetas y puede reconocer esas caras en los vídeos enviados en el futuro.
- **Indexación de hablantes**: Video Indexer tiene la funcionalidad de asignar y comprender qué orador dice qué palabras y cuándo.
- **Reconocimiento de texto visual**: con esta tecnología, el servicio Video Indexer extrae el texto que se muestra en los vídeos.  
- **Detección de actividad de voz**: la detección habilita Video Indexer para separar el ruido de fondo y la actividad de la voz. 
- **Detección de escena**: Video Indexer tiene la capacidad de realizar un análisis visual sobre el vídeo para determinar cuándo se cambia una escena en un vídeo.
- **Extracción de fotogramas clave**: Video Indexer detecta automáticamente los fotogramas clave de un vídeo. 
- **Análisis de sentimiento**: Video Indexer realiza un análisis de sentimiento sobre el texto extraído utilizando reconocimiento de voz a texto y el reconocimiento óptico de caracteres, y proporciona esa información en forma de opiniones positivas, negativas o neutras, junto con códigos de tiempo.
- **Traducción**: Video Indexer puede traducir la transcripción del audio de un idioma a otro. Se admiten los idiomas siguientes: inglés, español, francés, alemán, italiano, chino simplificado, portugués de Brasil, japonés y ruso. Una vez traducido, el usuario puede incluso obtener subtítulos en el reproductor de vídeo en otros idiomas.
- **Moderación del contenido visual**: esta tecnología permite la detección de contenido para adultos o atrevido que esté presente en el vídeo y puede utilizarse para el filtrado de contenido. 
- **Extracción de palabras clave**: Video Indexer extrae palabras clave basadas en la transcripción de las palabras habladas y el texto reconocido por el módulo de reconocimiento de texto visual.
- **Etiquetas**: Video Indexer proporciona etiquetas para objetos visuales como gato, perro, mesa, coche, así como acciones como estar de pie, correr o volar.
- **Marcas**: Video Indexer extrae marcas comerciales basadas en la transcripción de las palabras habladas y el texto reconocido por el módulo de reconocimiento de texto visual.

Cuando Video Indexer termina de procesar y analizar, se puede revisar, ajustar, buscar y publicar la información de vídeo.

Tanto si su rol es el de administrador de contenido como el de desarrollador, el servicio Video Indexer puede satisfacer sus necesidades. Los administradores de contenido pueden utilizar el portal web de Video Indexer para consumir el servicio sin tener que escribir una sola línea de código; consulte el artículo de [introducción al portal de Video Indexer](video-indexer-get-started.md). Los desarrolladores pueden aprovechar las API para procesar el contenido a escala; consulte [Uso de Video Indexer REST API](video-indexer-use-apis.md). El servicio también permite a los clientes utilizar widgets para publicar secuencias de vídeo y extraer información en sus propias aplicaciones; consulte el tema sobre la [inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md).

Puede suscribirse al servicio mediante una cuenta existente de AAD, LinkedIn, Facebook, Google o MSA. Para más información, consulte la [introducción](video-indexer-get-started.md).

## <a name="scenarios"></a>Escenarios

A continuación se presentan algunos escenarios en los que Video Indexer puede ser útil.

- Búsqueda: las información extraída del vídeo se puede utilizar para mejorar la experiencia de búsqueda en una biblioteca de vídeos. Por ejemplo, la indexación de palabras habladas y caras puede permitir la experiencia de búsqueda de momentos en un vídeo en los que una persona en particular ha pronunciado ciertas palabras o cuando se han visto juntas a dos personas. La búsqueda basada en esta información de los vídeos se puede aplicar a agencias de noticias, instituciones educativas, emisoras, propietarios de contenido de entretenimiento, aplicaciones de línea de negocio de empresas y, en general, a cualquier sector que tenga una biblioteca de vídeos en la que los usuarios necesiten buscar.

- Monetización: Video Indexer puede ayudar a mejorar el valor de los vídeos. Por ejemplo, los sectores que dependen de los ingresos por publicidad (por ejemplo, los medios de comunicación, las redes sociales, etc.), pueden entregar anuncios más relevantes con los conocimientos extraídos como señales adicionales para el servidor de anuncios (la presentación de un anuncio de calzado deportivo es más relevante en medio de un partido de fútbol que en una competición de natación).

- Involucración del usuario: la información del vídeo se puede utilizar para mejorar la involucración de los usuarios al posicionar momentos de vídeo más relevantes para ellos. Como ejemplo, considere un vídeo educativo que explique las esferas durante los primeros 30 minutos y las pirámides durante los siguientes 30 minutos. El estudiante que lee sobre las pirámides se beneficiaría más si el vídeo se posiciona a partir del marcador de 30 minutos.

Para más información, consulte [este blog](http://aka.ms/videoindexerblog).

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para empezar a trabajar con Video Indexer. Para más información, consulte los siguientes artículos.

- [Introducción al portal de Video Indexer](video-indexer-get-started.md)
- [Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
- [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md)
