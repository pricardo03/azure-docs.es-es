---
title: 'Guía de inicio rápido: Detección de caras en una imagen con el SDK de Azure Face para .NET'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, usará el SDK de Azure Face con C# para detectar caras en una imagen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 11/07/2018
ms.author: pafarley
ms.openlocfilehash: 4fbbde167a8c895a71ab3614e8c3ecbce26604a9
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578168"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-net-sdk"></a>Guía de inicio rápido: Detección de caras en una imagen con el SDK de Face para .NET

En esta guía de inicio rápido, usará el SDK del servicio Face con C# para detectar caras humanas en una imagen. Para obtener un ejemplo en funcionamiento del código de esta guía de inicio rápido, consulte el proyecto de Face en el repositorio [Guías de inicio rápido de Cognitive Services Vision para C#](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face) en GitHub.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="prerequisites"></a>Requisitos previos

- Una clave de suscripción de Face API. Puede obtener una clave de la suscripción de evaluación gratuita en la página [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). O bien, siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse al servicio Face API y obtener la clave.
- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Creación del proyecto de Visual Studio

1. En Visual Studio, cree un nuevo proyecto de **Aplicación de consola (.NET Framework)** y asígnele el nombre **FaceDetection**. 
1. Si hay otros proyectos en la solución, seleccione este como proyecto de inicio único.
1. Obtenga los paquetes NuGet requeridos. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes de NuGet**. Haga clic en la pestaña **Examinar** y seleccione **Incluir versión preliminar**; a continuación, busque e instale el paquete siguiente:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="add-face-detection-code"></a>Adición del código de detección de caras

Abra el archivo *Program.cs* del nuevo proyecto. Aquí, agregará el código necesario para cargar imágenes y detectar caras.

### <a name="include-namespaces"></a>Inclusión de espacios de nombres

Agregue las siguientes instrucciones `using` al principio del archivo *Program.cs*.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=1-7)]

### <a name="add-essential-fields"></a>Adición de los campos esenciales

Agregue los campos siguientes a la clase **Program** . Estos datos especifican cómo conectarse al servicio Face y dónde obtener los datos de entrada. Deberá actualizar el campo `subscriptionKey` con el valor de la clave de suscripción y es posible que deba cambiar la cadena `faceEndpoint` para que contenga el identificador de la región correcta. También deberá establecer los valores de `localImagePath` y `remoteImageUrl` en rutas de acceso que apunten a archivos de imagen reales.

El campo `faceAttributes` es simplemente una matriz de determinados tipos de atributos. Especificará qué información recuperar sobre las caras detectadas.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=13-34)]

### <a name="create-and-use-the-face-client"></a>Creación y uso del cliente de Face

A continuación, agregue el siguiente código al método **Main** de la clase **Program**. Esta acción configura un cliente de Face API.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=38-41)]

Además, en el método **Main**, agregue el código siguiente para usar el cliente de Face recién creado para detectar caras en una imagen local y remota. A continuación se definirán los métodos de detección. 

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=43-49)]

### <a name="detect-faces"></a>Detección de caras

Agregue el método siguiente a la clase **Program**. Usa el cliente del servicio Face para detectar caras en una imagen remota, a las que se hace referencia mediante una dirección URL. Observe que usa el campo `faceAttributes`; los objetos **DetectedFace** agregados a `faceList` tendrán los atributos especificados (en este caso, edad y sexo).

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=52-74)]

De forma similar, agregue el método **DetectLocalAsync**. Usa el cliente del servicio Face para detectar caras en una imagen remota, a las que se hace referencia mediante una ruta de acceso de archivo.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=76-101)]

### <a name="retrieve-and-display-face-attributes"></a>Recuperación y visualización de los atributos de la cara

A continuación, defina el método **GetFaceAttributes**. Devuelve una cadena con la información de atributos pertinente.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=103-116)]

Por último, defina el método **DisplayAttributes** para escribir los datos de los atributos de cara en la salida de consola.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=118-123)]

## <a name="run-the-app"></a>Ejecución de la aplicación

Una respuesta correcta muestra el sexo y la edad de cada cara de la imagen. Por ejemplo: 

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado una sencilla aplicación de consola de .NET que puede usar el servicio Face API para detectar caras en imágenes locales y remotas. A continuación, puede seguir un tutorial más detallado para ver cómo puede presentar la información de las caras al usuario de manera intuitiva.

> [!div class="nextstepaction"]
> [Tutorial: Creación de una aplicación WPF para detectar y analizar las caras de una imagen](../Tutorials/FaceAPIinCSharpTutorial.md)
