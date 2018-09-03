---
title: Computer Vision para Azure Cognitive Services | Microsoft Docs
description: Los algoritmos avanzados de Computer Vision le ayudan a procesar imágenes y devolver información en Azure Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: overview
ms.date: 08/22/2018
ms.author: v-deken
ms.openlocfilehash: ec3ffa8599192ecd9a4092b026a8d249f50c64c9
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248612"
---
# <a name="what-is-computer-vision"></a>¿Qué es Computer Vision?

El servicio Computer Vision está basado en la nube y proporciona a los desarrolladores acceso a algoritmos avanzados para procesar imágenes y devolver información. Computer Vision funciona con formatos de imagen muy utilizados, como JPEG y PNG. Para analizar una imagen, puede cargarla o especificar la dirección URL de la misma. Los algoritmos de Computer Vision pueden analizar el contenido de una imagen de diferentes formas, en función de las características visuales que le interesen. Por ejemplo, Computer Vision puede determinar si una imagen incluye contenido explícito o para adultos, o buscar todas las caras en una imagen.

Computer Vision se puede usar en cualquier aplicación. Para ello se pueden usar nuestras [bibliotecas cliente](quickstarts-sdk/csharp-analyze-sdk.md) para invocar al servicio, o bien invocar directamente a la [API REST](vision-api-how-to-topics/howtocallvisionapi.md) para:

