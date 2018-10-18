---
title: 'Inicio rápido: Reconocimiento de voz en C# en una aplicación para UWP mediante el SDK de Voz de Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Obtenga información sobre cómo reconocer la voz en una aplicación para UWP mediante el SDK de Voz de Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: d9a90869e060d2f8f1a1c522a4528e74841caada
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339621"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en una aplicación para UWP mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, creará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el [SDK de Voz](speech-sdk.md) de Cognitive Services. Transcribe de voz a texto en tiempo real desde el micrófono de su dispositivo. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admite Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos.

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción del servicio Voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita del servicio Voz](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

1. Inicie Visual Studio 2017.

1. Asegúrese de que la carga de trabajo de **desarrollo de Plataforma universal de Windows** está disponible. Elija **Herramientas** > **Get Tools and Features (Obtener herramientas y características)** desde la barra de menús de Visual Studio para abrir el instalador de Visual Studio. Si esta carga de trabajo ya está habilitada, cierre el cuadro de diálogo. 

    ![Captura de pantalla del instalador de Visual Studio, con la pestaña Workloads (Cargas de trabajo) resaltada](media/sdk/vs-enable-uwp-workload.png)

    En caso contrario, active la casilla junto a **Desarrollo multiplataforma de .NET Core** y elija **Modificar** en la esquina inferior derecha del cuadro de diálogo. La instalación de la nueva característica tardará un momento.

1. Crear una aplicación Windows Universal de Visual C# vacía. En primer lugar, elija **Archivo** > **Nuevo** > **Proyecto** en el menú. En el cuadro de diálogo **Nuevo proyecto**, expanda **Instalado** > **Visual C#** > **Windows Universal** en el panel izquierdo. Luego, seleccione **Aplicación vacía (Windows universal)**. Para el nombre del proyecto, escriba *helloworld*.

    ![Captura de pantalla del cuadro de diálogo Nuevo proyecto](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. El SDK de Voz requiere que la aplicación se cree para Windows 10 Fall Creators Update o posterior. En la ventana emergente **New Universal Windows Platform Project** (Nuevo proyecto de la Plataforma universal de Windows), elija **Windows 10 Fall Creators Update (10.0, compilación 16299)** como la **versión mínima**. En el cuadro **Versión de destino**, seleccione esta o cualquier versión posterior y, a continuación, haga clic en **Aceptar**.

    ![Captura de pantalla de la ventana New Universal Windows Platform Project (Nuevo proyecto de la Plataforma universal de Windows)](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Si está ejecutando Windows de 64 bits, puede cambiar la plataforma de compilación `x64` mediante el menú desplegable en la barra de herramientas de Visual Studio. (Windows de 64 bits puede ejecutar aplicaciones de 32 bits, por lo que puede dejarlo establecido en `x86` si lo prefiere).

   ![Captura de pantalla de la barra de herramientas de Visual Studio, con x64 resaltado](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > El SDK de Voz solo admite los procesadores compatibles con Intel. Actualmente, ARM no se admite.

1. Instale y haga referencia al [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget). En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Manage NuGet Packages for Solution** (Administrar paquetes de NuGet para la solución).

    ![Captura de pantalla del Explorador de soluciones, con la opción Administrar paquetes NuGet para la solución resaltada](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. En la esquina superior derecha, en el campo **Origen del paquete**, seleccione **nuget.org**. Busque el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

    ![Captura de pantalla del cuadro de diálogo de administrar paquetes para la solución](media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png "Instalación de paquete de NuGet")

1. Acepte la licencia que aparece para comenzar la instalación del paquete de NuGet.

    ![Captura de pantalla de cuadro de diálogo de aceptación de licencia](media/sdk/qs-csharp-uwp-06-nuget-license.png "Acepte la licencia")

1. La línea de salida siguiente aparece en la consola del Administrador de paquetes.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 1.0.1' to helloworld
   ```

1. Dado que la aplicación usa el micrófono para entrada de voz, agregue la funcionalidad de **micrófono** al proyecto. En el Explorador de soluciones, haga doble clic en **Package.appxmanifest** para editar el manifiesto de aplicación. Después, seleccione la pestaña **Capabilities** (Funcionalidades), active la casilla de verificación de la funcionalidad **Microphone** (Micrófono) y guarde los cambios.

   ![Captura de pantalla del manifiesto de aplicación de Visual Studio, con las opciones Capabilities (Funcionalidades) y Microphone (Micrófono) resaltadas](media/sdk/qs-csharp-uwp-07-capabilities.png)


## <a name="add-sample-code"></a>Adición de un código de ejemplo

1. La interfaz de usuario de la aplicación se define mediante el uso de XAML. Abra `MainPage.xaml` en el Explorador de soluciones. En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente en la etiqueta de cuadrícula (entre `<Grid>` y `</Grid>`).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Abra el archivo de código subyacente `MainPage.xaml.cs` (Búsquelo agrupado en `MainPage.xaml`). Reemplace todo el código que contiene por lo siguiente.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. En el controlador `SpeechRecognitionFromMicrophone_ButtonClicked` de este archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. En el controlador `SpeechRecognitionFromMicrophone_ButtonClicked`, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde todos los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-uwp-08-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-uwp-09-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana. Seleccione **Enable Microphone** (Habilitar teléfono) y confirme la solicitud de permiso emergente.

    ![Captura de pantalla de la solicitud de permiso](media/sdk/qs-csharp-uwp-10-access-prompt.png "Iniciar la aplicación en depuración")

1. Seleccione **Speech recognition with microphone input** (Reconocimiento de voz con entrada de micrófono) y diga una frase en inglés en el micrófono del dispositivo. Lo que diga se transmitirá al servicio de Voz y se transcribe en texto, que aparece en la misma ventana.

    ![Captura de pantalla de la interfaz de usuario de reconocimiento de voz](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/csharp-uwp`.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Reconocimiento de intenciones a partir de contenido de voz con el SDK de Voz para C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Otras referencias

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
