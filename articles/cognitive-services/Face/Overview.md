---
title: ¿Qué es el servicio Face API?
titleSuffix: Azure Cognitive Services
description: En este tema se explica el servicio Face API y los términos relacionados.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 10/11/2018
ms.author: sbowles
ms.openlocfilehash: 6c1e0d0a51bc01c581c05e7ce3215f5501b4be76
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310409"
---
# <a name="what-is-the-face-api-service"></a>¿Qué es el servicio Face API?

Face API es un servicio basado en la nube que proporciona algoritmos para analizar los rostros de las personas en imágenes y vídeo. Face API tiene dos funciones principales: detección de rostros con atributos y reconocimiento facial.

## <a name="face-detection"></a>Detección de caras

Face API detecta hasta 64 rostros de personas con una localización facial de alta precisión en las imágenes. La imagen puede especificarse por archivo (flujo de bytes) o con una dirección URL válida.

![Introducción sobre la detección de caras](./Images/Face.detection.jpg)

El rectángulo facial (con delimitación izquierda, derecha, de altura y anchura) que indica la ubicación de la cara en la imagen se devuelve junto con cada cara detectada. Si lo desea, la detección de caras extrae una serie de atributos faciales como la postura, el sexo, la edad, la posición de la cabeza, el vello facial y las gafas. Para más información, vea [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Reconocimiento facial

La capacidad de identificar rostros de personas es muy importante en muchos escenarios, como seguridad, interfaz de usuario natural, análisis y administración de contenido de imagen, aplicaciones móviles y robótica. El servicio Face API proporciona cuatro funciones de reconocimiento facial: comprobación de caras, búsqueda de caras similares, agrupación de caras e identificación de personas.

### <a name="face-verification"></a>Verificación de caras

La verificación de caras realiza la autenticación con dos caras detectadas o a partir de una cara detectada en un objeto personal. Para más información, vea [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-faces"></a>Búsqueda de caras similares

Con una cara objetivo detectada y una serie de caras candidatas con las que buscar, el servicio encuentra un conjunto pequeño de las caras que más se asemejan a la cara objetivo. Se admiten dos modos de funcionamiento, **matchFace** y **matchPerson**. El modo **matchPerson** devuelve caras similares después de aplicar un umbral de la misma persona derivado de [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). El modo **matchFace** ignora el umbral de la misma persona y devuelve las caras de los candidatos más similares. En el siguiente ejemplo se enumeran las caras de los candidatos.
![Información general: búsqueda de caras similares](./Images/FaceFindSimilar.Candidates.jpg) Esta es la cara de la consulta.
![Información general: búsqueda de caras similares](./Images/FaceFindSimilar.QueryFace.jpg)

Al buscar cuatro caras parecidas, el modo **matchPerson** devolvería (a) y (b), que representan la misma persona que la cara de la consulta. El modo **matchFace** devuelve (a), (b), (c) y (d), exactamente cuatro caras candidatas, aunque el nivel de similitud sea bajo. Para más información, vea [Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Agrupación de caras

Con un conjunto de caras desconocidas, la API de agrupación de caras las divide automáticamente en varios grupos en función de la similitud. Cada grupo es un subconjunto adecuado no relacionado de un conjunto original de caras desconocidas que contiene caras similares. Se puede considerar que todas las caras del mismo grupo pertenecen a la misma persona. Para más información, vea [Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="person-identification"></a>Identificación de personas

Se puede usar Face API para identificar a una persona a partir de una cara detectada y una base de datos de personas. Esta base de datos se crea de antemano y puede modificarse con el tiempo.

La siguiente imagen es un ejemplo de una base de datos denominada "myfriends". Cada grupo puede contener hasta 1 000 000/10 000 objetos de persona. Cada objeto de persona puede tener hasta 248 caras registradas.

![Introducción - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Una vez creada y entrenada la base de datos, se puede realizar la identificación en el grupo con una cara nueva detectada. Si la cara se identifica como una persona en el grupo, se devuelve el objeto de persona.

Para más información sobre la identificación de personas, vea las siguientes guías de API:

[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)  
[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)  
[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)  
[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)  
[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)  
[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)  
[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)  

#### <a name="face-storage-and-pricing"></a>Almacenamiento de caras y precios

El Almacenamiento de caras permite realizar una suscripción estándar para almacenar caras persistentes adicionales al usar los objetos de personas de LargePersonGroup/PersonGroup ([PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) o LargeFaceLists/FaceLists ([FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) para la identificación o la coincidencia de similitudes con Face API. Las imágenes almacenadas se cobran a 0,50 USD por 1000 caras y esta tarifa se prorratea por días. Las suscripciones del nivel Gratis están limitadas a un total de 1000 personas.

Por ejemplo, si la cuenta usó 10 000 caras almacenadas cada día durante la primera mitad del mes y ninguna la segunda mitad, se le cobraría solo por las 10 000 caras durante los días de almacenamiento. Como alternativa, si cada día del mes almacena 1000 caras durante unas horas y luego las elimina por la noche, se le facturarán 1000 caras almacenadas cada día.

## <a name="sample-apps"></a>Aplicaciones de ejemplo

Eche un vistazo a estas aplicaciones de ejemplo que usan Face API.

- [Microsoft Face API: Ejemplo y Biblioteca cliente de Windows](https://github.com/Microsoft/Cognitive-Face-Windows)
  - La aplicación de ejemplo WPF que demuestra varios escenarios de detección, análisis e identificación facial.
- [Aplicación FamilyNotes de UWP](https://github.com/Microsoft/Windows-appsample-familynotes)
  - La aplicación de ejemplo de la Plataforma universal de Windows (UWP) que muestra el uso de voz, Cortana, tinta y cámara mediante un escenario de uso compartido de notas familiares.
- [Ejemplo de análisis de fotogramas de vídeo](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Aplicación de ejemplo de Win32 que muestra cómo analizar secuencias de vídeo en directo casi en tiempo real con Face API, Computer Vision API y Emotion API.

## <a name="tutorials"></a>Tutoriales
En los siguientes tutoriales se describen los procesos de suscripción y las funcionalidades básicas de Face API:
- [Tutorial de introducción a Face API en CSharp](Tutorials/FaceAPIinCSharpTutorial.md)
- [Tutorial de introducción a Face API en Java para Android](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Tutorial de introducción a Face API en Python](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="next-steps"></a>Pasos siguientes

Pruebe una guía de inicio rápido para implementar un escenario sencillo de Face API.
- [Guía de inicio rápido: Detección de caras en una imagen con C#](quickstarts/csharp.md) (otros lenguajes disponibles)
