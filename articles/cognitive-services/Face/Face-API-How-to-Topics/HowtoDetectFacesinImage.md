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
ms.date: 02/22/2019
ms.author: sbowles
ms.openlocfilehash: bf3af8f5d1d2f063199a8275c2f49c70140e8732
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588778"
---
# <a name="get-face-detection-data"></a>Obtener datos de detección de caras

Esta guía demuestra cómo utilizar la detección de caras para extraer los atributos como sexo, edad o postura de una imagen determinada. Los fragmentos de código en esta guía se escriben C# utilizando la biblioteca de cliente de Face API, pero la misma funcionalidad está disponible a través de la [API de REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Esta guía le mostrará cómo para:

- Obtener las ubicaciones y las dimensiones de caras en una imagen.
- Obtener las ubicaciones de diversos faciales (alumnos, nariz, boca etc.) en una imagen.
- Adivinar el sexo, edad y emociones y otros atributos de un rostro detectado.

## <a name="setup"></a>Configuración

Esta guía se da por supuesto que ya ha construido una **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** objeto, denominado `faceClient`, con una URL de clave y el punto de conexión de suscripción de cara. Desde aquí, puede usar la característica de detección de caras llamando **[DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet)** (utilizado en esta guía) o **[DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet)**. Consulte la [detectar caras de inicio rápido para C# ](../quickstarts/csharp-detect-sdk.md) para obtener instrucciones sobre cómo configurar esta opción.

Esta guía se centra en los detalles de la llamada de detectar&mdash;argumentos que se pueden pasar y lo que puede hacer con los datos devueltos. Se recomienda consultar sólo las características que necesita, como cada operación tarda más tiempo en completarse.

## <a name="get-basic-face-data"></a>Obtener datos básicos de cara

Para encontrar caras y obtener sus ubicaciones en una imagen, llame al método con el _returnFaceId_ parámetro establecido en **true** (valor predeterminado).

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

El valor devuelto **[DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)** objetos se pueden consultar para sus identificadores únicos y un rectángulo que proporciona las coordenadas de píxel de la cara.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Consulte **[FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet)** para obtener información sobre cómo analizar la ubicación y las dimensiones de la cara. Normalmente, este rectángulo contiene los ojos, arqueadas, nariz y boca; la parte superior de la cabeza, oídos y chin no se incluyen necesariamente. Si piensa utilizar el rectángulo facial para recortar un encabezado completo o vertical del medio de captura (una imagen de tipo identificador de foto), puede expandir el rectángulo en un determinado margen en cada dirección.

## <a name="get-face-landmarks"></a>Obtener faciales

Los puntos de referencia son un conjunto de puntos de encontrar en una cara, como los alumnos o la sugerencia de punta. Puede obtener datos de punto de referencia de cara al establecer el _returnFaceLandmarks_ parámetro **true**.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

De forma predeterminada, existen 27 puntos de referencia predefinidos. La siguiente ilustración muestra todos los puntos de 27:

![Un diagrama de cara a todos los 27 puntos de referencia etiquetadas como](../Images/landmarks.1.jpg)

Los puntos que se devuelven están en unidades de píxeles, al igual que el marco del rectángulo de cara. El código siguiente muestra cómo se pueden recuperar las ubicaciones de la nariz y alumnos:

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

Datos de puntos de referencia de cara también pueden usarse para calcular con precisión la dirección de la cara. Por ejemplo, podemos definimos la rotación de la cara como un vector desde el centro de la boca hasta el centro de los ojos. El código siguiente calcula este vector:

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

Conocer la dirección de la cara, a continuación, puede girar el marco rectangular cara para alinearlo más correctamente. Si desea recortar caras en una imagen, mediante programación puede girar la imagen para que las caras siempre aparecen en verticales.

## <a name="get-face-attributes"></a>Obtener los atributos de cara

Además de los rectángulos y puntos de referencia, la API de detección de caras puede analizar varios atributos conceptuales de una cara. Entre ellas se incluyen las siguientes:

- Edad
- Sexo
- Intensidad de la sonrisa
- Vello facial
- Gafas
- Postura principal 3D
- Emotion

> [!IMPORTANT]
> Estos atributos se puede predecir mediante el uso de algoritmos estadísticos y puede que no siempre sea precisos. Tenga cuidado al tomar decisiones basadas en datos de atributo.
>

Para analizar los atributos de cara, establezca el _returnFaceAttributes_ parámetro a una lista de **[FaceAttributeType Enum](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** valores.

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

A continuación, obtener referencias a los datos devueltos y hacer aún más las operaciones de según sus necesidades.

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

Para obtener más información acerca de cada uno de los atributos, consulte el [glosario](../Glossary.md).

## <a name="next-steps"></a>Pasos siguientes

En esta guía ha aprendido cómo utilizar las diversas funcionalidades de detección de caras. A continuación, consulte el [glosario](../Glossary.md) para una vista más detallada en los datos de la cara que se han recuperado.

## <a name="related-topics"></a>Temas relacionados

- [Documentación de referencia (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Documentación de referencia (SDK de .NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/face?view=azure-dotnet)
