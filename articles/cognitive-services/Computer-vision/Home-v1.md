---
title: ¿Qué es Computer Vision API?
titlesuffix: Azure Cognitive Services
description: Computer Vision API proporciona a los desarrolladores acceso a algoritmos avanzados para procesar imágenes y devolver información.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 08/10/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 35002ccfc294b59a13e6826b4ca75a1ab6c68b62
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55870306"
---
# <a name="what-is-computer-vision-api-version-10"></a>¿Qué es Computer Vision API versión 1.0?

> [!IMPORTANT]
> Hay una nueva versión de Computer Vision API disponible. Consulte:
>- [Información general](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Computer Vision API versión 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

Computer Vision API, basada en la nube, provee a los desarrolladores de acceso a algoritmos avanzados para procesar imágenes y devolver información. Al cargar una imagen o al especificar una URL de imagen, los algoritmos del servicio Computer Vision pueden analizar el contenido visual de diferentes maneras basándose en las entradas y las opciones del usuario. Con Computer Vision API, los usuarios pueden analizar imágenes para:
* Etiquetar imágenes según el contenido.
* Categorizar imágenes.
* Identificar el tipo y la calidad de las imágenes.
* [Detectar caras humanas y devolver sus coordenadas. ](#Faces)
* Reconocer contenido específico del dominio.
* Generar descripciones de contenido.
* Utilizar reconocimiento óptico de caracteres para identificar el texto encontrado en las imágenes.
* Reconocer texto manuscrito.
* Distinguir combinaciones de colores.
* Marcar contenido solo para adultos.
* Recortar fotografías para usarlas como miniaturas.

## <a name="requirements"></a>Requisitos
* Métodos de entrada admitidos: binarios de imagen RAW en forma de application/octet-stream o URL de imagen.
* Formatos de imagen admitidos: JPEG, PNG, GIF y BMP.
* Tamaño del archivo de imagen: menos de 4 MB.
* Dimensión de la imagen: mayor que 50 x 50 píxeles.

## <a name="tagging-images"></a>Etiquetado de imágenes
Computer Vision API devuelve etiquetas basándose en miles de objetos, seres vivos, paisajes y acciones reconocibles. Cuando las etiquetas son ambiguas o no muy comunes, la respuesta de la API contiene «indicaciones» para aclarar el significado de la etiqueta en el contexto de un entorno conocido. Las etiquetas no están organizadas por su taxonomía y no existen jerarquías de herencia. Una colección de etiquetas de contenido es la base de la "descripción" de una imagen que se muestra en lenguaje natural con formato de oraciones completas. Tenga en cuenta que, en este momento, el inglés es el único idioma admitido para la descripción de la imagen.

Después de cargar una imagen o especificar la URL de una imagen, los algoritmos de Computer Vision API devuelven como salida etiquetas basadas en los objetos, seres vivos, paisajes y acciones identificados en la imagen. El etiquetado no se limita al sujeto principal, como una persona en primer plano, sino que también incluye el entorno (interior o exterior), muebles, herramientas, plantas, animales, accesorios, gadgets, etc.

### <a name="example"></a>Ejemplo
![House_Yard](./Images/house_yard.png) '

```json
Returned Json
{
   'tags':[
      {
         "name":"grass",
         "confidence":0.999999761581421
      },
      {
         "name":"outdoor",
         "confidence":0.999970674514771
      },
      {
         "name":"sky",
         "confidence":999289751052856
      },
      {
         "name":"building",
         "confidence":0.996463239192963
      },
      {
         "name":"house",
         "confidence":0.992798030376434
      },
      {
         "name":"lawn",
         "confidence":0.822680294513702
      },
      {
         "name":"green",
         "confidence":0.641222536563873
      },
      {
         "name":"residential",
         "confidence":0.314032256603241
      },
   ],
}
```
## <a name="categorizing-images"></a>Categorización de imágenes
Además de etiquetas y descripciones, Computer Vision API devuelve categorías basadas en la taxonomía de versiones anteriores. Estas categorías se organizan en una taxonomía con jerarquías hereditarias de elementos principales y secundarios. Todas las categorías están en inglés. Se pueden usar solas o con los modelos nuevos.

### <a name="the-86-category-concept"></a>El concepto de las 86 categorías
Las características visuales que se encuentran en una imagen se pueden categorizar de lo más amplio a lo más específico mediante la lista de 86 conceptos que vemos en el siguiente diagrama. Consulte la taxonomía completa en formato de texto en [Taxonomía de las categorías](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Análisis de categorías](./Images/analyze_categories.png)

Imagen                                                  | Response
------------------------------------------------------ | ----------------
![Woman Roof](./Images/woman_roof.png)                 | people
![Family Photo](./Images/family_photo.png)             | people_crowd
![Cute Dog](./Images/cute_dog.png)                     | animal_dog
![Outdoor Mountain](./Images/mountain_vista.png)       | outdoor_mountain
![Vision Analyze Food Bread](./Images/bread.png)       | food_bread

## <a name="identifying-image-types"></a>Identificación de los tipos de imagen
Hay varias maneras de categorizar las imágenes. Computer Vision API puede establecer una marca booleana para indicar si una imagen está en blanco y negro o en color. También puede establecer una marca booleana para indicar si una imagen es un dibujo lineal o no. También puede indicar si es una imagen prediseñada o no, y mostrar su calidad como tal en una escala del 0 al 3.

### <a name="clip-art-type"></a>Tipo de imagen prediseñada
Detecta si una imagen es una imagen prediseñada o no.  

Valor | Significado
----- | --------------
0     | No es imagen prediseñada
1     | Ambigua
2     | Imagen prediseñada normal
3     | Imagen prediseñada buena

Imagen|Response
----|----
![Vision Analyze Cheese Clip Art](./Images/cheese_clipart.png)|3 Imagen prediseñada buena
![Vision Analyze House Yard](./Images/house_yard.png)|0 No es imagen prediseñada

### <a name="line-drawing-type"></a>Tipo de dibujo lineal
Detecta si una imagen es un dibujo lineal o no.

Imagen|Response
----|----
![Vision Analyze Lion Drawing](./Images/lion_drawing.png)|True
![Vision Analyze Flower](./Images/flower.png)|False

### <a name="faces"></a>Caras
Detecta las caras humanas dentro de una imagen y genera las coordenadas faciales, el rectángulo de la cara, el sexo y la edad. Estas características visuales son un subconjunto de los metadatos generados para la cara. Para metadatos más amplios de las caras (identificación facial, detección de posición y más), se usa Face API.  

Imagen|Response
----|----
![Vision Analyze Woman Roof Face](./Images/woman_roof_face.png) | [ { "age": 23, "gender": "Female", "faceRectangle": { "left": 1379, "top": 320, "width": 310, "height": 310 } } ]
![Vision Analyze Mom Daughter Face](./Images/mom_daughter_face.png) | [ { "age": 28, "gender": "Female", "faceRectangle": { "left": 447, "top": 195, "width": 162, "height": 162 } }, { "age": 10, "gender": "Male", "faceRectangle": { "left": 355, "top": 87, "width": 143, "height": 143 } } ]
![Vision Analyze Family Phot Face](./Images/family_photo_face.png) | [ { "age": 11, "gender": "Male", "faceRectangle": { "left": 113, "top": 314, "width": 222, "height": 222 } }, { "age": 11, "gender": "Female", "faceRectangle": { "left": 1200, "top": 632, "width": 215, "height": 215 } }, { "age": 41, "gender": "Male", "faceRectangle": { "left": 514, "top": 223, "width": 205, "height": 205 } }, { "age": 37, "gender": "Female", "faceRectangle": { "left": 1008, "top": 277, "width": 201, "height": 201 } } ]


## <a name="domain-specific-content"></a>Contenido específico del dominio

Además de las etiquetas y las categorías de nivel superior, Computer Vision API también admite información especializada (o específica del dominio). La información especializada se puede implementar como un método independiente o con la categorización de alto nivel. Funciona como un método para mejorar la taxonomía de las 86 categorías a través de la adición de modelos específicos del dominio.

Actualmente, la única información especializada admitida es el reconocimiento de celebridades y el reconocimiento de puntos de referencia. Se trata de precisiones específicas del dominio para las categorías de personas y grupos de personas, y para puntos de referencia de todo el mundo.

Hay dos opciones para usar los modelos específicos del dominio:

### <a name="option-one---scoped-analysis"></a>Opción número uno: análisis con ámbito
Se analiza solo un modelo elegido, mediante la invocación de una llamada HTTP POST. Para esta opción, si se conoce qué tipo de modelo se quiere usar, se especifica el nombre del modelo y solo se obtiene información pertinente para ese modelo. Por ejemplo, puede usar esta opción para el reconocimiento de celebridades. La respuesta contiene una lista de potenciales coincidencias de celebridades, acompañadas de su puntuación de confianza.

### <a name="option-two---enhanced-analysis"></a>Opción número dos: análisis mejorado
Análisis que proporciona detalles adicionales relacionados con las categorías de la taxonomía de las 86 categorías. Esta opción está disponible para aplicaciones donde los usuarios quieren obtener un análisis genérico de imágenes adicional a los detalles de uno o más modelos específicos del dominio. Cuando se llama a este método, se llama primero al clasificador de la taxonomía de las 86 categorías. Si cualquiera de las categorías concuerda con los modelos conocidos/coincidentes, se procede a una segunda pasada de invocaciones de clasificador. Por ejemplo, si «details=all» o «details» incluyen «celebrities», el método llama al clasificador de celebridades una vez llamado el clasificador de las 86 categorías. El resultado incluye las etiquetas que comiencen por «people_».

## <a name="generating-descriptions"></a>Generación de descripciones 
Los algoritmos de Computer Vision API analizan el contenido de una imagen. Este análisis forma la base de una «descripción» que se presenta en lenguaje natural, en oraciones completas. La descripción resume lo que se encuentra en la imagen. Los algoritmos de Computer Vision API generan varias descripciones según los objetos identificados en la imagen. Cada una de estas descripciones se evalúa y se genera una puntuación de confianza. Después, se devuelve una lista de puntuaciones de confianza ordenadas de más alta a más baja. Un ejemplo de un bot que usa esta tecnología para generar leyendas de imagen se puede ver [aquí](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Ejemplo de generación de descripción
![B&W Buildings](./Images/bw_buildings.png) '
```json
 Returned Json

'description':{
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a large city',
         'confidence':0.607638706850331
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a photo of a large city',
         'confidence':0.577256764264197
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a city',
         'confidence':0.538493271791207
      }
   ]   
   'description':[
      "tags":{
         "outdoor",
         "city",
         "building",
         "photo",
         "large",
      }
   ]
}
```

## <a name="perceiving-color-schemes"></a>Percepción de combinaciones de colores
El algoritmo de Computer Vision extrae los colores de una imagen. Los colores se analizan en tres contextos diferentes: primer plano, fondo y todo. Se agrupan en doce colores de énfasis dominantes, que son: negro, azul, marrón, gris, verde, naranja, rosa, púrpura, rojo, verde azulado, blanco y amarillo. En función de los colores de una imagen, se pueden devolver el blanco y negro básico o los colores de énfasis en códigos de color hexadecimales.

Imagen                                                       | Primer plano |Fondo| Colores
----------------------------------------------------------- | --------- | ------- | ------
![Outdoor Mountain](./Images/mountain_vista.png)            | Negro     | Negro   | Blanco
![Vision Analyze Flower](./Images/flower.png)               | Negro     | Blanco   | Blanco, negro, verde
![Vision Analyze Train Station](./Images/train_station.png) | Negro     | Negro   | Negro

### <a name="accent-color"></a>Color de énfasis
Color que se extrae de una imagen y que está designado a representar el color más atractivo para los usuarios a través de una combinación de colores dominantes y saturación.

Imagen                                                       | Response
----------------------------------------------------------- | ----
![Outdoor Mountain](./Images/mountain_vista.png)            | #BC6F0F
![Vision Analyze Flower](./Images/flower.png)               | #CAA501
![Vision Analyze Train Station](./Images/train_station.png) | #484B83


### <a name="black--white"></a>Blanco y negro
Marca booleana que indica si una imagen está en blanco y negro o no.

Imagen                                                      | Response
---------------------------------------------------------- | ----
![Vision Analyze Building](./Images/bw_buildings.png)      | True
![Vision Analyze House Yard](./Images/house_yard.png)      | False

## <a name="flagging-adult-content"></a>Marcado de contenido solo para adultos
Entre las diferentes categorías visuales, se encuentra el grupo de contenido para adultos y subido de tono, que permite detectar el material destinado a adultos y limita la presentación de imágenes de contenido sexual. El filtro para detección de contenido para adultos y subido de tono se puede establecer en una escala gradual, para ajustarlo a la preferencia del usuario.

## <a name="optical-character-recognition-ocr"></a>Reconocimiento óptico de caracteres (OCR)
La tecnología OCR detecta el contenido de texto de una imagen y extrae el texto identificado en una secuencia de caracteres de lectura mecánica. El resultado se puede usar para búsquedas y otros propósitos, como registros médicos, seguridad y banca. Detecta automáticamente el idioma. OCR ahorra tiempo y es cómodo para los usuarios, ya que permite tomar fotografías del texto en lugar de transcribirlo.

OCR admite 25 idiomas, Estos idiomas son: árabe, chino simplificado, chino tradicional, checo, danés, holandés, inglés, finés, francés, alemán, griego, húngaro, italiano, japonés, coreano, noruego, polaco, portugués, rumano, ruso, serbio (cirílico y latino), eslovaco, español, sueco y turco.

Si es necesario, OCR corrige el giro del texto reconocido, en grados, alrededor del eje horizontal de la imagen. OCR da las coordenadas del marco de cada palabra, como se observa en la ilustración inferior.

![OCR Overview](./Images/vision-overview-ocr.png) Requisitos de OCR:
- El tamaño de la imagen de entrada debe estar entre 40 x 40 y 3200 x 3200 píxeles.
- La imagen no puede ser mayor que 10 megapixeles.

A la imagen de entrada se le puede aplicar un giro de cualquier múltiplo de 90 grados, además de un pequeño ángulo de hasta 40 grados.

La precisión del reconocimiento de texto depende de la calidad de la imagen. El origen de una lectura imprecisa puede ser:
- Imágenes borrosas.
- Texto escrito a mano o en cursiva.
- Estilos de fuente artísticos.
- Tamaño de texto pequeño.
- Fondos complejos, sombras, brillo sobre el texto o distorsión de la perspectiva.
- Letra mayúscula grande o falta de letras mayúsculas iniciales.
- Texto de superíndice, subíndice o tachado.

 Limitaciones: En las fotos en las que predomina el texto, las palabras que se reconocen de forma parcial pueden producir falsos positivos. En algunas fotos, especialmente las que no tienen texto, la precisión puede variar considerablemente según el tipo de imagen.

## <a name="recognize-handwritten-text"></a>Reconocer texto manuscrito
Esta tecnología permite detectar y extraer texto manuscrito de notas, cartas, ensayos, pizarras, formularios, etc. Funciona con diferentes tipos de superficies y fondos, tales como papel blanco, notas adhesivas amarillas y pizarras interactivas.

El reconocimiento de texto manuscrito ahorra tiempo y esfuerzo, y puede ayudarle a ser más productivo porque permite tomar imágenes del texto en lugar de tener que transcribirlo. Permite digitalizar notas. Esta digitalización permite implementar una búsqueda rápida y sencilla, así como reducir el uso de papel.

Requisitos de entrada:
- Formatos de imagen admitidos: JPEG, PNG y BMP.
- El tamaño del archivo de imagen debe ser inferior a 4 MB.
- Las dimensiones de la imagen deben ser al menos de 40 x 40, a lo sumo 3200 x 3200.

Nota: Esta tecnología está actualmente en versión preliminar y solo está disponible para texto en inglés.

## <a name="generating-thumbnails"></a>Generación de miniaturas
Una miniatura es una pequeña representación de una imagen a tamaño completo. La variedad de dispositivos, como teléfonos, tabletas y PC, hacen que sean necesarios diferentes diseños y tamaños de miniaturas para cada experiencia de usuario. Mediante el recorte inteligente, esta característica de Computer Vision API ayuda a resolver el problema.

Después de cargar una imagen, se genera una miniatura de alta calidad y el algoritmo de Computer Vision API analiza los objetos de la imagen. A continuación, se recorta la imagen para ajustarla a los requisitos del “área de interés”. La salida se muestra dentro de un marco especial, como se observa en la siguiente ilustración. La miniatura generada se puede presentar con una relación de aspecto que es diferente de la relación de aspecto de la imagen original para adaptarse a las necesidades del usuario.

El algoritmo de miniaturas funciona del siguiente modo:

1. Quita los elementos prescindibles de la imagen y reconoce el objeto principal, el “área de interés”.
2. Recorta la imagen en función del área de interés identificada.
3. Cambia la relación de aspecto para ajustarse a las dimensiones de la miniatura de destino.

![Miniaturas](./Images/thumbnail-demo.png)
