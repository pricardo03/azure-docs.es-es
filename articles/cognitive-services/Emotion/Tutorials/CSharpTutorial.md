---
title: 'Tutorial: Reconocimiento de emociones en las caras de una imagen mediante Emotion API con C#'
titlesuffix: Azure Cognitive Services
description: Explore una aplicación básica de Windows para reconocer las emociones expresadas por las caras de una imagen.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
ms.date: 01/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: f3a84a68718fba29e2a4b2fae057e68976119c95
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237031"
---
# <a name="tutorial-recognize-emotions-on-a-face-in-an-image"></a>Tutorial: Reconocimiento de emociones en las caras de una imagen

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/). 

Explore una aplicación básica de Windows que usa Emotion API para reconocer las emociones que expresan las caras de una imagen. El ejemplo siguiente le permite enviar la dirección URL de una imagen o un archivo almacenado localmente. Puede usar este ejemplo de código abierto como plantilla para crear su propia aplicación para Windows con Emotion API y WPF (Windows Presentation Foundation), una parte de .NET Framework.

## <a name="Prerequisites">Requisitos previos</a>
#### <a name="platform-requirements"></a>Requisitos de la plataforma  
El ejemplo siguiente se ha desarrollado para .NET Framework mediante [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).  

#### <a name="subscribe-to-emotion-api-and-get-a-subscription-key"></a>Suscripción a Emotion API y obtención de una clave de suscripción  
Antes de crear el ejemplo, debe suscribirse a Emotion API, que forma parte de Microsoft Cognitive Services. Consulte [Suscripciones](https://azure.microsoft.com/try/cognitive-services/). En este tutorial, puede usarse tanto la clave principal como la secundaria. Asegúrese de seguir los procedimientos recomendados para mantener en secreto y protegida la clave de API.  

#### <a name="get-the-client-library-and-example"></a>Obtención de la biblioteca cliente y ejemplo  
Puede descargar la biblioteca cliente de Emotion API a través del [SDK](https://www.github.com/microsoft/cognitive-emotion-windows). Debe extraer el archivo ZIP descargado en una carpeta de su elección, muchos usuarios eligen la carpeta de Visual Studio 2015.
## <a name="Step1">Paso 1: Abrir el ejemplo</a>
1.  Inicie Microsoft Visual Studio 2015 y haga clic en **Archivo**, seleccione **Abrir** y, a continuación, **Proyecto o solución**.
2.  Vaya a la carpeta donde guardó los archivos descargados de Emotion API. Haga clic en **Emotion**, **Windows** y, a continuación, en la carpeta **Sample-WPF**.
3.  Haga doble clic para abrir el archivo de solución de Visual Studio 2015 (.sln) denominado **EmotionAPI-WPF-Samples.sln**. Se abrirá la solución en Visual Studio.

## <a name="Step2">Paso 2: Compilar el ejemplo</a>
1. En el **Explorador de soluciones**, haga clic con el botón derecho en **Referencias** y seleccione **Administrar paquetes NuGet**.

  ![Abrir el Administrador de paquetes NuGet](../Images/EmotionNuget.png)

2.  Se abre la ventana**Administrador de paquetes NuGet**. Primero seleccione **Examinar** en la esquina superior izquierda y, a continuación, en el cuadro de búsqueda, escriba "Newtonsoft.Json", seleccione el paquete **Newtonsoft.Json** y haga clic en **Instalar**.  

  ![Examinar el paquete NuGet](../Images/EmotionNugetBrowse.png)  

3.  Presione Ctrl+Mayús+B, o haga clic en **Compilar** en el menú de la cinta de opciones y, a continuación, seleccione **Compilar solución**.

## <a name="Step3">Paso 3: Ejecutar el ejemplo</a>
1.  Una vez completada la compilación, presione **F5** o haga clic en **Iniciar** en el menú de la cinta de opciones para ejecutar el ejemplo.
2.  Busque la ventana de Emotion API con el **cuadro de texto** que reza "**Paste your subscription key here to start**" (Pegue la clave de suscripción aquí para comenzar). Pegue la clave de suscripción en el cuadro de texto tal y como se muestra en la siguiente captura de pantalla. Puede optar por mantener la clave de suscripción en su equipo portátil o de escritorio; para ello, haga clic en el botón "Save Key" (Guardar clave). Si quiere eliminar la clave de suscripción del sistema, haga clic en "Delete Key" (Eliminar clave) para quitarla del equipo de escritorio o portátil.

  ![Interfaz de funcionalidad Emotion](../Images/EmotionKey.png)

3.  En "**Seleccionar escenario**", haga clic para utilizar cualquiera de los dos escenarios, "**Detect emotion using a stream**" (Detectar emoción con una secuencia) o "**Detect emotion using a URL**" (Detectar emoción con dirección URL), a continuación, siga las instrucciones que aparecen en la pantalla. Microsoft recibe las imágenes cargadas y puede usarlas para mejorar Emotion API y los servicios relacionados. Al enviar una imagen, confirma que ha seguido nuestro [Código de conducta del desarrollador](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).
4.  Hay imágenes de ejemplo que se usan con esta aplicación de ejemplo. Estas imágenes las puede encontrar en el **repositorio de Github de Face API**, en la carpeta [Data](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data). Tenga en cuenta que se otorga licencia para el uso de estas imágenes con un contrato de uso justo, es decir, que puede usarlas para probar este ejemplo, pero no para republicarlas.

## <a name="Review">Revisión y comprensión</a>
Ahora que tiene una aplicación en ejecución, vamos a revisar cómo se integra esta aplicación de ejemplo con Microsoft Cognitive Services. Esta integración permitirá que sea más fácil seguir construyendo sobre esta aplicación o desarrollar su propia aplicación mediante Microsoft Emotion API.

Esta aplicación de ejemplo hace uso de la biblioteca cliente de Emotion API, un contenedor de cliente de C# fino para Microsoft Emotion API. Si ha compilado la aplicación de ejemplo como se ha descrito anteriormente, ha obtenido la biblioteca cliente del paquete NuGet. Puede revisar el código fuente de la biblioteca cliente en la carpeta "[Client Library](https://github.com/Microsoft/Cognitive-Emotion-Windows/tree/master/ClientLibrary)" en **Emotion**, **Windows**, **Client Library**, que forma parte del repositorio de archivos descargado mencionado anteriormente en los [requisitos previos](#Prerequisites).

También puede encontrar información sobre cómo usar el código de Client Library en **Explorador de soluciones**: en **EmotionAPI-WPF_Samples**, expanda **DetectEmotionUsingStreamPage.xaml** para ubicar **DetectEmotionUsingStreamPage.xaml.cs**, que se usa para la exploración en un archivo almacenado localmente, o expanda **DetectEmotionUsingURLPage.xaml** para buscar **DetectEmotionUsingURLPage.xaml.cs**, que se usa al cargar la dirección URL de una imagen. Haga doble clic en los archivos .xaml.cs para que se abran en nuevas ventanas en Visual Studio.

Revisar cómo se usa la biblioteca cliente de Emotion en nuestra aplicación de ejemplo, nos permite echar un vistazo a dos fragmentos de código de **DetectEmotionUsingStreamPage.xaml.cs** y **DetectEmotionUsingURLPage.xaml.cs**. Cada archivo contiene comentarios de código que indican "KEY SAMPLE CODE STARTS HERE" (EL CÓDIGO DE EJEMPLO PRINCIPAL COMIENZA AQUÍ) y "KEY SAMPLE CODE ENDS HERE" (EL CÓDIGO DE EJEMPLO PRINCIPAL FINALIZA AQUÍ) para ayudarle a encontrar los fragmentos de código reproducidos a continuación.

Emotion API puede funcionar con la dirección URL de una imagen o con datos de imágenes binarias (en forma de una secuencia de octetos) como entrada. A continuación se revisan las dos opciones. En ambos casos, en primer lugar encontrará una directiva using, que le permite usar la biblioteca cliente de Emotion.
```csharp

            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Use the following namespace for EmotionServiceClient
            // -----------------------------------------------------------------------
            using Microsoft.ProjectOxford.Emotion;
            using Microsoft.ProjectOxford.Emotion.Contract;
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
#### <a name="detectemotionusingurlpagexamlcs"></a>DetectEmotionUsingURLPage.xaml.cs

Este fragmento de código muestra cómo usar la biblioteca cliente para enviar la clave de suscripción y la dirección URL de una foto al servicio Emotion API.

```csharp

            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            window.Log("EmotionServiceClient is created");

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                //
                // Detect the emotions in the URL
                //
                Emotion[] emotionResult = await emotionServiceClient.RecognizeAsync(url);
                return emotionResult;
            }
            catch (Exception exception)
            {
                window.Log("Detection failed. Please make sure that you have the right subscription key and proper URL to detect.");
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
#### <a name="detectemotionusingstreampagexamlcs"></a>DetectEmotionUsingStreamPage.xaml.cs

A continuación se muestra cómo enviar la clave de suscripción y una imagen almacenada localmente a Emotion API.


```csharp


            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                Emotion[] emotionResult;
                using (Stream imageFileStream = File.OpenRead(imageFilePath))
                {
                    //
                    // Detect the emotions in the URL
                    //
                    emotionResult = await emotionServiceClient.RecognizeAsync(imageFileStream);
                    return emotionResult;
                }
            }
            catch (Exception exception)
            {
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
<!--
## <a name="Related">Related Topics</a>
[Emotion API Overview](.)
-->
