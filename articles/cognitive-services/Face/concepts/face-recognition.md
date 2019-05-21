---
title: Conceptos de reconocimiento de caras
titleSuffix: Azure Cognitive Services
description: Aprenda los conceptos acerca del reconocimiento de caras.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pafarley
ms.openlocfilehash: fa38c492530cb8938e49bc15e13fdd39ed5b6f1c
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65890884"
---
# <a name="face-recognition-concepts"></a>Conceptos de reconocimiento de caras

En este artículo se explica los conceptos de las operaciones de reconocimiento de caras Compruebe, buscar Similar, grupo e identificar y las estructuras de datos subyacente. En términos generales, reconocimiento describe el trabajo de comparar dos caras diferentes para determinar si son similares o pertenecer a la misma persona.

## <a name="recognition-related-data-structures"></a>Estructuras de datos relacionados con el reconocimiento

Las operaciones de reconocimiento usan principalmente las siguientes estructuras de datos. Estos objetos se almacenan en la nube y pueden hacer referencia a sus cadenas de identificador. Las cadenas de identificador siempre son únicas dentro de una suscripción. Se pueden duplicar los campos de nombre.

|NOMBRE|DESCRIPCIÓN|
|:--|:--|
|DetectedFace| Esta representación sola cara se recupera mediante la [detección de caras](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md) operación. Su identificador expira 24 horas después de crearlo.|
|PersistedFace| Cuando se agregan objetos DetectedFace a un grupo, como FaceList o persona, se convierten en objetos PersistedFace. Pueden ser [recuperar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) en un tiempo y no caducan.|
|[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b) o [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)| Esta estructura de datos es una lista de objetos PersistedFace ordenada. Un FaceList tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario.|
|[Person](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)| Esta estructura de datos es una lista de objetos PersistedFace que pertenecen a la misma persona. Tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario.|
|[Grupo de personas](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) o [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)| Esta estructura de datos es una lista de objetos Person ordenada. Tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario. Debe ser un grupo de personas [entrenado](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) antes de que se puede utilizar en operaciones de reconocimiento.|

## <a name="recognition-operations"></a>Operaciones de reconocimiento

Esta sección detalla el usan de las cuatro operaciones de reconocimiento en las estructuras de datos se ha descrito anteriormente. Para una amplia descripción de cada operación de reconocimiento, consulte [Introducción](../Overview.md).

### <a name="verify"></a>Verify

El [compruebe](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) operación toma un identificador de la cara de DetectedFace o PersistedFace y otro Id. de interfaz o un objeto Person y determina si pertenecen a la misma persona. Si se pasa un objeto Person, opcionalmente, puede pasar en un grupo de personas al que pertenece esa persona para mejorar el rendimiento.

### <a name="find-similar"></a>Buscar similares

El [buscar Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) operación toma un identificador facial de DetectedFace o PersistedFace y un FaceList o una matriz de los otros identificadores de los rostros. Con un FaceList, devuelve un FaceList más pequeño de caras que son similares a la cara determinada. Con una matriz de identificadores de rostro, del mismo modo devuelve una matriz más pequeña.

### <a name="group"></a>Grupo

El [grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) operación toma una matriz de identificadores ordenado que se enfrentan desde DetectedFace o PersistedFace y devuelve los mismos identificadores que se agrupan en varias matrices más pequeñas. Cada matriz de "grupos" contiene identificadores de los que se parecen rostros. Una matriz única "messyGroup" contiene face ID para que se han encontrado ninguna similitudes.

### <a name="identify"></a>Identificar

El [identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) operación toma uno o varios identificadores de los rostros de DetectedFace o PersistedFace y un grupo de personas y devuelve una lista de objetos Person que cada cara puede pertenecer a. Devuelve objetos se encapsulan como objetos de candidato, que tienen un valor de predicción de confianza de persona.

## <a name="input-data"></a>Datos de entrada

Para asegurarse de que las imágenes de entrada proporcionan los resultados más precisos de reconocimiento, utilice las siguientes sugerencias:

* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF (el primer fotograma), BMP.
* Tamaño del archivo de imagen debe ser mayor de 4 MB.
* Cuando crea objetos Person, use las fotos que distintos tipos de ángulos y la iluminación de características.
* Algunas caras podrían no reconocer debido a desafíos técnicos, como:
  * Las imágenes con iluminación extremas, por ejemplo, grave luz de fondo.
  * Obstrucciones que bloquean los ojos de uno o ambos.
  * Diferencias en vello facial o de tipo de selección precisa.
  * Cambios en apariencia facial debido a la edad.
  * Expresiones faciales extremas.

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con conceptos de reconocimiento de caras, obtenga información sobre cómo escribir un script que identifica las caras de frente a un grupo de personas entrenado.

* [Identificación de caras en imágenes](../Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)