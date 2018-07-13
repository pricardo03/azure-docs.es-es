---
title: Tutorial de C# de Computer Vision API | Microsoft Docs
description: Explore una aplicación básica de Windows que usa Computer Vision API de Microsoft Cognitive Services. Realice OCR, cree vistas en miniatura y trabaje con características visuales en una imagen.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/22/2017
ms.author: kefre
ms.openlocfilehash: f2aeb1734f8858ed8b80e5cdf48ef7e558703919
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381019"
---
# <a name="computer-vision-api-c35-tutorial"></a>Tutorial de C# de Computer Vision API

Explore una aplicación básica de Windows que usa Computer Vision API para realizar el reconocimiento óptico de caracteres (OCR), crear miniaturas con recorte inteligente, y detectar, clasificar, etiquetar y describir características visuales, como caras, en una imagen. El ejemplo siguiente le permite enviar una dirección URL de imagen o un archivo almacenado localmente. Puede usar este ejemplo de código abierto como plantilla para crear su propia aplicación para Windows con Vision API y WPF (Windows Presentation Foundation), una parte de .NET Framework.

### <a name="prerequisites"></a>requisitos previos

#### <a name="platform-requirements"></a>Requisitos de la plataforma

El ejemplo siguiente se ha desarrollado para .NET Framework mediante [Visual Studio 2015, Community Edition](https://www.visualstudio.com/downloads/).

#### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Suscripción a Computer Vision API y obtención de una clave de suscripción 

Antes de crear el ejemplo, debe suscribirse a Computer Vision API que forma parte de Microsoft Cognitive Services (anteriormente Project Oxford). Para información detallada sobre la administración de claves y suscripciones, consulte [Suscripciones](https://azure.microsoft.com/try/cognitive-services/). En este tutorial, puede usarse tanto la clave principal como secundaria. 

> [!NOTE]
> El tutorial está diseñado para usar claves de suscripción en la región **westcentralus**. Las claves de suscripción generadas en la versión de evaluación gratuita de Computer Vision usan la región **westcentralus**, así que funcionan correctamente. Si generó las claves de suscripción con la cuenta de Azure mediante [https://azure.microsoft.com/](https://azure.microsoft.com/), debe especificar la región **westcentralus**. Las claves generadas fuera de la región **westcentralus** no funcionarán.

#### <a name="get-the-client-library-and-example"></a>Obtención de la biblioteca cliente y ejemplo

Puede clonar la biblioteca cliente y la aplicación de ejemplo de Computer Vision API en el equipo mediante el [SDK](https://www.github.com/microsoft/cognitive-vision-windows). No lo descargue como un archivo ZIP.

### <a name="Step1">Paso 1: Instalación del ejemplo</a>

En el escritorio de GitHub, abra Sample-WPF\VisionAPI-WPF-Samples.sln.

### <a name="Step2">Paso 2: Compilación del ejemplo</a>

* Presione Ctrl + Mayús + B, o haga clic en (Build) Compilar en el menú de la cinta de opciones y, a continuación, seleccione Build Solution (Compilar solución).

### <a name="Step3">Paso 3: Ejecución del ejemplo</a>

1. Una vez completada la compilación, presione **F5** o haga clic en **Start** (Iniciar) en el menú de la cinta de opciones para ejecutar el ejemplo.
2. Busque la ventana de interfaz de usuario de Computer Vision API con el cuadro de edición de texto que ponga "Paste your subscription key here to start" (Pegue la clave de suscripción aquí para comenzar).
Puede optar por conservar la clave de suscripción en su equipo portátil o de escritorio; para ello, haga clic en el botón "Save Key" (Guardar clave). Si quiere eliminar la clave de suscripción del sistema, haga clic en "Delete Key" (Eliminar clave) para quitarla del equipo portátil o de escritorio.

    ![Clave de suscripción de Vision](../Images/Vision_UI_Subscription.PNG)

3. En "Select Scenario" (Seleccionar escenario), haga clic para usar uno de los seis escenarios y luego siga las instrucciones en pantalla. Microsoft recibe las imágenes cargadas y puede usarlas para mejorar Computer Vision API y los servicios relacionados. Al enviar una imagen, confirma que ha seguido nuestro [Código de conducta del desarrollador](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).

    ![Análisis de la interfaz de imagen](../Images/Analyze_Image_Example.PNG)

4. Hay imágenes de ejemplo que se usan con esta aplicación de ejemplo. Estas imágenes las puede encontrar en el repositorio de Github de Windows de Face API, en la [carpeta de datos](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data). Tenga en cuenta que el uso de estas imágenes se realiza bajo licencia según el acuerdo [LICENSE-IMAGE](https://github.com/Microsoft/Cognitive-Face-Windows/blob/master/LICENSE-IMAGE.md).

### <a name="Review">Revisión y comprensión</a>

Ahora que tiene una aplicación en ejecución, vamos a revisar cómo se integra esta aplicación de ejemplo con la tecnología Cognitive Services. Esta integración permitirá que sea más fácil seguir construyendo sobre esta aplicación o desarrollar su propia aplicación mediante Microsoft Computer Vision API.

Esta aplicación de ejemplo hace uso de la biblioteca cliente de Computer Vision API, un contenedor de cliente de C# fino para Microsoft Computer Vision API. Si ha compilado la aplicación de ejemplo como se ha descrito anteriormente, habrá obtenido la biblioteca cliente del paquete NuGet. Puede revisar el código fuente de la biblioteca cliente en la carpeta "**Client Library**" (Biblioteca cliente) en **Vision**, **Windows**, **Client Library** (Biblioteca cliente), que forma parte del repositorio de archivos descargados mencionado anteriormente en los requisitos previos.

También puede averiguar cómo usar el código de la biblioteca cliente en el Explorador de soluciones: en **VisionAPI-WPF_Samples**, expanda **AnalyzePage.xaml** para buscar **AnalyzePage.xaml.cs**, que se usa para enviar una imagen al punto de conexión de análisis de imagen. Haga doble clic en los archivos .xaml.cs para tenerlos abiertos en nuevas ventanas en Visual Studio.

Durante la revisión de cómo se usa la biblioteca cliente de Vision en nuestra aplicación de ejemplo, vamos a examinar dos fragmentos de código de **AnalyzePage.xaml.cs**. El archivo contiene comentarios de código que indican "KEY SAMPLE CODE STARTS HERE" (EL CÓDIGO DE EJEMPLO PRINCIPAL COMIENZA AQUÍ) y "KEY SAMPLE CODE ENDS HERE" (EL CÓDIGO DE EJEMPLO PRINCIPAL FINALIZA AQUÍ) para ayudarle a encontrar los fragmentos de código reproducidos a continuación.

El punto de conexión de análisis puede funcionar con una dirección URL de imagen o con datos de imágenes binarias (en forma de una secuencia de octetos) como entrada. En primer lugar, encontrará una directiva using, que le permite usar la biblioteca cliente de Vision.

```
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE STARTS HERE
                // Use the following namespace for VisionServiceClient 
                // ---------------------------------------------------------------------- 
                using Microsoft.ProjectOxford.Vision; 
                using Microsoft.ProjectOxford.Vision.Contract; 
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE ENDS HERE 
                // ----------------------------------------------------------------------

```
**UploadAndAnalyzeImage(…)** Este fragmento de código muestra cómo usar la biblioteca cliente para enviar la clave de suscripción y una imagen almacenada localmente al punto de conexión de análisis del servicio Computer Vision API.

```
    private async Task<AnalysisResult> UploadAndAnalyzeImage(string imageFilePath)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------  
        //
        // Create Project Oxford Computer Vision API Service client
        //
        VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            //
            // Analyze the image for all visual features
            //
            Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
         VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
            AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageFileStream, visualFeatures);
            return analysisResult;
        }
    
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
        }
```
**AnalyzeUrl(...)**  Este fragmento de código muestra cómo usar la biblioteca cliente para enviar la clave de suscripción y una dirección URL de foto al punto de conexión de análisis del servicio Computer Vision API.

```
    private async Task<AnalysisResult> AnalyzeUrl(string imageUrl)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------
    
        //
        // Create Project Oxford Computer Vision API Service client
        //
     VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        //
        // Analyze the url for all visual features
        //
        Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
        VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
        AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageUrl, visualFeatures);
     return analysisResult;
    }
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
```
**Otras páginas y puntos de conexión** Para saber cómo interactuar con otros puntos de conexión expuestos por el servicio Computer Vision API, examine las otras páginas del ejemplo; por ejemplo, el punto de conexión OCR se muestra como parte del código contenido en OCRPage.xaml.cs 

### <a name="Related">Temas relacionados</a>
 * [Introducción a Face API](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
 
 


