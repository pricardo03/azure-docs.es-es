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
ms.openlocfilehash: e61048eeab9d7061c18f3237db22fc87ca52f526
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65891143"
---
# <a name="face-detection-and-attributes"></a>Detección de caras y atributos

Este artículo explica los conceptos de detección de caras y datos de atributos de cara. Detección de caras es la acción de búsqueda de caras humanas en una imagen y opcionalmente devolver distintos tipos de datos relacionados con la cara.

Usa el [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) operación detectar caras en una imagen. Como mínimo, cada rostro detectado corresponde a un campo faceRectangle en la respuesta. Este conjunto de coordenadas de píxel de la izquierda, superior, ancho y alto de marcar la cara que encuentra. Con estas coordenadas, puede obtener la ubicación de la cara y su tamaño. En la respuesta de API, caras se muestran en orden de tamaño de mayor a menor.

## <a name="face-id"></a>Id. de cara

El identificador de rostros es una cadena de identificador único para cada rostro detectado en una imagen. Puede solicitar un identificador facial en sus [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) llamada API.

## <a name="face-landmarks"></a>Puntos de referencia de cara

Los puntos de referencia son un conjunto de puntos de encontrar en una cara, por ejemplo, los alumnos o la sugerencia de la punta. De forma predeterminada, existen 27 puntos de referencia predefinidos. La siguiente ilustración muestra todos los puntos de 27:

![Un diagrama de cara a todos los 27 puntos de referencia con la etiqueta](../Images/landmarks.1.jpg)

Se devuelven las coordenadas de los puntos en unidades de píxeles.

## <a name="attributes"></a>Atributos

Los atributos son un conjunto de características que opcionalmente se pueden detectar el [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API. Se pueden detectar los siguientes atributos:

* **Edad**. La antigüedad estimada en años de una cara determinada.
* **Desenfoque**. El desenfoque de la cara de la imagen. Este atributo devuelve un valor entre cero y uno y una clasificación informal de baja, Media o alta.
* **Emotion**. Una lista de emociones con su confianza de detección para la cara determinada. Se normalizan las puntuaciones de confianza y agregue las puntuaciones en todas las emociones en uno. Las emociones devueltas son felicidad, tristeza, neutral, ira, desprecio, asco, sorpresa y miedo.
* **Exposición**. La exposición de la cara de la imagen. Este atributo, devuelve un valor entre cero y uno y una clasificación de subexposición, goodExposure o sobreexposición informal.
* **Vello facial**. La presencia de estimado vello facial y la longitud de la cara determinada.
* **Sexo**. Sexo estimado de la cara determinada. Los valores posibles son hombre, mujer y genderless.
* **Gafas**. Si la cara determinada tiene gafas. Los valores posibles son NoGlasses, ReadingGlasses, gafas de sol y natación anteojos.
* **Pelo**. El tipo de selección precisa de la cara. Este atributo muestra si está visible el cabello, si se detecta baldness y se detectan los colores de selección precisa.
* **Head suponer**. Orientación de la cara en un espacio 3D. Este atributo se describe mediante el cabeceo y puesta ángulos de rotación en grados. Los intervalos de valores son los grados de-90 a 90 grados, de-180 grados a 180 grados y grados de-90 a 90 grados, respectivamente. Consulte el siguiente diagrama de asignaciones de angular:

    ![Un encabezado con el tono, revertir y guiñada ejes con la etiqueta](../Images/headpose.1.jpg)
* **Composición**. Si la cara tiene maquillaje. Este atributo devuelve un valor booleano para eyeMakeup y lipMakeup.
* **Ruido**. El ruido visual detectado en la imagen. Este atributo devuelve un valor entre cero y uno y una clasificación informal de baja, Media o alta.
* **Oclusión**. Si hay objetos bloqueos partes de la cara. Este atributo devuelve un valor booleano para eyeOccluded foreheadOccluded y mouthOccluded.
* **SMILE**. La expresión de sonrisa de la cara determinada. Este valor está entre cero para no sonrisa y otro para una sonrisa clara.

> [!IMPORTANT]
> Se puede predecir atributos faciales mediante el uso de algoritmos estadísticos. Podrían no ser siempre precisas. Tenga cuidado al realizar decisiones basadas en datos de atributo.

## <a name="input-data"></a>Datos de entrada

Utilice las siguientes sugerencias para asegurarse de que las imágenes de entrada proporcionan los resultados más precisos de detección:

* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF para el primer fotograma y BMP.
* El tamaño del archivo de imagen debe ser no más de 4 MB.
* El intervalo de tamaño de cara detectable es 36 x 36 a 4096 × 4096 píxeles. No se puede detectar caras fuera de este intervalo.
* Algunas caras podrían no detectarse debido a desafíos técnicos. Extremas ángulos de cara (postura principal) u oclusión cara (objetos como gafas de sol o en las manos que bloquear parte de la cara) puede afectar a la detección. Las caras frontales y de frente cercanas ofrecen los mejores resultados.

Si está detectando caras de una fuente de vídeo, puede mejorar el rendimiento mediante el ajuste de determinadas opciones de la cámara de vídeo:

* **Suavizado**: Muchas cámaras de vídeo se aplica un efecto de suavizado. Debe desactivar esta opción si es posible porque crea un desenfoque entre marcos y reduce la claridad.
* **Velocidad de obturación**: Una velocidad de obturación reduce la cantidad de movimiento entre marcos y hace que cada fotograma más clara. Se recomienda que las velocidades de obturación de 1/60 segundos o más rápido.
* **Ángulo de obturación**: Algunas cámaras especifican el ángulo del obturador en lugar de velocidad de obturación. Debe usar un ángulo del obturador inferior si es posible. Esto provocará en fotogramas de vídeo más claras.

    >[!NOTE]
    > Una cámara con un ángulo inferior del obturador recibirán menos luz en cada fotograma, por lo que la imagen será más oscura. Deberá determinar el nivel adecuado para usar.

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con conceptos de detección de caras, obtenga información sobre cómo escribir un script que detecta caras en una imagen determinada.

* [Detectar caras en una imagen](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)