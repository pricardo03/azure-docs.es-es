---
title: Introducción al servicio Face API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: En el glosario se explican los términos que puede encontrar cuando use el servicio Face API.
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: fb1d14ff80bf53adc3008d79cc998739ffffde1b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048676"
---
# <a name="what-is-face-api"></a>¿Qué es Face API?

Bienvenido a Microsoft Face API, un servicio basado en la nube que proporciona los algoritmos faciales más avanzados. Face API tiene dos funciones principales: detección de caras con atributos y reconocimiento facial.

## <a name="face-detection"></a>Detección de caras

Face API detecta hasta 64 caras humanas con una localización facial de alta precisión en una imagen. La imagen se puede especificar por archivo en bytes o mediante una dirección URL válida.

![Introducción sobre la detección de caras](./Images/Face.detection.jpg)

El rectángulo facial (con delimitación izquierda, derecha, de altura y anchura) que indica la ubicación de la cara en la imagen se devuelve junto con cada cara detectada. Si lo desea, la detección de caras extrae una serie de atributos faciales como la postura, el sexo, la edad, la posición de la cabeza, el vello facial y las gafas. Para más información, vea [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Reconocimiento facial

El reconocimiento facial se usa ampliamente en muchos escenarios, como seguridad, interfaz de usuario natural, análisis y administración de contenido de imagen, aplicaciones móviles y robótica. Se ofrecen cuatro funciones de reconocimiento facial: comprobación de caras, búsqueda de caras similares, agrupación facial e identificación de la persona.

### <a name="face-verification"></a>Comprobación de caras

La comprobación de Face API realiza una autenticación de dos caras detectadas o la autenticación a partir de una cara detectada con un objeto personal. Para más información, vea [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-face"></a>Búsqueda de caras similares

Con una cara objetivo detectada y una serie de caras candidatas con las que buscar, el servicio encuentra un conjunto pequeño de las caras que más se asemejan a la cara objetivo. Se admiten los dos modos de trabajo `matchFace` y `matchPerson`. El modo `matchPerson` devuelve caras similares después de aplicar un umbral de la misma persona derivado de [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). El modo `matchFace` ignora el umbral de la misma persona y devuelve las caras de candidatos más similares. En el siguiente ejemplo se enumeran las caras de candidatos.
![Introducción - Encontrar cara similar](./Images/FaceFindSimilar.Candidates.jpg) Y una cara consultada es ![Introducción - Encontrar cara similar](./Images/FaceFindSimilar.QueryFace.jpg)

Para buscar cuatro caras similares, el modo `matchPerson` devuelve a) y b), que pertenecen a la misma persona con la cara consultada. El modo `matchFace` devuelve a), b), c) y d), exactamente cuatro caras candidatas, aunque el nivel de similitud sea bajo. Para más información, vea [Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Agrupación de caras

Con un conjunto de caras desconocidas, la API de agrupación de caras las divide automáticamente en varios grupos en función de la similitud. Cada grupo es un subconjunto adecuado no relacionado de un conjunto original de caras desconocidas, que contiene caras similares. Además, se puede considerar que todas las caras del mismo grupo pertenecen al objeto de la misma persona. Para más información, vea [Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="face-identification"></a>Identificación de caras

Se puede usar Face API para identificar a una persona en función de una cara detectada y en una base de datos de personas (que se definen como LargePersonGroup/PersonGroup). Cree esta base de datos de antemano, que puede modificarse con el tiempo.

La siguiente imagen es un ejemplo de una instancia LargePersonGroup/PersonGroup denominada "myfriends". Cada grupo puede contener hasta 1 000 000/10 000 objetos de personas. Mientras tanto, cada objeto de persona puede tener hasta 248 caras registradas.

![Introducción - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Después de crear y entrenar una instancia de LargePersonGroup/PersonGroup, se puede realizar la identificación del grupo y de una cara nueva detectada. Si la cara se identifica como un objeto de persona en el grupo, se devuelve el objeto de persona.

Para más información sobre la identificación de personas, vea las siguientes guías de API:

[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)
[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)
[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)
[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)
[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)

### <a name="face-storage"></a>Almacenamiento de caras

El Almacenamiento de caras permite realizar una suscripción estándar para almacenar caras persistentes adicionales al usar los objetos de personas de LargePersonGroup/PersonGroup ([PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) o LargeFaceLists/FaceLists ([FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) para la identificación o la coincidencia de similitudes con Face API. Las imágenes almacenadas se cobran a 0,5 USD por 1000 caras y esta tarifa se prorratea por días. Las suscripciones del nivel Gratis son gratuitas, pero están limitadas a un total de 1000 personas.

El precio del Almacenamiento de caras se prorratea por días. Por ejemplo, si la cuenta usó 10 000 caras almacenadas cada día durante la primera mitad del mes y ninguna la segunda mitad, se le cobraría solo por las 10 000 caras durante los días de almacenamiento. Como alternativa, si cada día del mes almacena 1000 caras durante unas horas y luego las elimina por la noche, se le facturarán 1000 caras almacenadas cada día.

## <a name="getting-started-tutorials"></a>Tutoriales de introducción

En los siguientes tutoriales se describen los procesos de suscripción y las funcionalidades básicas de Face API:

- [Tutorial de introducción a Face API en CSharp](Tutorials/FaceAPIinCSharpTutorial.md)
- [Tutorial de introducción a Face API en Java para Android](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Tutorial de introducción a Face API en Python](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="sample-apps"></a>Aplicaciones de ejemplo

Eche un vistazo a estas aplicaciones de ejemplo que usan Face API.

- [Microsoft Face API: Ejemplo y Biblioteca cliente de Windows](https://github.com/Microsoft/Cognitive-Face-Windows)
  - La aplicación de ejemplo WPF que demuestra varios escenarios de detección, análisis e identificación facial.
- [Aplicación FamilyNotes de UWP](https://github.com/Microsoft/Windows-appsample-familynotes)
  - La aplicación de ejemplo de la Plataforma universal de Windows (UWP) que muestra el uso de voz, Cortana, tinta y cámara mediante un escenario de uso compartido de notas familiares.
- [Ejemplo de análisis de fotogramas de vídeo](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Aplicación de ejemplo de la Plataforma universal de Windows (UWP) que muestra cómo analizar secuencias de vídeo en directo casi en tiempo real con Face API, Computer Vision API y Emotion API.

## <a name="related-topics"></a>Temas relacionados

- [Notas de la versión Face API 1.0](ReleaseNotes.md)
- [Detección de caras en imágenes](Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
- [Identificación de caras en imágenes](Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)
