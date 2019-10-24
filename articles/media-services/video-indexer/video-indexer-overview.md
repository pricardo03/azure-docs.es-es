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
ms.date: 09/23/2019
ms.author: juliako
ms.openlocfilehash: 2afc3a859ddb5378b6313c43d693842fdb5fce14
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2019
ms.locfileid: "72296433"
---
# <a name="what-is-video-indexer"></a>¿Qué es Video Indexer?

Video Indexer (VI) es la solución de IA de Azure Media Services y parte de la marca Microsoft Cognitive Services. Video Indexer proporciona la capacidad de extraer información profunda (sin necesidad de análisis de datos o conocimientos de codificación) mediante el uso de modelos de Machine Learning basados en varios canales (voz, voces y objeto visual). Los modelos se pueden personalizar y entrenar aún más. El servicio habilita la búsqueda profunda, reduce los costos operativos, permite nuevas oportunidades de monetización, nuevas experiencias de usuario en grandes archivos de vídeos (con bajas barreras de entrada). 

Para empezar a extraer información con Video Indexer, tiene que crear una cuenta y cargar vídeos. Cuando se cargan vídeos en Video Indexer, este analiza los objetos visuales y el audio mediante la ejecución de diferentes modelos de IA. A medida que Video Indexer analiza el vídeo, los modelos extraen la información.

El siguiente diagrama es una ilustración, no una explicación técnica, de cómo funciona Video Indexer en el back-end.

![Diagrama de flujo](./media/video-indexer-overview/model-chart.png)

## <a name="what-can-i-do-with-video-indexer"></a>¿Qué puedo hacer con Video Indexer?

La información de Video Indexer se puede aplicar a muchos escenarios, entre ellos:

* *Búsqueda profunda*: use la información extraída del vídeo para mejorar la experiencia de búsqueda en una biblioteca de vídeos. Por ejemplo, la indexación de palabras habladas y de caras, permite la experiencia de búsqueda de momentos en un vídeo en los que una persona ha pronunciado ciertas palabras o cuando se han visto juntas a dos personas. La búsqueda basada en esta información de los vídeos se puede aplicar a agencias de noticias, instituciones educativas, emisoras, propietarios de contenido de entretenimiento, aplicaciones de línea de negocio de empresas y, en general, a cualquier sector que tenga una biblioteca de vídeos en la que los usuarios necesiten buscar.
* *Creación de contenido*: cree clips finales, resalte momentos destacados, contenido de medios sociales o clips de noticias basados en la información que Video Indexer extrae de su contenido. Los fotogramas clave, los marcadores de escenas y las marcas de tiempo para las personas y los aspectos de la etiqueta hacen que el proceso de creación sea mucho más fluido y sencillo, y le permite ir directamente a las partes interesantes del vídeo que necesita para el contenido que está creando.
* *Accesibilidad*: si desea que el contenido esté disponible para personas con discapacidades, o que se distribuya en distintas regiones con distintos idiomas, puede usar la transcripción y traducción que proporciona Video Indexer en varios idiomas y lenguajes.
* *Monetization*: Video Indexer puede ayudar a aumentar el valor de los vídeos. Por ejemplo, los sectores que dependen de los ingresos por publicidad (como el caso de los medios de comunicación, las redes sociales, etc.), pueden entregar anuncios más pertinentes con la información extraída como señales adicionales para el servidor de anuncios.
* *Moderación de contenido*: use modelos de moderación de contenido textual y visual para proteger a los usuarios de contenido inadecuado, y asegurarse de que el contenido que publique coincide con los valores de su organización. Puede bloquear automáticamente determinados vídeos o avisar a los usuarios sobre el contenido. 
* *Recomendaciones*: La información de vídeo se puede usar para mejorar la interacción con los usuarios, resaltando los momentos de vídeo que les puedan resultar más pertinentes. Al etiquetar cada vídeo con metadatos adicionales, puede recomendar a los usuarios los vídeos más importantes y resaltar la parte del vídeo que se adaptaría mejor a sus necesidades. 

