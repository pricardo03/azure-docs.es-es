---
title: 'Ejemplo: Detección de caras en imágenes (Face API)'
titleSuffix: Azure Cognitive Services
description: Use Face API para detectar caras en imágenes.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 8c89a43910a5e98286a82de8626870d3aec55b94
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214218"
---
# <a name="example-how-to-detect-faces-in-image"></a>Ejemplo: Detección de caras en imágenes

En esta guía se mostrará cómo detectar caras en una imagen, con atributos de cara extraídos como el sexo, la edad o la posición. Los ejemplos están escritos en C# con la biblioteca cliente de Face API. 

## <a name="concepts"></a>Conceptos

Si no está familiarizado con alguno de los siguientes conceptos de esta guía, consulte las definiciones en nuestro [Glosario](../Glossary.md) en cualquier momento: 

- Detección de caras
- Puntos de referencia de cara
- Posición de la cabeza
- Atributos de la cara

## <a name="preparation"></a>Preparación

En este ejemplo, se mostrarán las siguientes funciones: 

- Detectar caras en una imagen y marcarlas usando marcos rectangulares
- Analizar las ubicaciones de las pupilas, la nariz o la boca y marcarlas en la imagen
- Analizar la posición de la cabeza, el sexo y la edad de la cara

Para ejecutar estas funciones, deberá preparar una imagen con al menos una cara clara. 

## <a name="step-1-authorize-the-api-call"></a>Paso 1: Autorización de la llamada a la API

Cada llamada a Face API requiere una clave de suscripción. Esta clave se debe pasar a través de un parámetro de cadena de consulta o se debe especificar en el encabezado de la solicitud. Para pasar la clave de suscripción a través de una cadena de consulta, haga referencia a la dirección URL de la solicitud para [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como ejemplo:

```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

Como alternativa, también puede especificarse la clave de suscripción en el encabezado de la solicitud HTTP: **ocp-apim-subscription-key: &lt;Clave de suscripción&gt;** Al usar una biblioteca cliente, la clave de suscripción se pasa por el constructor de la clase FaceServiceClient. Por ejemplo: 
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step-2-upload-an-image-to-the-service-and-execute-face-detection"></a>Paso 2: Cargar una imagen en el servicio y ejecutar la detección de caras

La manera más sencilla de realizar la detección de caras es cargar una imagen directamente. Para esto, se envía una solicitud "POST" con el tipo de contenido application/octet-stream, con los datos leídos de una imagen JPEG. El tamaño máximo de la imagen es 4 MB.

Con la biblioteca cliente, la detección de caras mediante la carga se realiza pasando un objeto de secuencia. Observe el ejemplo siguiente:

```CSharp
using (Stream s = File.OpenRead(@"D:\MyPictures\image1.jpg"))
{
    var faces = await faceServiceClient.DetectAsync(s, true, true);
 
    foreach (var face in faces)
    {
        var rect = face.FaceRectangle;
        var landmarks = face.FaceLandmarks;
    }
}
```

Tenga en cuenta que el método DetectAsync de FaceServiceClient es asincrónico. El método que realiza la llamada debe estar marcado como asincrónico también, para poder usar la cláusula await.
Si la imagen ya está en la Web y tiene una dirección URL, se puede ejecutar la detección de caras proporcionando también la dirección URL. En este ejemplo, el cuerpo de la solicitud será una cadena JSON, que contenga la dirección URL.
Con la biblioteca cliente, la detección de caras mediante una dirección URL se puede ejecutar fácilmente utilizando otra sobrecarga del método DetectAsync.

```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

La propiedad FaceRectangle que se devuelve con las caras detectadas es básicamente ubicaciones de la cara en píxeles. Por lo general, este rectángulo contiene los ojos, las cejas, la nariz y la boca: la parte superior de la cabeza, las orejas y la barbilla no se incluyen. Si recorta un retrato de medio cuerpo o de cabeza completa (una foto de imagen de tipo de carné de identidad), puede que quiera expandir el área del marco rectangular de la cara, ya que el área de la cara puede ser demasiado pequeña para algunas aplicaciones. Para localizar una cara con más precisión, resultan útiles los puntos de referencia de cara (mecanismos para localizar las características de la cara o la dirección de la cara) descritos en la siguiente sección.

## <a name="step-3-understanding-and-using-face-landmarks"></a>Paso 3: Saber qué son los puntos de referencia de la cara y usarlos

Los puntos de referencia de cara son una serie de puntos detallados en una cara; normalmente puntos de componentes de la cara como las pupilas, las comisuras o la nariz. Los puntos de referencia de cara son atributos opcionales que se pueden analizar durante la detección de caras. Puede pasar "true" como valor booleano para el parámetro de consulta returnFaceLandmarks al llamar a [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) o usar el parámetro opcional returnFaceLandmarks para el método DetectAsync de la clase FaceServiceClient a fin de incluir los puntos de referencia de cara en los resultados de la detección.

De forma predeterminada, existen 27 puntos de referencia predefinidos. En la siguiente figura se muestra cómo se definen los 27 puntos:

![HowToDetectFace](../Images/landmarks.1.jpg)

Los puntos que se devuelven están en unidades de píxeles, igual que el marco rectangular de la cara. Por lo tanto, facilita marcar puntos de interés específicos en la imagen. El código siguiente muestra la recuperación de las ubicaciones de la nariz y las pupilas:

```CSharp
var faces = await faceServiceClient.DetectAsync(imageUrl, returnFaceLandmarks:true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
 
    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;
 
    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;
 
    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
``` 

Además de marcar las características de la cara en una imagen, los puntos de referencia de cara también pueden utilizarse para calcular con precisión la dirección de la cara. Por ejemplo, se puede definir la dirección de la cara como un vector desde el centro de la boca hasta el centro de los ojos. El código siguiente explica esto con detalle:

```CSharp
var landmarks = face.FaceLandmarks;
 
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

Si se conoce la dirección de la cara, se puede girar el marco rectangular de la cara para alinearlo con la cara. Está claro que la utilización de puntos de referencia de cara puede proporcionar más detalles y utilidad.

## <a name="step-4-using-other-face-attributes"></a>Paso 4: Usar otros atributos de la cara

Además de los puntos de referencia de cara, la API Face - Detect también puede analizar otros atributos de una cara. Estos atributos son los siguientes:

- Edad
- Sexo
- Intensidad de la sonrisa
- Vello facial
- Posición de la cabeza 3D

Estos atributos se predicen mediante algoritmos estadísticos y no siempre son precisos al 100 %. Sin embargo, siguen siendo útiles cuando se quieren clasificar caras por estos atributos. Para obtener más información sobre cada uno de los atributos, consulte el [Glosario](../Glossary.md).

A continuación, se muestra un ejemplo sencillo de extracción de atributos de cara durante la detección de caras:

```CSharp
var requiredFaceAttributes = new FaceAttributeType[] {
                FaceAttributeType.Age,
                FaceAttributeType.Gender,
                FaceAttributeType.Smile,
                FaceAttributeType.FacialHair,
                FaceAttributeType.HeadPose,
                FaceAttributeType.Glasses
            };
var faces = await faceServiceClient.DetectAsync(imageUrl,
    returnFaceLandmarks: true,
    returnFaceAttributes: requiredFaceAttributes);

foreach (var face in faces)
{
    var id = face.FaceId;
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
}
``` 

## <a name="summary"></a>Resumen

En esta guía ha aprendido las funcionalidades de la API [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), cómo puede detectar caras para imágenes locales cargadas o direcciones URL en la web; cómo puede detectar caras mediante la devolución de marcos rectangulares de caras, cómo puede analizar puntos de referencia de cara, posiciones de la cabeza 3D, así como otros atributos de la cara.

Para más información sobre los detalles de la API, vea la guía de referencia de la API [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related-topics"></a>Temas relacionados

[Identificación de caras en imágenes](HowtoIdentifyFacesinImage.md)
