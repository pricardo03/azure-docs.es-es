---
title: 'Guía de inicio rápido: Reconocimiento de voz en C# en una aplicación para UWP mediante Speech SDK de Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Obtenga información sobre cómo reconocer la voz en una aplicación para UWP mediante el SDK de Voz de Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 549a8fd29cf1f986da0af1ae49dbf0e5335603bd
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325707"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-using-the-speech-sdk"></a>Guía de inicio rápido: Reconocimiento de voz en una aplicación para UWP mediante el SDK de Voz

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, obtendrá información sobre cómo crear una aplicación para Plataforma universal de Windows (UWP) mediante el SDK de Voz de Cognitive Services para transcribir la conversión de voz en texto.
La aplicación se compila con el [paquete NuGet del SDK de Voz de Microsoft Cognitive Services](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017.

> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admite Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos. Las aplicaciones que usan el SDK de Voz todavía no pasan el Kit para la certificación de aplicaciones en Windows (WACK). Es posible transferir localmente la aplicación, pero actualmente no se puede enviar a la Tienda Windows.

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Un PC Windows (Windows 10 Fall Creators Update o versión posterior) con micrófono operativo.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition o superior.
* La carga de trabajo de **desarrollo de la Plataforma universal de Windows** en Visual Studio. Puede habilitarla en **Herramientas** \> **Get Tools and Features** (Obtener herramientas y características).

  ![Habilitación del desarrollo de la Plataforma universal de Windows](media/sdk/vs-enable-uwp-workload.png)

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

1. En Visual Studio 2017, cree una aplicación vacía de Windows Universal para Visual C#. En el cuadro de diálogo **New Project** (Proyecto nuevo), en el panel de la izquierda, expanda **Installed** (Instalado) \> **Visual C#** \> **Windows Universal** y seleccione **Blank App (Universal Windows)** (Aplicación vacía [Universal Windows]). Para el nombre del proyecto, escriba *helloworld*.

    ![](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. En la ventana emergente **New Universal Windows Platform Project** (Nuevo proyecto de la Plataforma universal de Windows), elija **Windows 10 Fall Creators Update (10.0, compilación 16299)** como la **versión mínima** y esta o cualquier otra versión posterior como la **versión de destino** y haga clic en **Aceptar**.

    ![](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Si dispone de una instalación de Windows de 64 bits, puede cambiar la plataforma de compilación a `x64`.

   ![Cambio de la plataforma de compilación a x64](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > En este momento, el SDK de Voz admite los procesadores compatibles con Intel, pero no ARM.

1. Instale y haga referencia al [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget). En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Manage NuGet Packages for Solution** (Administrar paquetes de NuGet para la solución).

    ![Haga clic con el botón derecho en Administrar paquetes NuGet para la solución.](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. En la esquina superior derecha, en el campo **Origen del paquete**, elija **Nuget.org**. Busque e instale el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

    ![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-uwp-05-nuget-install-0.5.0.png "Install Nuget package")

1. Acepte la licencia en el cuadro de diálogo que aparece.

    ![Aceptación de licencia](media/sdk/qs-csharp-uwp-06-nuget-license.png "Accept the license")

1. La línea de salida siguiente aparece en la consola del Administrador de paquetes.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 0.5.0' to helloworld
   ```

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. En el Explorador de soluciones, haga doble clic en **Package.appxmanifest** para editar el manifiesto de aplicación.
   Seleccione la ficha **Capabilities** (Funcionalidades), active la casilla de verificación de la funcionalidad **Microphone** (Micrófono) y guarde los cambios.

   ![](media/sdk/qs-csharp-uwp-07-capabilities.png)

1. Edite la interfaz de usuario de la aplicación mediante doble clic en `MainPage.xaml` en el Explorador de soluciones. 

    En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente en la etiqueta de cuadrícula (entre `<Grid>` y `</Grid>`).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Edite el código subyacente XAML con doble clic en `MainPage.xaml.cs` en el Explorador de soluciones (se agrupa bajo el elemento `MainPage.xaml`).
   Reemplace todo el código de este archivo por lo siguiente.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. En el controlador `SpeechRecognitionFromMicrophone_ButtonClicked`, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. En el controlador `SpeechRecognitionFromMicrophone_ButtonClicked`, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde todos los cambios en el proyecto.

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Compile la aplicación. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Compilación correcta](media/sdk/qs-csharp-uwp-08-build.png "Successful build")

1. Inicie la aplicación. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Inicio de depuración en la aplicación](media/sdk/qs-csharp-uwp-09-start-debugging.png "Start the app into debugging")

1. Aparece una ventana de interfaz gráfica de usuario. En primer lugar, haga clic en el botón **Enable Microphone** (Habilitar teléfono) y confirme la solicitud de permiso emergente.

    ![Inicio de depuración en la aplicación](media/sdk/qs-csharp-uwp-10-access-prompt.png "Start the app into debugging")

1. Haga clic en **Speech recognition with microphone input** (Reconocimiento de voz con entrada de micrófono) y diga una frase breve en el micrófono del dispositivo. El texto reconocido aparece en la ventana.

    ![](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/csharp-uwp`.

## <a name="next-steps"></a>Pasos siguientes

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
