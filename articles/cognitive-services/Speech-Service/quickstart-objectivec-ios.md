---
title: 'Inicio rápido: Reconocimiento de voz en Objective-C en iOS mediante el SDK de Speech Service'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en Objective-C en iOS mediante el SDK de Speech Service
services: cognitive-services
author: chlandsi
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: chlandsi
ms.openlocfilehash: 7d1e05e13e55b8b7bc07eda71d63f96f12c81ff9
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219195"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-speech-service-sdk"></a>Inicio rápido: Reconocimiento de voz en Objective-C en iOS mediante el SDK de Speech Service

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, obtendrá información sobre cómo crear una aplicación iOS en Objective-C mediante el SDK de Voz de Cognitive Services para transcribir en texto un archivo de audio con la voz grabada.

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Un equipo Mac con Xcode 9.4.1 instalado como entorno de desarrollo de iOS. Este tutorial está destinado a las versiones 11.4 de iOS. Si todavía no tiene Xcode, puede instalarlo desde la [App Store](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12).

## <a name="get-the-speech-sdk-for-ios"></a>Obtención del SDK de Voz para iOS

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

La versión actual del SDK de Speech de Cognitive Services es `1.1.0`.

En la actualidad, el SDK de Voz de Cognitive Services para iOS y Mac se distribuye como marco de Cocoa.
Se puede descargar desde https://aka.ms/csspeech/iosbinary. Descargue el archivo en el directorio principal.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode 

Inicie Xcode y haga clic en **Archivo** > **Nuevo** > **Proyecto** para iniciar un proyecto nuevo.
En el cuadro de diálogo de selección de plantilla, elija la plantilla "Aplicación de vista única (iOS)".

En los cuadros de diálogo que aparecen después, realice las selecciones siguientes:

1. Cuadro de diálogo Opciones del proyecto
    1. Escriba un nombre para la aplicación de inicio rápido, por ejemplo `helloworld`.
    1. Escriba un nombre e identificador de la organización adecuados, si ya tiene una cuenta de desarrollador de Apple. Con fines de prueba, puede elegir cualquier nombre, como `testorg`. Para firmar la aplicación, también necesita un perfil de aprovisionamiento adecuado. Consulte el [sitio para desarrolladores de Apple](https://developer.apple.com/) para obtener más información.
    1. Asegúrese de elegir Objective-C como lenguaje para el proyecto.
    1. Desactive todas las casillas de las pruebas y los datos principales.
    ![Configuración del proyecto](media/sdk/qs-objectivec-project-settings.png)
1. Selección del directorio del proyecto
    1. Elija el directorio principal para incluir el proyecto. Esto creará un directorio `helloworld` en el directorio principal que contendrá todos los archivos del proyecto de Xcode.
    1. Deshabilite la creación de un repositorio de Git para este proyecto de ejemplo.
    1. Ajuste las rutas de acceso para el SDK en *Configuración del proyecto*.
        1. En la pestaña **General** del encabezado **Binarios incrustados**, agregue la biblioteca del SDK como marco de trabajo: **Agregar binarios incrustados** > **Agregar otro...** > Vaya al directorio principal y elija el archivo `MicrosoftCognitiveServicesSpeech.framework`. Esto agregará automáticamente la biblioteca del SDK al encabezado **Marcos y bibliotecas vinculadas**.
        ![Marco de trabajo agregado](media/sdk/qs-objectivec-framework.png)
        1. Vaya a la pestaña **Configuración de compilación** y active la configuración **Todos**.
        1. Agregue el directorio `$(SRCROOT)/..` a *Rutas de búsqueda de marco* bajo el encabezado **Rutas de búsqueda**.
        ![Opción Ruta de búsqueda de marco](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Configuración de la interfaz de usuario

La aplicación de ejemplo tendrá una interfaz de usuario muy simple: dos botones para iniciar el reconocimiento de voz desde un archivo o desde la entrada del micrófono y una etiqueta de texto para mostrar el resultado.
La interfaz de usuario se configura en la parte `Main.storyboard` del proyecto.
Para abrir la vista XML del guion gráfico, haga clic con el botón derecho en la entrada `Main.storyboard` del árbol del proyecto y seleccione **Abrir como...** > **Código fuente**.
Reemplace el código XML generado automáticamente por esto:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Para descargar el [archivo wav de ejemplo](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) haga clic con el botón derecho en el vínculo y seleccione **Guardar destino como...**. Agregue el archivo wav al proyecto como un recurso arrastrándolo desde una ventana de Finder al nivel raíz de la vista del proyecto.
Haga clic en **Finalizar** en el siguiente cuadro de diálogo sin cambiar la configuración.
1. Reemplace el contenido del archivo `ViewController.m` generado automáticamente por lo siguiente:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.
1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).
1. Agregue la solicitud de acceso de micrófono. Haga clic con el botón derecho en la entrada `Info.plist` del árbol del proyecto y seleccione **Abrir como...** > **Código fuente**. Agregue las líneas siguientes en la sección `<dict>` y, después, guarde el archivo.
    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="building-and-running-the-sample"></a>Compilación y ejecución del ejemplo

1. Haga visible la salida de depuración (**Ver** > **Área de depuración** > **Activar consola**).
1. Elija el simulador de iOS o un dispositivo iOS conectado al equipo de desarrollo como destino para la aplicación desde la lista en el menú **Producto** -> **Destino**.
1. Para compilar y ejecutar el código de ejemplo en el simulador de iOS seleccione **Producto** -> **Ejecutar** en el menú o haga clic en el botón **Reproducir**.
En la actualidad, el SDK de Voz solo admite las plataformas iOS de 64 bits.
1. Tras hacer clic en el botón "Recognize (File)" (Reconocer [Archivo]) de la aplicación, debería ver el contenido del archivo de audio "What's the weather like?" (¿Qué tiempo hace?). en la parte inferior de la pantalla.

 ![Aplicación de iOS simulada](media/sdk/qs-objectivec-simulated-app.png)

1. Después haga clic en el botón "Recognize (Microphone)" (Reconocer [Micrófono]) de la aplicación y diga algunas palabras; debería ver el texto que ha dicho en la parte inferior de la pantalla.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/objectivec-ios`.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtener nuestros ejemplos](speech-sdk.md#get-the-samples)

