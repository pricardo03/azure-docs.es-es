---
title: Detección de caras y conceptos de atributos
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los conceptos sobre detección de caras y los atributos de cara.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: 3293067190386738efbfeb433bd38453c9e5699f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64572555"
---
# <a name="face-detection-and-attributes"></a>Detección de caras y atributos

Este artículo explica los conceptos de detección de caras y datos de atributos de cara. Detección de caras es la acción de buscar caras humanas en una imagen y, opcionalmente, devolviendo una variedad de datos relacionados con la cara.

Usa el [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) operación detectar caras en una imagen. Como mínimo, cada detectado caras se corresponden con un **faceRectangle** campo en la respuesta. Se trata de un conjunto de coordenadas de píxel (izquierda, superior, width, height) marcar la cara que encuentra. Con estas coordenadas, puede obtener la ubicación de la cara, así como su tamaño. En la respuesta de API, caras se muestran en orden de tamaño de mayor a menor.

## <a name="face-id"></a>Id. de cara

El identificador de rostros es simplemente una cadena de identificador único para cada rostro detectado en una imagen. Puede solicitar un identificador facial en sus [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) llamada API.

## <a name="face-landmarks"></a>Puntos de referencia de cara

Los puntos de referencia son un conjunto de puntos de encontrar en una cara, como los alumnos o la sugerencia de punta. De forma predeterminada, existen 27 puntos de referencia predefinidos. La siguiente ilustración muestra todos los puntos de 27:

![Un diagrama de cara a todos los 27 puntos de referencia con la etiqueta](../Images/landmarks.1.jpg)

Se devuelven las coordenadas de los puntos en unidades de píxeles.

## <a name="attributes"></a>Atributos

Los atributos son un conjunto de características de cara adicional que opcionalmente se pueden detectar el [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API. Los siguientes son atributos que pueden detectarse:

* **Edad** calcula que la edad, en años, de una cara determinada.
* **Desenfoque** el desenfoque de la cara de la imagen. Esto devuelve un valor entre cero y uno, así como una clasificación informal ("baja", "medium", "alto").
* **Exposición** la exposición de la cara de la imagen. Esto devuelve un valor entre cero y uno, así como una clasificación informal ("subexposición", "goodExposure", "sobreexposición").
* **Emotion** una lista de emociones con su confianza de detección para la cara determinada. Se normalizan las puntuaciones de confianza: las puntuaciones en todas las emociones agregará hasta uno. Las emociones devueltas son felicidad, tristeza, neutral, ira, desprecio, asco, sorpresa y miedo.
* **Vello facial** la presencia de estimado vello facial y la longitud de la cara determinada.
* **Sexo** el sexo estimado de la cara determinada. Los valores posibles son "male", "female" y "genderless".
* **Gafas** si la cara determinada tiene gafas. Los valores posibles son "NoGlasses", "ReadingGlasses", "Gafas de sol" y "Swimming gafas".
* **Pelo** el estilo de selección precisa de la cara. Esto muestra si está visible el cabello, si se detecta baldness y se detectan los colores de selección precisa.
* **Head suponer** orientación de la cara en un espacio 3D. Esto se describe mediante el cabeceo y puesta ángulos de rotación en grados. Los intervalos de valores son [-90, 90], [-180, 180] y [-90, 90] grados, respectivamente. Consulte el siguiente diagrama de asignaciones de angular:

    ![Un encabezado con el tono, revertir y guiñada ejes con la etiqueta](../Images/headpose.1.jpg)
* **Composición** si la cara tiene maquillaje. Esto devuelve un valor booleano para "eyeMakeup" y "lipMakeup".
* **Ruido** ruido visual presente detectado en la imagen. Esto devuelve un valor entre cero y uno, así como una clasificación informal ("baja", "medium", "alto").
* **Oclusión** si hay partes de la cara de bloqueo de objetos. Esto devuelve un valor booleano para "eyeOccluded", "foreheadOccluded" y "mouthOccluded".
* **SMILE** la expresión sonrisa de la cara determinada. Se trata de un valor entre cero (no sonrisa) y uno (sonrisa claro).

> [!IMPORTANT]
> Los atributos de cara se predicen mediante el uso de algoritmos estadísticos y puede que no siempre sea precisos. Tenga cuidado al tomar decisiones basadas en datos de atributo.

## <a name="input-data"></a>Datos de entrada

Use las siguientes sugerencias para asegurarse de que las imágenes de entrada proporcionan los resultados más precisos de detección:

* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF (el primer fotograma), BMP.
* Tamaño del archivo de imagen debe ser mayor de 4 MB.
* El intervalo de tamaños de cara detectable es 36 x 36 a 4096 x 4096 píxeles. No se puede detectar caras fuera de este intervalo.
* Algunas caras podrían no detectarse debido a desafíos técnicos. Extremas ángulos de cara (postura principal) u oclusión cara (objetos como gafas de sol o parte de la cara de bloqueo de manos) puede afectar a la detección. Las caras frontales y de frente cercanas ofrecen los mejores resultados.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya está familiarizado con conceptos de detección de caras, obtenga información sobre cómo escribir un sencillo script que detecta caras en una imagen determinada.

* [Cómo detectar caras en una imagen](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)