- [Analizar imágenes para obtener información](#analyzing-images-for-insight)
- [Extraer texto de imágenes](#extracting-text-from-images)
- [Moderar el contenido de las imágenes](#moderating-content-in-images)

## <a name="analyzing-images-for-insight"></a>Analizar imágenes para obtener información

Puede analizar imágenes con Computer Vision para detectar y proporcionan información detallada acerca de las funciones visuales y las características de las imágenes. Puede cargar el contenido de una imagen, cuando se analizan imágenes locales, o bien especificar la dirección URL de una imagen para analizar imágenes remotas.

Computer Vision puede realizar las siguientes acciones al analizar una imagen:

| . | DESCRIPCIÓN |
| ------ | ----------- |
|**[Etiquetar características visuales](quickstarts/csharp-analyze.md)**|Identifique y etiquete las características visuales de una imagen basándose en más de 2000 objetos, seres vivos, paisajes y acciones reconocibles. Cuando las etiquetas son ambiguas o no muy comunes, la respuesta proporciona "indicaciones" para aclarar el significado de la etiqueta en el contexto de un entorno conocido. El etiquetado no se limita al sujeto principal, como una persona en primer plano, sino que también incluye el entorno (interior o exterior), muebles, herramientas, plantas, animales, accesorios, gadgets, etc.|
|**[Clasificar una imagen](quickstarts/csharp-analyze.md)**|Identifique y clasifique toda una imagen mediante una [taxonomía de categoría](Category-Taxonomy.md) con jerarquías hereditarias de elementos primarios y secundarios. Las categorías se pueden usar solas o con nuestros nuevos modelos de etiquetado.<br/>Actualmente, el inglés es el único idioma que se admite para etiquetar y clasificar imágenes.|
|**[Describir una imagen](quickstarts/csharp-analyze.md)**|Genere una descripción de toda una imagen en lenguaje natural, con frases completas. Los algoritmos de Computer Vision generan varias descripciones en función de los objetos identificados en la imagen. Cada una de estas descripciones se evalúa y se genera una puntuación de confianza. Después, se devuelve una lista de puntuaciones de confianza ordenadas de más alta a más baja.<br/>Un ejemplo de bot que usa esta tecnología para generar leyendas de imagen se puede ver [en GitHub](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).|
|**[Detectar caras](quickstarts/csharp-analyze.md)** |Detecte caras en una imagen y proporcione información acerca de ellas. Computer Vision devuelve las coordenadas, el rectángulo, el género y la edad de los rostros que detecta.<br/>Computer Vision proporciona un subconjunto de la funcionalidad que se puede encontrar en [Face](/azure/cognitive-services/face/) y este servicio se puede usar para obtener un análisis más detallado, como la identificación facial y la detección de posturas.|
|**[Detectar tipos de imagen](quickstarts/csharp-analyze.md)**|Detecte las características de una imagen, como por ejemplo, si una imagen es un dibujo lineal o la probabilidad de que sea una imagen prediseñada.|
|**[Detectar contenido específico del dominio](quickstarts/python-domain.md)**|Use los modelos de dominio para detectar e identificar el contenido específico del dominio en una imagen, como celebridades y monumentos. Por ejemplo, si una imagen contiene personas, Computer Vision puede usar un modelo de dominio para celebridades que se incluye con el servicio para determinar si las personas que se han detectado en la imagen coinciden con famosos conocidos.|
|**[Detectar la combinación de colores](quickstarts/csharp-analyze.md)**|Analice el uso del color en una imagen. Computer Vision puede determinar si una imagen está en blanco y negro o en color, y en las imágenes de color, identificar los colores dominantes y de énfasis.|
|**[Generar una miniatura](quickstarts/csharp-thumb.md)**|Analice el contenido de una imagen para generar una miniatura adecuada de la misma. Computer Vision en primer lugar genera una miniatura de alta calidad y, después, analiza los objetos de la imagen para determinar la *región de interés* (ROI). Luego, recorta la imagen para ajustarla a los requisitos de la región de interés. La miniatura generada se puede presentar con una relación de aspecto diferente de la de la imagen original en función de sus necesidades.|

## <a name="extracting-text-from-images"></a>Extracción de texto de las imágenes

Puede usar Computer Vision para [extraer mediante OCR el texto ](quickstarts/csharp-print-text.md) de una imagen en una secuencia de caracteres de lectura mecánica. Si es necesario, OCR corrige el giro del texto reconocido, en grados, alrededor del eje horizontal de la imagen y proporciona las coordenadas del marco de cada palabra. El OCR admite 25 idiomas y detecta automáticamente el idioma del texto extraído.

También puede [reconocer tanto el texto impreso como el manuscrito](quickstarts/csharp-hand-text.md) de una imagen. Computer Vision puede detectar y extraer texto impreso y manuscrito de imágenes de diversos objetos con diferentes superficies y fondos, como recibos, pósteres, tarjetas de visita, letras y pizarras. En la actualidad, el reconocimiento de texto impreso y manuscrito está en versión preliminar, y el inglés es el único lenguaje que se admite.  

## <a name="moderating-content-in-images"></a>Moderación del contenido de las imágenes

Computer Vision se puede usar para [detectar contenido explícito y para adultos](quickstarts/csharp-analyze.md) en una imagen, para lo que valora la probabilidad de que la imagen contenga este tipo de contenido y genera una puntuación relativa a la confianza de todo él. El filtro para la detección de contenido explícito y para adultos se puede establecer en una escala deslizando, con el fin de que pueda ajustarlo a sus preferencias.

## <a name="image-requirements"></a>Requisitos de imagen

Computer Vision puede analizar las imágenes que cumplan los requisitos siguientes:

- La imagen se debe presentar en formato JPEG, PNG, GIF o BMP
- El tamaño de archivo de la imagen debe ser inferior a 4 megabytes (MB)
- Las dimensiones de la imagen deben ser mayores que 50 x 50 píxeles  
  Para el OCR, las dimensiones de la imagen deben estar entre 40 x 40 y 3200 x 3200 píxeles, y la imagen no puede superar los 10 megapixeles.

## <a name="next-steps"></a>Pasos siguientes

Comience a usar Computer Vision con una de nuestras guías de inicio rápido:

- [Analizar una imagen](/quickstarts-sdk/csharp-analyze-sdk.md)
- [Extracción de texto manuscrito](/quickstarts-sdk/csharp-hand-text-sdk.md)
- [Generación de miniaturas](/quickstarts-sdk/csharp-thumb-sdk.md)
