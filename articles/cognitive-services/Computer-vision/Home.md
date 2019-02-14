---
title: ¿Qué es Computer Vision API? - Computer Vision
titlesuffix: Azure Cognitive Services
description: El servicio Computer Vision proporciona a los desarrolladores acceso a algoritmos avanzados para procesar imágenes y devolver información.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 08/22/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 4322b118268ee88235190f5672585d17ea365495
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55872653"
---
# <a name="what-is-computer-vision"></a>¿Qué es Computer Vision?

El servicio Computer Vision de Azure proporciona a los desarrolladores acceso a algoritmos avanzados para procesar imágenes y devolver información. Para analizar una imagen, puede cargarla o especificar la dirección URL de la misma. Los algoritmos de procesamiento de imágenes pueden analizar el contenido de una imagen de diferentes formas, en función de las características visuales que le interesen. Por ejemplo, Computer Vision puede determinar si una imagen incluye contenido explícito o para adultos, o buscar todos los rostros en una imagen.

Puede usar Computer Vision en la aplicación mediante el uso de un SDK nativo o invocar la API REST directamente. Esta página cubre ampliamente lo que puede hacer con Computer Vision.

## <a name="analyze-images-for-insight"></a>Análisis de imágenes para obtener información

Puede analizar imágenes para detectar y proporcionar información detallada acerca de las funciones visuales y las características. Todas las características de la tabla siguiente se proporcionan gracias a la API [de análisis de imágenes](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).