## <a name="features"></a>Características

A continuación se muestra la lista de información que puede recuperar de los vídeos mediante los modelos de audio y vídeo de Video Indexer:

### <a name="video-insights"></a>Información de los vídeos

* **Detección de caras**: detecta y agrupa las caras que aparecen en el vídeo.
* **Identificación de celebridades**: Video Indexer identifica automáticamente más de un millón de famosos (como líderes mundiales, actores y actrices, atletas, investigadores y líderes empresariales y tecnológicos de todo el mundo). Los datos de estos famosos también pueden encontrarse en varios sitios web famosos, como IMDB y Wikipedia.
* **Identificación facial basada en cuentas**:  Video Indexer entrena un modelo para una cuenta específica. A continuación reconoce las caras en el vídeo según el modelo entrenado. Para más información, consulte [Customize a Person model from the Video Indexer website](customize-person-model-with-website.md) (Personalización de un modelo de persona desde el sitio web de Video Indexer) y [Customize a Person model with the Video Indexer API](customize-person-model-with-api.md) (Personalización de un modelo de persona con Video Indexer API).
* **Extracción de miniaturas de caras** ("mejor cara"): identifica automáticamente la mejor cara capturada en cada grupo de caras (según la calidad, el tamaño y la posición frontal) y la extrae como un recurso de imagen.
* **Reconocimiento de texto visual** (OCR): extrae el texto que se muestra visualmente en el vídeo.
* **Moderación de contenido visual**: Detecta los objetos visuales para adultos o subidos de tono.
* **Identificación de etiquetas**: identifica los objetos visuales y las acciones que se muestran.
* **Segmentación de escenas**: determina cuándo una escena cambia en el vídeo según indicaciones visuales. Una escena representa un evento único y se compone de una serie de cortes consecutivos que están semánticamente relacionadas.
* **Detección de cortes**: determina cuándo una escena cambia en el vídeo según indicaciones visuales. Un corte es una serie de fotogramas tomados a partir de la misma cámara de imágenes en movimiento. Para más información, consulte Escenas, cortes y fotogramas clave.
* **Detección de fotogramas negros**: identifica los fotogramas negros presentados en el vídeo.
* **Extracción de fotogramas clave**: detecta los fotogramas clave estables en un vídeo.
* **Créditos de rodaje**: identifica el principio y el final de los créditos del final de las series de televisión y las películas.
* **Detección de personajes animados** (versión preliminar): detección, agrupación y reconocimiento de personajes en contenido animado a través de la integración con la [visión personalizada de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/). Para más información, consulte [Detección de personajes animados](animated-characters-recognition.md).
* **Detección del tipo de toma editorial**: etiquetado de capturas basado en su tipo (como plano general, plano medio, primer plano, primerísimo primer plano, dos capturas, varias personas, exterior e interior, etc.). Para más información, consulte [Detección del tipo de toma editorial](scenes-shots-keyframes.md#editorial-shot-type-detection).

### <a name="audio-insights"></a>Información de audio

* **Detección de idioma automática**: identifica automáticamente el idioma hablado dominante. Los idiomas admitidos son alemán, chino (simplificado), español, francés, inglés, italiano, japonés, portugués (brasileño) y ruso. Si el idioma hablado no se puede identificar con confianza, Video Indexer supone que es inglés. Para obtener más información, vea [Modelo de identificación de idiomas](language-identification-model.md).
* **Identificación y transcripción de voz en varios idiomas** (versión preliminar): identifica automáticamente el idioma que se habla en los diferentes segmentos del audio, el envío de cada segmento del archivo multimedia que se va a transcribir y la combinación de la transcripción para tener una transcripción unificada. Para más información, consulte [Identificación y transcripción automáticas del contenido de varios idiomas](multi-language-identification-transcription.md).
* **Transcripción de audio**: convierte la voz en texto en 12 idiomas y permite extensiones. Los idiomas admitidos son alemán, árabe, chino (simplificado), coreano, español, francés, hindi, inglés, italiano, japonés, portugués brasileño y ruso.
* **Subtítulos (CC)** : crea subtítulos en tres formatos: VTT, TTML y SRT.
* **Procesamiento de dos canales**: detecta, separa la transcripción y se combina en una única escala de tiempo de forma automática.
* **Reducción de ruido**: borra el audio telefónico o las grabaciones ruidosas (basado en los filtros de Skype).
* **Personalización de la transcripción** (CRIS): entrena modelos personalizados de conversión de voz en texto para crear transcripciones específicas del sector. Para más información, consulte [Customize a Language model from the Video Indexer website](customize-language-model-with-website.md) (Personalización de un modelo de lenguaje desde el sitio web de Video Indexer) y [Customize a Language model with the Video Indexer API](customize-language-model-with-api.md) (Personalización de un modelo de lenguaje con Video Indexer API).
* **Enumeración de altavoz**: asigna y comprende las palabras que transmitió cada altavoz y cuándo.
* **Estadísticas de altavoz**: Proporciona las estadísticas de las relaciones de voz de los altavoces.
* **Moderación de contenido textual**: detecta texto explícito en la transcripción de audio.
* **Efectos de audio**: identifica los efectos de audio, como palmadas, voces y silencios.
* **Detección de emociones**: identifica emociones en función de la voz (lo que se dice) y el tono (cómo se dice). La emoción podría ser felicidad, tristeza, ira o miedo.
* **Traducción**: crea traducciones de la transcripción de audio en 54 idiomas diferentes.

### <a name="audio-and-video-insights-multi-channels"></a>Información de audio y vídeo (varios canales)

Al indexar por un canal, puede haber resultados parciales para esos modelos disponibles

* **Extracción de palabras clave**: extrae palabras clave a partir de voz y texto visual.
* **Extracción de entidades con nombre**: extrae marcas, ubicaciones y personas del lenguaje hablado y del texto visual mediante el procesamiento de lenguaje natural (NLP).
* **Inferencia de tema**: saca conclusiones de los temas principales a partir de las transcripciones. Se incluye la taxonomía IPTC de segundo nivel.
* **Artefactos**: extrae una amplia variedad de artefactos de "detalles de siguiente nivel" para cada uno de los modelos.
* **Análisis de opiniones**: identifica opiniones positivas, negativas y neutras a partir de voz y texto visual.

## <a name="how-can-i-get-started-with-video-indexer"></a>¿Cómo puedo empezar a usar Video Indexer?

Puede tener acceso a las funciones de Video Indexer de tres maneras diferentes: 

* Portal de Video Indexer: una solución fácil de usar que le permite evaluar el producto, administrar la cuenta y personalizar los modelos. 

    Para más información sobre el portal, consulte la [Introducción al sitio web de Video Indexer](video-indexer-get-started.md).  
* Integración de API: todas las funcionalidades de Video Indexer están disponibles a través de una API REST para que pueda integrar la solución en sus aplicaciones e infraestructura. 

    Para empezar a trabajar como desarrollador, consulte  [Uso de la API REST de Video Indexer](video-indexer-use-apis.md). 
* Widget enmendable: permite insertar experiencias de información, reproductor y editor de Video Indexer en la aplicación. 

    Para más información, consulte  [Inserción de widgets de Video Indexer en las aplicaciones](video-indexer-embed-widgets.md). 

Si usa el sitio web, la información se agrega como metadatos y es visible en el portal. Si usa las API, la información está disponible como archivo JSON. 

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para empezar a trabajar con Video Indexer. Para más información, consulte los siguientes artículos.

- [Introducción al sitio web de Video Indexer](video-indexer-get-started.md)
- [Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
- [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md)
