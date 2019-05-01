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
ms.openlocfilehash: 8c0bf001c2bdbedaa041dbd10b5c7edb08eec4c6
ms.sourcegitcommit: 524625dd12e0537173616a991802075e2dc9da12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "64416004"
---
# <a name="face-recognition-concepts"></a>Conceptos de reconocimiento de caras

En este artículo se explica los conceptos de las distintas operaciones de reconocimiento de caras (comprobación, busque de forma similar, agrupación, identificación) y las estructuras de datos subyacente. Ampliamente, reconocimiento describe el trabajo de comparar dos caras diferentes para determinar si son similares o pertenecer a la misma persona.

## <a name="recognition-related-data-structures"></a>Estructuras de datos relacionados con el reconocimiento

Las operaciones de reconocimiento usan principalmente las siguientes estructuras de datos. Estos objetos se almacenan en la nube y pueden hacer referencia a sus cadenas de identificador. Por lo tanto, las cadenas de identificador siempre son únicas dentro de una suscripción, mientras que se pueden duplicar los campos de nombre.

|NOMBRE|DESCRIPCIÓN|
|:--|:--|
|**DetectedFace**| Esta es una representación única cara recuperada por la [detección de caras](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md) operación. Su identificador expira 24 horas después de crearlo.|
|**PersistedFace**| Cuando **DetectedFace** los objetos se agregan a un grupo (como **FaceList** o **persona**), estos se convierten en **PersistedFace** objetos, que pueden ser [recuperar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) en cualquiera de tiempo y que no expiran.|
|**[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b)**/**[LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)**| Esta es una lista de diversas **PersistedFace** objetos. Un **FaceList** tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario.|
|**[Persona](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)**| Se trata de una lista de **PersistedFace** objetos que pertenecen a la misma persona. Tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario.|
|**[PersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)**/**[LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)**| Esta es una lista de diversas **persona** objetos. Tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario. Un **grupo de personas** debe ser [entrenado](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) antes de que se puede utilizar en operaciones de reconocimiento.|

## <a name="recognition-operations"></a>Operaciones de reconocimiento

Esta sección detalla el usan de las cuatro operaciones de reconocimiento en las estructuras de datos anterior. Consulte la [Introducción](../Overview.md) para una amplia descripción de cada operación de reconocimiento.

### <a name="verification"></a>Comprobación

El [compruebe](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) operación toma un identificador facial (**DetectedFace** o **PersistedFace**) y cualquier otro face ID o un **persona** objeto y Determina si pertenecen a la misma persona. Si se pasa un **persona**, también se pueden pasar en un **grupo de personas** en los que **persona** pertenece con el fin de mejorar el rendimiento.

### <a name="find-similar"></a>Buscar similares

El [Buscar similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) operación toma un identificador facial (**DetectedFace** o **PersistedFace**) y un **FaceList** o una matriz de otro cara Identificadores. Con un **FaceList**, devuelve un menor **FaceList** de caras que son similares a la cara determinada. Con una matriz de identificadores de rostro, del mismo modo devuelve una matriz más pequeña.

### <a name="grouping"></a>Agrupación

El [grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) operación toma una matriz de identificadores ordenado que se enfrentan (**DetectedFace** o **PersistedFace**) y devuelve los mismos identificadores que se agrupan en varias matrices más pequeñas. Cada matriz de "grupos" contiene face ID que se parecen y una matriz única "messyGroup" face ID para que se han encontrado ninguna similitudes.

### <a name="identification"></a>Identificación

El [identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) operación toma uno o varios identificadores de los rostros (**DetectedFace** o **PersistedFace**) y un **grupo de personas** y devuelve una lista de **persona** que cada cara puede pertenecer a los objetos. Devuelve **persona** encapsula los objetos como **candidato** objetos, que tienen un valor de predicción de confianza.

## <a name="input-data"></a>Datos de entrada

Para asegurarse de que las imágenes de entrada proporcionan los resultados más precisos de reconocimiento, utilice las siguientes sugerencias:

* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF (el primer fotograma), BMP.
* Tamaño del archivo de imagen debe ser mayor de 4 MB.
* Al crear **persona** objetos, debe usar las fotos que presentan una variedad de ángulos y la iluminación.
* Algunas caras no reconoce debido a desafíos técnicos:
  * Imágenes con iluminación extreme (por ejemplo, retroiluminación grave).
  * Obstrucciones bloqueando los ojos de uno o ambos.
  * Diferencias de pelo facial o de estilo de selección precisa.
  * Cambios en apariencia cara debido a la edad.
  * Expresiones faciales extremas.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya está familiarizado con conceptos de reconocimiento de caras, obtenga información sobre cómo escribir una secuencia de comandos simple que identifica las caras contra un modelo **grupo de personas**.

* [Cómo identificar caras en imágenes](../Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)