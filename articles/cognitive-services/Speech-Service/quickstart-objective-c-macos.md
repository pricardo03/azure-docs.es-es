---
title: 'Inicio rápido: Reconocimiento de voz, Objective-C (Speech Service)'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en Objective-C para macOS mediante el SDK de Voz.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/03/2019
ms.author: chlandsi
ms.openlocfilehash: 67f2531b24796de1e00505fdc757f3c2244c5054
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002348"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en Objective-C para macOS mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a crear una aplicación macOS en Objective-C mediante el SDK de Voz de Cognitive Services para transcribir a texto la voz grabada con un micrófono.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, presentamos una lista de requisitos previos:

* Una [clave de suscripción](get-started.md) para Speech Service
* Una máquina macOS con [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) o posterior y macOS 10.13 o posterior

## <a name="get-the-speech-sdk-for-macos"></a>Obtención del SDK de Voz para macOS

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

La versión actual del SDK de Speech de Cognitive Services es `1.5.1`.

El SDK de Voz de Cognitive Services para Mac se distribuye como un paquete de marcos.
Se puede usar en proyectos de Xcode como un administrador de dependencias [CocoaPod](https://cocoapods.org/), o se puede descargar desde https://aka.ms/csspeech/macosbinary y vincularse manualmente. En esta guía se usa CocoaPod.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Inicie Xcode y haga clic en **Archivo** > **Nuevo** > **Proyecto** para iniciar un proyecto nuevo.
En el cuadro de diálogo de selección de plantilla, elija la plantilla "Aplicación de Cocoa".

En los cuadros de diálogo que aparecen después, realice las selecciones siguientes:

1. Cuadro de diálogo Opciones del proyecto
    1. Escriba un nombre para la aplicación de inicio rápido, por ejemplo `helloworld`.
    1. Si ya tiene una cuenta de desarrollador de Apple, escriba un nombre y un identificador de organización adecuados. Con fines de prueba, puede elegir cualquier nombre, como `testorg`. Para firmar la aplicación, necesita un perfil de aprovisionamiento adecuado. Consulte el [sitio para desarrolladores de Apple](https://developer.apple.com/) para más información.
    1. Asegúrese de elegir Objective-C como lenguaje para el proyecto.
    1. Desactive las casillas para usar guiones gráficos y para crear una aplicación basada en documentos. La interfaz de usuario sencilla para la aplicación de ejemplo se creará mediante programación.
    1. Desactive todas las casillas de las pruebas y los datos principales.
    ![Configuración del proyecto](media/sdk/qs-objectivec-macos-project-settings.png)
1. Selección del directorio del proyecto
    1. Elija un directorio en el que colocar el proyecto. Se crea un directorio `helloworld` en el directorio principal que contiene todos los archivos del proyecto de Xcode.
    1. Deshabilite la creación de un repositorio de Git para este proyecto de ejemplo.
1. Establezca los derechos de acceso a la red y al micrófono. Haga clic en el nombre de la aplicación en la primera línea de la información general a la izquierda para obtener la configuración de aplicación y, a continuación, elija la pestaña "Funcionalidades".
    1. Habilite la opción "App Sandbox" para la aplicación.
    1. Habilite las casillas de acceso a "Conexiones salientes" y "Micrófono".
    ![Configuración de espacio aislado](media/sdk/qs-objectivec-macos-sandbox.png)
1. La aplicación también debe declarar el uso del micrófono en el archivo `Info.plist`. Haga clic en el archivo en la información general y agregue la clave "Privacidad: descripción de uso del micrófono", con un valor como "Microphone is needed for speech recognition" (Se necesita el micrófono para el reconocimiento de voz).
    ![Configuración de Info.plist](media/sdk/qs-objectivec-macos-info-plist.png)
1. Cierre el proyecto de Xcode. Más tarde, usará una instancia diferente de él después de configurar CocoaPods.

## <a name="install-the-sdk-as-a-cocoapod"></a>Instalación del SDK como CocoaPod

1. Instale el administrador de dependencias de CocoaPod como se describe en sus [instrucciones de instalación](https://guides.cocoapods.org/using/getting-started.html).
1. Vaya al directorio de la aplicación de ejemplo (`helloworld`). Coloque un archivo de texto con el nombre `Podfile` y el siguiente contenido en ese directorio:
    ```
    target 'helloworld' do
        platform :osx, '10.13'
        pod 'MicrosoftCognitiveServicesSpeech-macOS', '~> 1.5.1'
    end
    ```
1. Vaya al directorio `helloworld` en un terminal y ejecute el comando `pod install`. Se genera un área de trabajo de Xcode `helloworld.xcworkspace` que contiene la aplicación de ejemplo y el SDK de Voz como dependencia. Este área de trabajo se usa en las secciones siguientes.

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Abra el área de trabajo `helloworld.xcworkspace` en Xcode.
1. Reemplace el contenido del archivo `AppDelegate.m` generado automáticamente por lo siguiente:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/helloworld/AppDelegate.m#code)]
1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.
1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Haga visible la salida de depuración (**Ver** > **Área de depuración** > **Activar consola**).
1. Para compilar y ejecutar el código de ejemplo, seleccione **Producto** -> **Ejecutar** en el menú o haga clic en el botón **Reproducir**.
1. Después de hacer clic en el botón y decir algunas palabras, debería ver el texto que ha dicho en la parte inferior de la pantalla. Al ejecutar la aplicación por primera vez, se le solicitará que proporcione a la aplicación acceso al micrófono del equipo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Objective-C en GitHub](https://aka.ms/csspeech/samples)

