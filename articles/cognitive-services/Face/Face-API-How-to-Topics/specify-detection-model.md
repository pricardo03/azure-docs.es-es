---
title: Cómo especificar un modelo de detección - Face API
titleSuffix: Azure Cognitive Services
description: En este artículo le mostrará cómo elegir qué modelo de detección de caras para usar con su aplicación de Azure Face API.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: yluiu
ms.openlocfilehash: dde5623bf5bd579a13fa7271dfba64f9df61bad1
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66576702"
---
# <a name="specify-a-face-detection-model"></a>Especifique un modelo de detección de caras

Esta guía muestra cómo especificar un modelo de detección de caras para Face API de Azure.

Face API utiliza los modelos de aprendizaje automático para realizar operaciones en caras humanas en imágenes. Seguimos mejorando la precisión de nuestros modelos basados en comentarios de los clientes y avances en la investigación, y se ofrecen estas mejoras como actualizaciones del modelo. Los desarrolladores tienen la opción para especificar qué versión del modelo de detección de caras le gustaría utilizar; Puede elegir el modelo que mejor se adapte a sus casos de uso.

Siga leyendo para obtener información sobre cómo especificar el modelo de detección de caras en ciertas operaciones caras. Face API utiliza la detección de caras cada vez que convierte una imagen de una cara en algún otro tipo de datos.

Si no está seguro de si debe utilizar el modelo más reciente, vaya a la [evaluar modelos diferentes](#evaluate-different-models) sección para evaluar el modelo nuevo y comparar los resultados con el conjunto de datos actual.

## <a name="prerequisites"></a>Requisitos previos

Debe estar familiarizado con el concepto de detección de caras de inteligencia artificial. Si no, consulte la guía conceptual de detección de caras o Guía de procedimientos:

* [Conceptos de detección de caras](../concepts/face-detection.md)
* [Cómo detectar caras en una imagen](HowtoDetectFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Detectar caras con el modelo especificado

Detección de caras busca las ubicaciones del cuadro de límite de caras humanas e identifica sus puntos de referencia visuales. Extrae las características de la cara y almacenarlos para su uso posterior en [reconocimiento](../concepts/face-recognition.md) operaciones.

Cuando se usa el [enfrentan: detectar] API, puede asignar la versión del modelo con el `detectionModel` parámetro. Los valores disponibles son:

* `detection_01`
* `detection_02`

Una dirección URL de solicitud para el [enfrentan: detectar] API de REST tendrá este aspecto:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel][&detectionModel]
&subscription-key=<Subscription key>`

Si usa la biblioteca de cliente, puede asignar el valor de `detectionModel` pasando una cadena adecuada. Si lo deja sin asignar, la API utilizará la versión del modelo predeterminado (`detection_01`). Consulte el siguiente ejemplo de código para la biblioteca de cliente.

```csharp
string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.Face.DetectWithUrlAsync(imageUrl, false, false, recognitionModel: "recognition_02", detectionModel: "detection_02");
```

## <a name="add-face-to-person-with-specified-model"></a>Agregar cara a la persona con el modelo especificado

Face API puede extraer datos de la cara de una imagen y asociarla con un **persona** objeto a través de la [persona del grupo de personas - agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API. En esta llamada API, puede especificar el modelo de detección de la misma manera que en [enfrentan: detectar].

Consulte el siguiente ejemplo de código para la biblioteca de cliente.

```csharp
// Create a PersonGroup and add a person with face detected by "detection_02" model
string personGroupId = "mypersongroupid";
await faceServiceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");

string personId = (await faceServiceClient.PersonGroupPerson.CreateAsync(personGroupId, "My Person Name")).PersonId;

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.PersonGroupPerson.AddFaceFromUrlAsync(personGroupId, personId, imageUrl, detectionModel: "detection_02");
```

Este código crea un **grupo de personas** con el identificador `mypersongroupid` y agrega un **persona** a él. A continuación, agrega una cara a este **persona** utilizando el `detection_02` modelo. Si no se especifica la *detectionModel* parámetro, la API utilizará el modelo predeterminado, `detection_01`.

> [!NOTE]
> No es necesario usar el mismo modelo de detección para todas las caras en una **persona** objeto y no es necesario utilizar el mismo modelo de detección para detectar caras nuevas que se compara con un **persona** objeto (en el [Face - Identify] API, por ejemplo).

## <a name="add-face-to-facelist-with-specified-model"></a>Agregar cara a FaceList con el modelo especificado

También puede especificar un modelo de detección al agregar una cara a una existente **FaceList** objeto. Consulte el siguiente ejemplo de código para la biblioteca de cliente.

```csharp
await faceServiceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.FaceList.AddFaceFromUrlAsync(faceListId, imageUrl, detectionModel: "detection_02");
```

Este código crea un **FaceList** llamado `My face collection` y le agrega una cara con el `detection_02` modelo. Si no se especifica la *detectionModel* parámetro, la API utilizará el modelo predeterminado, `detection_01`.

> [!NOTE]
> No es necesario usar el mismo modelo de detección para todas las caras en una **FaceList** objeto y no es necesario utilizar el mismo modelo de detección para detectar caras nuevas que se compara con un **FaceList** objeto.

## <a name="evaluate-different-models"></a>Evaluación de modelos diferentes

Los modelos de detección de caras diferentes están optimizados para diferentes tareas. Consulte la tabla siguiente para obtener información general de las diferencias.

|**detection_01**  |**detection_02**  |
|---------|---------|
|Opción predeterminada para todas las operaciones de detección de caras. | Publicado en mayo de 2019 y está disponible si lo desea, en todas las operaciones de detección de caras.
|No se ha optimizado para pequeña, vista de lado o caras borrosas.  | Precisión mejorada en pequeño, vista de lado y caras borrosas. |
|Devuelve los atributos (postura principal, edad, emoción etc.) enfrenta a si se especifican en la llamada de detectar. |  No devuelve los atributos de cara.     |
|Devuelve enfrenta a puntos de referencia si se especifican en la llamada de detectar.   | No devuelve los puntos de referencia.  |

La mejor manera de comparar el rendimiento de la `detection_01` y `detection_02` modelos consiste en utilizarlos en un conjunto de datos de ejemplo. Se recomienda llamar a la [enfrentan: detectar] API en una variedad de imágenes, especialmente las imágenes de muchas caras o de caras que son difíciles de ver, con cada modelo de detección. Preste atención al número de caras que devuelve cada modelo.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo especificar el modelo de detección para usar con diferentes Face API. A continuación, siga una guía de inicio rápido para empezar a usar la detección de caras.

* [Detectar caras en una imagen (SDK de .NET)](../quickstarts/csharp-detect-sdk.md)

[Enfrentan: detectar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Face - Find Similar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Face - Identify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Face - Verify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Train]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[FaceList - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250
[LargeFaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc