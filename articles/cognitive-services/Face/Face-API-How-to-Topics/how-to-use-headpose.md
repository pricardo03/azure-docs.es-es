---
title: Utilice HeadPose para ajustar el rectángulo facial
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar el atributo HeadPose para girar automáticamente el rectángulo de cara.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: ddc5bc522c0d3ac258581f2a48a5c3b755302f01
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576507"
---
# <a name="use-the-headpose-attribute-to-adjust-the-face-rectangle"></a>Utilice el atributo HeadPose para ajustar el rectángulo facial

En esta guía, usará un atributo rostro detectado, HeadPose, para girar el rectángulo de un objeto de cara. El ejemplo de código en esta guía, desde el [WPF de Cognitive Services Face](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) aplicación de ejemplo, usa el SDK. NET.

El rectángulo facial, devuelven con cada rostro detectado, marca la ubicación y tamaño de la cara de la imagen. De forma predeterminada, el rectángulo siempre está alineado con la imagen (sus lados son perfectamente vertical y horizontal). Esto puede ser ineficaz de caras en ángulo de tramas. En situaciones donde desea recortar mediante programación las caras de una imagen, es conveniente ser capaz de girar el rectángulo de recorte.

## <a name="explore-the-sample-code"></a>Exploración del código de ejemplo

Mediante programación, puede girar el rectángulo facial mediante el atributo HeadPose. Si especifica este atributo cuando se detecta caras (consulte [cómo detectar caras](HowtoDetectFacesinImage.md)), podrán realizar consultas en ella más adelante. El método siguiente desde el [WPF de Cognitive Services Face](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) aplicación toma una lista de **DetectedFace** objetos y devuelve una lista de **[cara](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/Face.cs)** objetos. **Cara** aquí es una clase personalizada que enfrentan a los almacenes de datos, incluidas las coordenadas del rectángulo actualizada. Los nuevos valores se calculan para **superior**, **izquierdo**, **ancho**, y **alto**y un nuevo campo **FaceAngle**especifica el giro.

```csharp
/// <summary>
/// Calculate the rendering face rectangle
/// </summary>
/// <param name="faces">Detected face from service</param>
/// <param name="maxSize">Image rendering size</param>
/// <param name="imageInfo">Image width and height</param>
/// <returns>Face structure for rendering</returns>
public static IEnumerable<Face> CalculateFaceRectangleForRendering(IList<DetectedFace> faces, int maxSize, Tuple<int, int> imageInfo)
{
    var imageWidth = imageInfo.Item1;
    var imageHeight = imageInfo.Item2;
    var ratio = (float)imageWidth / imageHeight;
    int uiWidth = 0;
    int uiHeight = 0;
    if (ratio > 1.0)
    {
        uiWidth = maxSize;
        uiHeight = (int)(maxSize / ratio);
    }
    else
    {
        uiHeight = maxSize;
        uiWidth = (int)(ratio * uiHeight);
    }

    var uiXOffset = (maxSize - uiWidth) / 2;
    var uiYOffset = (maxSize - uiHeight) / 2;
    var scale = (float)uiWidth / imageWidth;

    foreach (var face in faces)
    {
        var left = (int)(face.FaceRectangle.Left * scale + uiXOffset);
        var top = (int)(face.FaceRectangle.Top * scale + uiYOffset);

        // Angle of face rectangles, default value is 0 (not rotated).
        double faceAngle = 0;

        // If head pose attributes have been obtained, re-calculate the left & top (X & Y) positions.
        if (face.FaceAttributes?.HeadPose != null)
        {
            // Head pose's roll value acts directly as the face angle.
            faceAngle = face.FaceAttributes.HeadPose.Roll;
            var angleToPi = Math.Abs((faceAngle / 180) * Math.PI);

            // _____       | / \ |
            // |____|  =>  |/   /|
            //             | \ / |
            // Re-calculate the face rectangle's left & top (X & Y) positions.
            var newLeft = face.FaceRectangle.Left +
                face.FaceRectangle.Width / 2 -
                (face.FaceRectangle.Width * Math.Sin(angleToPi) + face.FaceRectangle.Height * Math.Cos(angleToPi)) / 2;

            var newTop = face.FaceRectangle.Top +
                face.FaceRectangle.Height / 2 -
                (face.FaceRectangle.Height * Math.Sin(angleToPi) + face.FaceRectangle.Width * Math.Cos(angleToPi)) / 2;

            left = (int)(newLeft * scale + uiXOffset);
            top = (int)(newTop * scale + uiYOffset);
        }

        yield return new Face()
        {
            FaceId = face.FaceId?.ToString(),
            Left = left,
            Top = top,
            OriginalLeft = (int)(face.FaceRectangle.Left * scale + uiXOffset),
            OriginalTop = (int)(face.FaceRectangle.Top * scale + uiYOffset),
            Height = (int)(face.FaceRectangle.Height * scale),
            Width = (int)(face.FaceRectangle.Width * scale),
            FaceAngle = faceAngle,
        };
    }
}
```

## <a name="display-the-updated-rectangle"></a>Mostrar el rectángulo actualizado

Desde aquí, puede usar el valor devuelto **cara** objetos en la pantalla. Las siguientes líneas de [FaceDetectionPage.xaml](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/FaceDetectionPage.xaml) mostrar cómo se representa el nuevo rectángulo desde estos datos:

```xaml
 <DataTemplate>
    <Rectangle Width="{Binding Width}" Height="{Binding Height}" Stroke="#FF26B8F4" StrokeThickness="1">
        <Rectangle.LayoutTransform>
            <RotateTransform Angle="{Binding FaceAngle}"/>
        </Rectangle.LayoutTransform>
    </Rectangle>
</DataTemplate>
```

## <a name="next-steps"></a>Pasos siguientes

Consulte la [WPF de Cognitive Services Face](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) app en GitHub para obtener un ejemplo de rectángulos faciales girado. O bien, consulte el [Face API HeadPose ejemplo](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) app, que realiza un seguimiento el atributo HeadPose en tiempo real para detectar varios movimientos principales (nodding, agita).