| . | DESCRIPCIÓN |
| ------ | ----------- |
|**[Etiquetar características visuales](concept-tagging-images.md)**|Identifique y etiquete las características visuales de una imagen na partir de un conjunto de miles de objetos, seres vivos, paisajes y acciones reconocibles. Cuando las etiquetas son ambiguas o no muy comunes, la respuesta de la API contiene “indicaciones” para aclarar el significado de la etiqueta en el contexto de un entorno conocido. El etiquetado no se limita al sujeto principal, como una persona en primer plano, sino que también incluye el entorno (interior o exterior), muebles, herramientas, plantas, animales, accesorios, gadgets, etc.|
|**[Detectar objetos](concept-object-detection.md)**| La detección de objetos es similar al etiquetado, pero la API devuelve las coordenadas del rectángulo delimitador para cada etiqueta aplicada. Por ejemplo, si una imagen contiene un perro, un gato y una persona, la operación de detección mostrará esos objetos junto con sus coordenadas en la imagen. Puede usar esta funcionalidad para procesar más las relaciones entre los objetos de una imagen. También permite saber cuándo hay varias instancias de la misma etiqueta en una imagen.|
|**[Detección de las marcas](concept-brand-detection.md)**|Identifique las marcas comerciales de imágenes o vídeos desde una base de datos de miles de logotipos globales. Puede usar esta característica, por ejemplo, para detectar qué marcas son más populares en medios sociales o más frecuentes en la ubicación de los productos multimedia.|
|**[Clasificar una imagen](concept-categorizing-images.md)**|Identifique y clasifique toda una imagen mediante una [taxonomía de categoría](Category-Taxonomy.md) con jerarquías hereditarias de elementos primarios y secundarios. Las categorías se pueden usar solas o con nuestros nuevos modelos de etiquetado.<br/>Actualmente, el inglés es el único idioma que se admite para etiquetar y clasificar imágenes.|
|**[Describir una imagen](concept-describing-images.md)**|Genere una descripción de toda una imagen en lenguaje natural, con frases completas. Los algoritmos de Computer Vision generan varias descripciones en función de los objetos identificados en la imagen. Cada una de estas descripciones se evalúa y se genera una puntuación de confianza. Después, se devuelve una lista de puntuaciones de confianza ordenadas de más alta a más baja.|
|**[Detectar caras](concept-detecting-faces.md)** |Detecte caras en una imagen y proporcione información acerca de ellas. Computer Vision devuelve las coordenadas, el rectángulo, el género y la edad de los rostros que detecta.<br/>Computer Vision proporciona un subconjunto de la funcionalidad que se puede encontrar en [Face](/azure/cognitive-services/face/) y este servicio se puede usar para obtener un análisis más detallado, como la identificación facial y la detección de posturas.|
|**[Detectar tipos de imagen](concept-detecting-image-types.md)**|Detecte las características de una imagen, como por ejemplo, si una imagen es un dibujo lineal o la probabilidad de que sea una imagen prediseñada.|
|**[Detectar contenido específico del dominio](concept-detecting-domain-content.md)**|Use los modelos de dominio para detectar e identificar el contenido específico del dominio en una imagen, como celebridades y monumentos. Por ejemplo, si una imagen contiene personas, Computer Vision puede usar un modelo de dominio para celebridades que se incluye con el servicio para determinar si las personas que se han detectado en la imagen coinciden con famosos conocidos.|
|**[Detectar la combinación de colores](concept-detecting-color-schemes.md)**|Analice el uso del color en una imagen. Computer Vision puede determinar si una imagen está en blanco y negro o en color, y en las imágenes de color, identificar los colores dominantes y de énfasis.|
|**[Generar una miniatura](concept-generating-thumbnails.md)**|Analice el contenido de una imagen para generar una miniatura adecuada de la misma. En primer lugar, Computer Vision genera una miniatura de alta calidad y, después, analiza los objetos de la imagen para determinar el *área de interés*. Luego, Computer Vision recorta la imagen para ajustarla a los requisitos del área de interés. La miniatura generada se puede presentar con una relación de aspecto diferente de la de la imagen original en función de sus necesidades.|
|**[Obtener el área de interés](concept-generating-thumbnails.md#area-of-interest)**|Analice el contenido de una imagen para devolver las coordenadas del *área de interés*. Se trata de la misma función que se usa para generar una miniatura, pero en lugar de recortar la imagen, Computer Vision devuelve las coordenadas del rectángulo delimitador de la región, por lo que la aplicación que realiza la llamada puede modificar la imagen original según sea necesario.|


## <a name="extract-text-from-images"></a>Extracción de texto en las imágenes

Puede usar Computer Vision para [extraer mediante OCR el texto ](concept-extracting-text-ocr.md) de una imagen en una secuencia de caracteres de lectura mecánica. Si es necesario, OCR corrige el giro del texto reconocido, en grados, alrededor del eje horizontal de la imagen y proporciona las coordenadas del marco de cada palabra. El OCR admite 25 idiomas y detecta automáticamente el idioma del texto extraído.

También puede [reconocer tanto el texto impreso como el manuscrito](concept-recognizing-text.md) de una imagen. Computer Vision puede detectar y extraer texto impreso y manuscrito de imágenes de diversos objetos con diferentes superficies y fondos, como recibos, pósteres, tarjetas de visita, letras y pizarras. En la actualidad, el reconocimiento de texto impreso y manuscrito está en versión preliminar, y el inglés es el único lenguaje que se admite.  

## <a name="moderate-content-in-images"></a>Moderación del contenido de las imágenes

Computer Vision se puede usar para [detectar contenido explícito y para adultos](concept-detecting-adult-content.md) en una imagen, para lo que valora la probabilidad de que la imagen contenga este tipo de contenido y genera una puntuación relativa a la confianza de todo él. El filtro para la detección de contenido explícito y para adultos se puede establecer en una escala deslizando, con el fin de que pueda ajustarlo a sus preferencias.

## <a name="use-containers"></a>Uso de contenedores

[Use contenedores de Computer Vision](computer-vision-how-to-install-containers.md) para reconocer texto impreso y escrito a mano localmente, mediante la instalación de un contenedor de Docker estándar más cercano a los datos.

## <a name="image-requirements"></a>Requisitos de imagen

Computer Vision puede analizar las imágenes que cumplan los requisitos siguientes:

- La imagen se debe presentar en formato JPEG, PNG, GIF o BMP
- El tamaño de archivo de la imagen debe ser inferior a 4 megabytes (MB)
- Las dimensiones de la imagen deben ser mayores que 50 x 50 píxeles
  - Para OCR, el tamaño de la imagen de entrada debe estar entre 50 x 50 y 4200 x 4200 píxeles.

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de los datos

Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Computer Vision deben estar al tanto de las directivas de Microsoft sobre los datos de clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Introducción a Computer Vision, con una guía de inicio rápido:

- [Inicio rápido: Análisis de una imagen](quickstarts-sdk/csharp-analyze-sdk.md)
- [Inicio rápido: Extracción de texto manuscrito](quickstarts-sdk/csharp-hand-text-sdk.md)
- [Inicio rápido: Generación de miniaturas](quickstarts-sdk/csharp-thumb-sdk.md)
