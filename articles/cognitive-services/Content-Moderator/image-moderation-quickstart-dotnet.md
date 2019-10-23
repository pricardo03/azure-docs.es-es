---
title: 'Inicio rápido: Comprobación de imágenes en busca de contenido inapropiado en C#: Content Moderator'
titleSuffix: Azure Cognitive Services
description: Cómo analizar el contenido de imagen de diverso material ofensivo mediante el SDK de Content Moderator para .NET
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: pafarley
ms.openlocfilehash: 3fdc3fa0b7c624558aef84f86afd85c5aedb7054
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72757319"
---
# <a name="quickstart-analyze-images-for-objectionable-content-in-c"></a>Inicio rápido: Análisis de imágenes en busca de contenido inapropiado en C#

En este artículo se proporciona información y ejemplos de código que le ayudarán a empezar a usar el [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/). Aprenderá a buscar contenido para adultos o subido de tono, texto extraíble y caras humanas con el fin de moderar material posiblemente ofensivo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="prerequisites"></a>Requisitos previos

- Una clave de suscripción de Content Moderator. Siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse a Content Moderator y obtener su clave.
- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).


> [!NOTE]
> Esta guía usa una suscripción de Content Moderator gratuita. Para información sobre lo que se proporciona con cada nivel de suscripción, consulte la página [Precios y límites](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/).

## <a name="create-the-visual-studio-project"></a>Creación del proyecto de Visual Studio

1. En Visual Studio, cree un nuevo proyecto de **Aplicación de consola (.NET Framework)** y asígnele el nombre **ImageModeration**. 
1. Si hay otros proyectos en la solución, seleccione este como proyecto de inicio único.
1. Obtenga los paquetes NuGet requeridos. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione **Administrar paquetes NuGet**; a continuación, busque e instale los siguientes paquetes:
    - Microsoft.Azure.CognitiveServices.ContentModerator
    - Microsoft.Rest.ClientRuntime
    - Newtonsoft.Json

## <a name="add-image-moderation-code"></a>Incorporación de código de moderación de imágenes

A continuación, copiará y pegará el código de esta guía en el proyecto para implementar un escenario básico de moderación de contenido.

### <a name="include-namespaces"></a>Inclusión de espacios de nombres

Agregue las siguientes instrucciones `using` al principio del archivo *Program.cs*.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=1-7)]

### <a name="create-the-content-moderator-client"></a>Creación del cliente de Content Moderator

Agregue el código siguiente al archivo *Program.cs* para crear un cliente de Content Moderator para su suscripción. Agregue el código junto con la clase **Program**, en el mismo espacio de nombres. Tendrá que actualizar los campos **AzureRegion** y **CMSubscriptionKey** con los valores de su identificador de región y clave de suscripción.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=83-106)]


### <a name="set-up-input-and-output-targets"></a>Configuración de los destinos de entrada y salida

Agregue los siguientes campos estáticos a la clase **Program** en _Program.cs_. Estos campos especifican los archivos para el contenido de imagen de entrada y el contenido JSON de salida.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=48-52)]

Tendrá que crear el archivo *ImageFiles.txt* de entrada y actualizar su ruta de acceso según corresponda (las rutas de acceso relativas se refieren al directorio de ejecución). Abra _ImageFiles.txt_ y agregue las direcciones URL de las imágenes que desea moderar. En esta guía de inicio rápido se usan las siguientes direcciones URL como entrada de ejemplo:

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="create-a-class-to-handle-results"></a>Creación de una clase para controlar los resultados

Agregue el código siguiente a *Program.cs*, a lo largo de la clase **Program**, en el mismo espacio de nombres. Se utilizará una instancia de esta clase para registrar los resultados de la moderación de las imágenes revisadas.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=108-123)]


### <a name="define-the-image-evaluation-method"></a>Definición del método de evaluación de la imagen

Agregue el método siguiente a la clase **Program**. Este método evalúa una sola imagen de tres modos distintos y devuelve los resultados de la evaluación. Si desea más información acerca de lo que hacen las operaciones individuales, siga el vínculo de la sección [Pasos siguientes](#next-steps).

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=54-80)]

### <a name="load-the-input-images"></a>Carga de las imágenes de entrada

Agregue el siguiente código al método **Main** de la clase **App**. Este código configura el programa para recuperar los datos de evaluación de cada dirección URL de imagen del archivo de entrada. A continuación, escribe estos datos en un único archivo de salida.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=16-45)]

## <a name="run-the-program"></a>Ejecución del programa

El programa escribirá los datos de cadena de JSON en el archivo _ModerationOutput.json_. Las imágenes de ejemplo utilizadas en esta guía de inicio rápido provocan la siguiente salida. Cada imagen tiene secciones diferentes para `ImageModeration`, `FaceDetection`, y `TextDetection`, que corresponden a las tres llamadas de API del método **EvaluateImage**.

```json
[{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
  "ImageModeration": {
    "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
    "adultClassificationScore": 0.019196987152099609,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.032390203326940536,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "116"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "12"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
    "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
    "language": "eng",
    "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
    "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
    "result": false,
    "count": 0,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "11"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "faces": []
  }
},
{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
  "ImageModeration": {
    "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
    "adultClassificationScore": 0.0035635426174849272,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.021369094029068947,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "109"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "46"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
    "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
    "language": "eng",
    "text": "",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
    "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
    "result": true,
    "count": 6,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "60"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "faces": [
      {
        "bottom": 598,
        "left": 44,
        "right": 268,
        "top": 374
      },
      {
        "bottom": 620,
        "left": 308,
        "right": 532,
        "top": 396
      },
      {
        "bottom": 575,
        "left": 594,
        "right": 773,
        "top": 396
      },
      {
        "bottom": 563,
        "left": 812,
        "right": 955,
        "top": 420
      },
      {
        "bottom": 611,
        "left": 972,
        "right": 1151,
        "top": 432
      },
      {
        "bottom": 510,
        "left": 1232,
        "right": 1456,
        "top": 286
      }
    ]
  }
}]
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha desarrollado una aplicación .NET simple que usa el servicio Content Moderator para devolver información pertinente sobre un ejemplo de imagen dado. A continuación, conozca lo que significan las distintas marcas y las clasificaciones para que pueda decidir qué datos necesita y el modo en que la aplicación debe controlarlos.

> [!div class="nextstepaction"]
> [Guía para la moderación de imágenes](image-moderation-api.md)
