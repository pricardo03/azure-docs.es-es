---
title: Detectar caras en una imagen - Face API
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar los datos de distintos devueltos por la característica de detección de caras.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: sbowles
ms.openlocfilehash: 46bd1bdd55725878bc7b1bd55d5e24b78d82aada
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124551"
---
# <a name="get-face-detection-data"></a>Obtener datos de detección de caras

Esta guía muestra cómo usar la detección de caras para extraer los atributos como sexo, edad o postura de una imagen determinada. Los fragmentos de código en esta guía se escriben C# mediante el uso de la biblioteca de cliente de Azure Cognitive Services Face API. La misma funcionalidad está disponible a través de la [API de REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Esta guía le mostrará cómo para:

- Obtener las ubicaciones y las dimensiones de caras en una imagen.
- Obtener las ubicaciones de diversos faciales, por ejemplo, alumnos, nariz y boca en una imagen.
- Adivinar el sexo, edad, emoción y otros atributos de un rostro detectado.

## <a name="setup"></a>Configuración

Esta guía se da por supuesto que construye una [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) objeto, denominado `faceClient`, con una URL de clave y el punto de conexión de suscripción de cara. Desde aquí, puede usar la característica de detección de caras llamando [DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet), que se usa en esta guía, o [DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet). Para obtener instrucciones sobre cómo configurar esta característica, consulte el [detectar caras de inicio rápido para C# ](../quickstarts/csharp-detect-sdk.md).

Esta guía se centra en los detalles de la llamada de detectar, como los argumentos que se pueden pasar y lo que puede hacer con los datos devueltos. Se recomienda consultar sólo las características que necesita. Cada operación tarda más tiempo en completarse.

## <a name="get-basic-face-data"></a>Obtener datos básicos de cara

Para encontrar caras y obtener sus ubicaciones en una imagen, llame al método con el _returnFaceId_ parámetro establecido en **true**. Esta es la configuración predeterminada.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

Puede consultar el valor devuelto [DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet) identificadores y un rectángulo que proporciona las coordenadas de píxel de la cara de objetos para su único.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Para obtener información sobre cómo analizar la ubicación y las dimensiones de la cara, consulte [FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet). Normalmente, este rectángulo contiene los ojos, arqueadas, nariz y boca. La parte superior de la cabeza, oídos y chin no están incluidos necesariamente. Para usar el rectángulo facial para recortar un encabezado completo u obtener un retrato captura intermedio, quizás para una imagen de tipo de identificador de foto, puede expandir el rectángulo en cada dirección.

## <a name="get-face-landmarks"></a>Obtener faciales

[Puntos de referencia se enfrentan](../concepts/face-detection.md#face-landmarks) son un conjunto de puntos de encontrar en una cara, por ejemplo, los alumnos o la sugerencia de la punta. Para obtener datos de punto de referencia de cara, establezca el _returnFaceLandmarks_ parámetro **true**.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

El código siguiente muestra cómo se pueden recuperar las ubicaciones de la nariz y alumnos:

```csharp
foreach (var face in faces)
{
    var landmarks = face.FaceLandmarks;

    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;

    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;

    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
```

También puede usar datos de puntos de referencia de cara para calcular con precisión la dirección de la cara. Por ejemplo, puede definir la rotación de la cara como un vector desde el centro de la boca hasta el centro de los ojos. El código siguiente calcula este vector:

```csharp
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;

var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);

var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;

var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);

Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
```

Cuando conoce la dirección de la cara, puede girar el marco rectangular cara para alinearlo más correctamente. Para recortar caras en una imagen, mediante programación puede girar la imagen para que las caras siempre aparecen en verticales.

## <a name="get-face-attributes"></a>Obtener los atributos de cara

Además de los rectángulos y puntos de referencia, la API de detección de caras puede analizar varios atributos conceptuales de una cara. Para obtener una lista completa, consulte el [atributos faciales](../concepts/face-detection.md#attributes) sección conceptual.

Para analizar los atributos de cara, establezca el _returnFaceAttributes_ parámetro a una lista de [FaceAttributeType Enum](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet) valores.

```csharp
var requiredFaceAttributes = new FaceAttributeType[] {
    FaceAttributeType.Age,
    FaceAttributeType.Gender,
    FaceAttributeType.Smile,
    FaceAttributeType.FacialHair,
    FaceAttributeType.HeadPose,
    FaceAttributeType.Glasses,
    FaceAttributeType.Emotion
};
var faces = await faceClient.DetectWithUrlAsync(imageUrl, true, false, requiredFaceAttributes);
```

A continuación, obtener referencias a los datos devueltos y realizar operaciones más según sus necesidades.

```csharp
foreach (var face in faces)
{
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
    var emotion = attributes.Emotion;
}
```

Para obtener más información acerca de cada uno de los atributos, vea el [Face detection y atributos](../concepts/face-detection.md) guía conceptual.

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido cómo utilizar las diversas funcionalidades de detección de caras. A continuación, integrar estas características en la aplicación siguiendo un exhaustivo tutorial.

- [Tutorial: Crear una aplicación WPF para mostrar los datos de cara en una imagen](../Tutorials/FaceAPIinCSharpTutorial.md)
- [Tutorial: Crear una aplicación Android para detectar y caras en una imagen de marco](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)

## <a name="related-topics"></a>Temas relacionados

- [Documentación de referencia (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Documentación de referencia (SDK de .NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/face?view=azure-dotnet)