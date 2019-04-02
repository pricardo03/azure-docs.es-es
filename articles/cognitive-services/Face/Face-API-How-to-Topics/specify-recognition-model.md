---
title: Cómo especificar un modelo de reconocimiento - Face API
titleSuffix: Azure Cognitive Services
description: En este artículo le mostrará cómo elegir qué modelo de reconocimiento para usar con su aplicación de Azure Face API.
services: cognitive-services
author: longl
manager: nitinme
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: longl
ms.openlocfilehash: 33348e637143b923719425b9674f99a475d848d9
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58806669"
---
# <a name="specify-a-face-recognition-model"></a>Especifique un modelo de reconocimiento de caras

Esta guía muestra cómo especificar un modelo de reconocimiento de caras para la detección de caras, la identificación y la búsqueda de similitud con Face API de Azure.

Face API utiliza los modelos de aprendizaje automático para realizar operaciones en caras humanas en imágenes. Seguimos mejorando la precisión de nuestros modelos basados en comentarios de los clientes y avances en la investigación, y se ofrecen estas mejoras como actualizaciones del modelo. Los desarrolladores tienen la opción para especificar qué versión del modelo de reconocimiento de caras le gustaría utilizar; Puede elegir el modelo que mejor se adapte a sus casos de uso.

Si es un nuevo usuario, se recomienda que usar el modelo más reciente. Siga leyendo para obtener información sobre cómo especificar de distintas operaciones de cara al tiempo que evita conflictos de modelo. Si es un usuario avanzado y no está seguro de si debe cambiar al modelo más reciente, vaya a la [evaluar modelos diferentes](#evaluate-different-models) sección para evaluar el modelo nuevo y comparar los resultados con el conjunto de datos actual.

## <a name="prerequisites"></a>Requisitos previos

Debe estar familiarizado con los conceptos de identificación y detección de caras de inteligencia artificial. Si no, consulte estas guías de procedimientos en primer lugar:

* [Cómo detectar caras en una imagen](HowtoDetectFacesinImage.md)
* [Cómo identificar caras en una imagen](HowtoIdentifyFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Detectar caras con el modelo especificado

Detección de caras identifica los puntos de referencia visuales de caras humanas y busca sus ubicaciones del cuadro de límite. También extrae las características de la cara y almacenarlos para su uso en la identificación. Toda esta información constituye la representación de una cara.

El modelo de reconocimiento se usa cuando se extraen las características faciales, por lo que puede especificar una versión de modelo al realizar la operación de detección.

Cuando se usa el [enfrentan: detectar] API, asigne a la versión de modelo con el `recognitionModel` parámetro. Los valores disponibles son:

* `recognition_01`
* `recognition_02`

Si lo desea, puede especificar el _returnRecognitionModel_ parámetro (valor predeterminado **false**) para indicar si _recognitionModel_ se deben devolver en la respuesta. Por lo tanto, una dirección URL de solicitud para el [enfrentan: detectar] API de REST tendrá este aspecto:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel]
&subscription-key=<Subscription key>`

Si usa la biblioteca de cliente, puede asignar el valor de `recognitionModel` pasando una cadena que representa la versión.
Si lo deja sin asignar, la versión del modelo predeterminado (_recognition_01_) se usará. Consulte el siguiente ejemplo de código para la biblioteca de cliente.

```csharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.Face.DetectWithUrlAsync(imageUrl, true, true, recognitionModel: "recognition_02", returnRecognitionModel: true);
```

## <a name="identify-faces-with-specified-model"></a>Identifique caras con el modelo especificado

Face API puede extraer datos de la cara de una imagen y asociarla con un **persona** objeto (a través de la [agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) llamada de API, por ejemplo) y varios **persona** pueden ser objetos almacenan juntos en un **grupo de personas**. A continuación, se puede comparar una cara nueva con un **grupo de personas** (con el [Face - Identify] llamar), y se puede identificar la persona correspondiente dentro de ese grupo.

Un **grupo de personas** debe tener un modelo de reconocimiento único para todas la **persona**s y puede especificar esto mediante la `recognitionModel` parámetro al crear el grupo ([PersonGroup - Create] o [LargePersonGroup - Create]). Si no especifica este parámetro, el original `recognition_01` se usa el modelo. Siempre usará el modelo de reconocimiento que se creó con un grupo y caras nuevas estará asociadas con este modelo cuando se agregan a él; no se puede cambiar después de la creación de un grupo. Para ver qué modelo un **grupo de personas** está configurada, utilice el [grupo de personas - Get] API con el _returnRecognitionModel_ establecido como **true**.

Consulte el siguiente ejemplo de código para la biblioteca de cliente.

```csharp
// Create an empty PersonGroup with "recognition_02" model
string personGroupId = "mypersongroupid";
await faceServiceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");
```

En este código, un **grupo de personas** con el identificador `mypersongroupid` se crea, y está configurado para usar el _recognition_02_ modelo para extraer características faciales.

En consecuencia, deberá especificar qué modelo utilizar al detectar caras para comparar con este **grupo de personas** (a través de la [enfrentan: detectar] API). El modelo que utilice siempre debe ser coherente con la **grupo de personas**de configuración; en caso contrario, se producirá un error la operación debido a los modelos incompatibles.

No hay ningún cambio en el [Face - Identify] API; basta con especificar la versión del modelo de detección.

## <a name="find-similar-faces-with-specified-model"></a>Busque caras parecidas con el modelo especificado

También puede especificar un modelo de reconocimiento para la búsqueda de similitud. Puede asignar la versión del modelo con `recognitionModel` al crear la lista de caras con [FaceList - crear] API o [LargeFaceList - crear]. Si no especifica este parámetro, el original `recognition_01` se usa el modelo. Siempre usará el modelo de reconocimiento que se creó con una lista de caras y caras nuevas estará asociadas con este modelo cuando se agregan a él; no se puede cambiar después de su creación. Para ver qué modelo se configura una lista de caras con, use el [FaceList - Get] API con el _returnRecognitionModel_ establecido como **true**.

Consulte el siguiente ejemplo de código para la biblioteca de cliente.

```csharp
await faceServiceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");
```

Este código crea una lista de caras denominada `My face collection`, usando la _recognition_02_ modelo de extracción de características. Al buscar en esta lista de caras para caras parecidas a un nuevo rostro detectado, que se enfrentan debe haber detectado ([enfrentan: detectar]) mediante el _recognition_02_ modelo. Como se muestra en la sección anterior, el modelo debe ser coherente.

No hay ningún cambio en el [cara - buscar Similar] API; debe especificar solo la versión del modelo de detección.

## <a name="verify-faces-with-specified-model"></a>Verifique caras con el modelo especificado

El [Enfrentan: comprobación] API comprueba si dos caras pertenecen a la misma persona. No hay ningún cambio en la API de comprobar con respecto a los modelos de reconocimiento, pero solo se pueden comparar caras que se han detectado con el mismo modelo. Por lo tanto, las dos caras ambos tendrá que se han detectado mediante `recognition_01` o `recognition_02`.

## <a name="evaluate-different-models"></a>Evaluación de modelos diferentes

Si desea comparar el rendimiento de la _recognition_01_ y _recognition_02_ modelos en los datos, tendrá que:

1. Cree dos **grupo de personas**s con _recognition_01_ y _recognition_02_ respectivamente.
1. Usar los datos de imagen para detectar caras y registrarlos en **persona**s para estos dos **grupo de personas**s y la formación de desencadenador procesan con [PersonGroup - Train] API.
1. Probar con [Face - Identify] en ambos **grupo de personas**s y comparar los resultados.

Si especifica un umbral de confianza (un valor entre cero y uno que determina el nivel de confianza debe ser el modelo identificar una cara) normalmente, deberá utilizar diferentes umbrales para los distintos modelos. Un umbral para un modelo no está diseñado para compartirse a otro y no producirá necesariamente los mismos resultados.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo especificar el modelo de reconocimiento para usar con el servicio de Face API diferentes. A continuación, siga una guía de inicio rápido para empezar a usar la detección de caras.

* [Detectar caras en una imagen](../quickstarts/csharp-detect-sdk.md)

[Enfrentan: detectar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Cara - buscar Similar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Face - Identify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Enfrentan: comprobación]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[Grupo de personas - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Train]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - crear]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[LargeFaceList - crear]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc
