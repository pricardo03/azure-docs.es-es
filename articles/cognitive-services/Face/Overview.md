---
title: ¿Qué es Face API?
titleSuffix: Azure Cognitive Services
description: Aprenda a usar el servicio Face para detectar y analizar las caras en imágenes.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 10/29/2018
ms.author: sbowles
ms.openlocfilehash: 361d80bcca0b0a151e9fcdaab938de0c8ff5f8bc
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220117"
---
# <a name="what-is-the-azure-face-api"></a>¿Qué es Azure Face API?

Azure Face API es un servicio de Cognitive Services que proporciona algoritmos para detectar, reconocer y analizar las caras humanas en imágenes. La capacidad de procesar rostros de personas es muy importante en muchos escenarios de software diferentes, por ejemplo, en la seguridad, la interfaz de usuario natural, el análisis y la administración de contenido de las imágenes, las aplicaciones móviles y la robótica.

Face API proporciona varias funciones distintas, que se describen en las secciones siguientes. Siga leyendo para obtener más información sobre cada una y para determinar si se adaptan a sus necesidades.

## <a name="face-detection"></a>Detección de caras

Face API puede detectar caras humanas en una imagen y devolver las coordenadas del rectángulo de sus ubicaciones. Si lo desea, la detección de caras puede extraer una serie de atributos faciales como la postura, el sexo, la edad, la posición de la cabeza, el vello facial y las gafas.

![Una imagen de una mujer y un hombre, con rectángulos dibujados en torno a sus caras y edad y sexo](./Images/Face.detection.jpg)

La característica de detección facial también está disponible mediante [Computer Vision API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), pero, si desea realizar más operaciones con datos de caras, debe usar Face API (este servicio). Para más información acerca de la detección facial, consulte [Detect API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-verification"></a>Verificación de caras

Verify API realiza una autenticación con dos caras detectadas o a partir de una cara detectada en un objeto personal. En la práctica, evalúa si dos caras pertenecen a la misma persona. Esto puede ser muy útil en escenarios de seguridad. Para más información, consulte [Verify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="find-similar-faces"></a>Búsqueda de caras similares

Find Similar API toma una cara objetivo y una serie de caras candidatas y busca un conjunto pequeño de las caras que más se asemejan a la cara objetivo. Se admiten dos modos de funcionamiento: **matchPerson** y **matchFace**. El modo **matchPerson** devuelve las caras parecidas tras filtrar a la misma persona (mediante [Verify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)). El modo **matchFace** omite el filtro de la misma persona y devuelve una lista de caras similares candidatas que pueden o no pertenecer a la misma persona.

En el ejemplo siguiente, esta es la cara de destino:

![Una mujer sonriendo](./Images/FaceFindSimilar.QueryFace.jpg)

Y estas son las caras candidatas:

![Cinco imágenes de personas sonriendo. Las imágenes (a) y (b) son de la misma persona](./Images/FaceFindSimilar.Candidates.jpg)

Al buscar cuatro caras parecidas, el modo **matchPerson** devolvería (a) y (b), que representan a la misma persona que la cara objetivo. El modo **matchFace** devuelve (a), (b), (c) y (d)&mdash;, exactamente cuatro caras candidatas, aunque algunas no sean la misma persona que el objetivo o el nivel de similitud sea bajo. Para más información, consulte [Find Similar API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

## <a name="face-grouping"></a>Agrupación de caras

Group API divide un conjunto de caras desconocidas en varios grupos en función de la similitud. Cada grupo es un subconjunto apropiado separado del conjunto original de caras. Todas las caras de un grupo pertenecerán probablemente a la misma persona, pero puede haber varios grupos diferentes para una sola persona (que se diferencien por otro factor, por ejemplo, por la expresión). Para más información, consulte [Group API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="person-identification"></a>Identificación de personas

Identify API puede usarse para identificar una cara detectada en una base de datos de personas. Esto puede resultar de utilidad para el etiquetado automático de imágenes en software de administración de fotografías. Esta base de datos se crea de antemano y puede modificarse con el tiempo.

La siguiente imagen es un ejemplo de una base de datos denominada "myfriends". Cada grupo puede contener hasta 1 000 000 de objetos persona distintos y cada objeto persona puede tener hasta 248 caras registradas.

![Una cuadrícula con tres columnas para diferentes personas, cada una con tres filas de imágenes de caras](./Images/person.group.clare.jpg)

Una vez creada y entrenada la base de datos, puede realizar la identificación en el grupo con una cara nueva detectada. Si la cara se identifica como una persona en el grupo, se devuelve el objeto de persona.

Para más información acerca de la identificación de personas, consulte [Identify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="use-containers"></a>Uso de contenedores

[Utilice el contenedor de caras](face-how-to-install-containers.md) para detectar, reconocer e identificar caras, mediante la instalación de un contenedor de Docker estándar más cercano a los datos.

## <a name="sample-apps"></a>Aplicaciones de ejemplo

Las aplicaciones de ejemplo siguientes muestran algunas de las maneras en que se puede usar Face API.

- [Microsoft Face API: biblioteca de cliente Windows y ejemplo](https://github.com/Microsoft/Cognitive-Face-Windows): una aplicación de WPF que demuestra varios escenarios de detección, análisis e identificación de caras.
- [Aplicación FamilyNotes UWP](https://github.com/Microsoft/Windows-appsample-familynotes): una aplicación de Plataforma universal de Windows (UWP) que usa la identificación facial junto con la voz, Cortana, la escritura manuscrita y la cámara en un escenario de uso compartido de notas en familia.

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de los datos

Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Face deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/en-us/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Siga una guía de inicio rápido para implementar un escenario sencillo de identificación facial en el código.
- [Inicio rápido: Detección de caras en una imagen con .NET SDK con C#](quickstarts/csharp.md) (otros lenguajes disponibles)
