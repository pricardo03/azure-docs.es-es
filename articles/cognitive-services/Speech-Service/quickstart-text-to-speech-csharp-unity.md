---
title: 'Inicio rápido: Síntesis de voz en Unity (Servicios de voz)'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de conversión de texto a voz con Unity y el SDK de Voz para Unity. Cuando termine, puede sintetizar la voz a partir de texto en tiempo real en el altavoz del dispositivo.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 9/19/2019
ms.author: yinhew
ms.openlocfilehash: b4d329d9d3c2a259fb90b0278c54ba8590590995
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675485"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-unity"></a>Inicio rápido: Síntesis de Voz con el SDK de voz para Unity

También hay inicios rápidos disponibles para el [reconocimiento de voz](quickstart-csharp-unity.md).

Use esta guía para crear una aplicación de conversión de texto a voz mediante [Unity](https://unity3d.com/) y el SDK de Voz de Azure Cognitive Services para Unity.
Cuando termine, puede sintetizar la voz a partir de texto en tiempo real en el altavoz del dispositivo.
Si no está familiarizado con Unity, consulte el [manual del usuario de Unity](https://docs.unity3d.com/Manual/UnityManual.html) antes de iniciar el desarrollo de aplicaciones.

> [!NOTE]
> Unity es compatible con Windows Desktop (x86 y x64) o con la Plataforma universal de Windows (x86, x64, ARM o ARM64), Android (x86, ARM32/64) e iOS (simulador de x64, ARM32 y ARM64).

## <a name="prerequisites"></a>Requisitos previos

Para completar este proyecto necesitará lo siguiente:

* [Unity 2018.3 o posterior](https://store.unity.com/) con [Unity 2019.1 que agrega compatibilidad con UWP ARM64](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal).
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). La versión 15.9 o posterior de Visual Studio 2017 también es aceptable.
* Para obtener compatibilidad con Windows ARM64, instale las [herramientas de compilación opcionales para ARM64 y el SDK de Windows 10 para ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).
* Una clave de suscripción para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-unity-project"></a>Creación de un proyecto de Unity

* Inicie Unity y, en la pestaña **Projects** (Proyectos), seleccione **New** (Nuevo).
* En **Project name** (Nombre del proyecto) especifique **csharp-unity**, en **Template** (Plantilla) seleccione **3D** y elija una ubicación.
  A continuación, seleccione **Create project** (Crear proyecto).
* Después de un tiempo, aparecerá la ventana del editor de Unity.

## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

[!INCLUDE [License notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

* El SDK de Voz para Unity (Beta) se empaqueta como un paquete de recursos de Unity (.unitypackage). Descárguelo de [este sitio web](https://aka.ms/csspeech/unitypackage).
* Importe el SDK de Voz seleccionando **Assets (Recursos)**  > **Import Package (Importar paquete)**  > **Custom Package (Paquete personalizado)** . Para más información, consulte la [documentación sobre Unity](https://docs.unity3d.com/Manual/AssetPackages.html).
* En el selector de archivos, seleccione el archivo .unitypackage del SDK de Voz que descargó.
* Asegúrese de que se seleccionan todos los archivos y seleccione **Import** (Importar).

  ![Captura de pantalla del editor de Unity al importar el paquete de recursos de Unity para el SDK de Voz](media/sdk/qs-csharp-unity-01-import.png)

## <a name="add-a-ui"></a>Incorporación de una interfaz de usuario

Vamos a agregar una pequeña interfaz de usuario a nuestro escenario que consiste en un campo de entrada para escribir el texto para la síntesis, en un botón para desencadenar la síntesis de voz y un campo de texto para que aparezca el resultado.

* En la [ventana de jerarquía](https://docs.unity3d.com/Manual/Hierarchy.html) (que se sitúa de forma predeterminada a la izquierda), aparece una escena de ejemplo que Unity ha creado con el nuevo proyecto.
* Seleccione el botón **Crear** situado en la parte superior de la ventana de **jerarquía** y seleccione **IU** > **Campo de entrada**.
* Esta opción crea tres objetos de juego que se pueden ver en la ventana de **jerarquía**: un objeto **Input Field** (Campo de entrada) anidado dentro de un objeto **Canvas** (Lienzo) y un objeto **EventSystem**.
* [Vaya a la vista de escena](https://docs.unity3d.com/Manual/SceneViewNavigation.html) para ver una buena vista del lienzo y del campo de entrada en la [vista de escena](https://docs.unity3d.com/Manual/UsingTheSceneView.html).
* Seleccione el objeto **Input Field** (Campo de entrada) en la ventana de **jerarquía** para mostrar su configuración en la [ventana del inspector](https://docs.unity3d.com/Manual/UsingTheInspector.html) (que se sitúa de forma predeterminada a la derecha).
* Establezca las propiedades **Pos X** y **Pos Y** en **0**, para que el campo de entrada se sitúe en el centro del lienzo.
* Seleccione de nuevo el botón **Crear** situado en la parte superior de la ventana de **jerarquía**. Seleccione **IU** > **Botón** para crear un botón.
* Seleccione el objeto **Button** (Botón) en la ventana de **jerarquía** para mostrar su configuración en la [ventana del inspector](https://docs.unity3d.com/Manual/UsingTheInspector.html) (que se sitúa de forma predeterminada a la derecha).
* Establezca las propiedades **Pos X** y **Pos Y** en **0** y **-48**. Establezca las propiedades **Width** (Anchura) y **Height** (Altura) en **160** y **30** para asegurarse de que el botón y el campo de entrada no se superponen.
* Seleccione de nuevo el botón **Crear** situado en la parte superior de la ventana de **jerarquía**. Seleccione **IU** > **Texto** para crear un campo de texto.
* Seleccione el objeto **Text** (Texto) en la ventana de **jerarquía** para mostrar su configuración en la [ventana del inspector](https://docs.unity3d.com/Manual/UsingTheInspector.html) (que se sitúa de forma predeterminada a la derecha).
* Establezca las propiedades **Pos X** y **Pos Y** en **0** y **80**. Establezca las propiedades **Width** (Anchura) y **Height** (Altura) en **320** y **80** para asegurarse de que el campo de texto y el campo de entrada no se superponen.
* Seleccione de nuevo el botón **Crear** situado en la parte superior de la ventana de **jerarquía**. Seleccione **Audio** > **Origen de audio** para crear un origen de audio.

Cuando haya terminado, la interfaz de usuario se parecerá a esta captura de pantalla:

[![Captura de pantalla de la interfaz de usuario de inicio rápido del editor de Unity](media/sdk/qs-tts-csharp-unity-ui-inline.png)](media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. En la [ventana del proyecto](https://docs.unity3d.com/Manual/ProjectView.html) (que se sitúa de forma predeterminada en la parte inferior izquierda), seleccione el botón **Crear** y, a continuación, seleccione **Script de C#** . Llame `HelloWorld` al script.

1. Edite el script haciendo doble clic en él.

   > [!NOTE]
   > Puede configurar qué editor de código se inicia seleccionando **Editar** > **Preferencias**. Para obtener más información, consulte el [Manual de usuario de Unity](https://docs.unity3d.com/Manual/Preferences.html).

1. Reemplace todo el código por lo siguiente:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Localice la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción a los servicios de voz.

1. Localice la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a sus suscripción. Por ejemplo, si usa la evaluación gratuita, la región es `westus`.

1. Guarde los cambios en el script.

1. De nuevo en el editor de Unity, agregue el script como componente a uno de los objetos de juego.

   * Seleccione el objeto **Canvas** (Lienzo) en la ventana de **jerarquía** para abrir su configuración en la [ventana del inspector](https://docs.unity3d.com/Manual/UsingTheInspector.html) (que se sitúa de forma predeterminada a la derecha).
   * En la ventana del **inspector**, seleccione el botón **Agregar componente**. A continuación, busque el script `HelloWorld` que hemos creado anteriormente y agréguelo.
   * El componente HelloWorld tiene cuatro propiedades sin inicializar, **Output Text** (Texto de salida), **Input Field** (Campo de entrada), **Speak Button** (Botón Voz) y **Audio Source** (Origen de audio), que coinciden con las propiedades públicas de la clase `HelloWorld`.
     Para conectarlos, seleccione el selector de objetos (el icono de círculo pequeño situado a la derecha de la propiedad). Seleccione los objetos de texto y botón que creó anteriormente.

     > [!NOTE]
     > El campo de entrada y el botón también tienen un objeto de texto anidado. Asegúrese de no seleccionarlo accidentalmente para la salida de texto. O bien, puede cambiar el nombre de los objetos de texto que usan el campo **Name** (Nombre) en la ventana del **inspector** para evitar esa confusión.

## <a name="run-the-application-in-the-unity-editor"></a>Ejecución de la aplicación en el editor de Unity

* Seleccione el botón **Play** (Reproducir) en la barra de herramientas del editor de Unity que está debajo de la barra de menús.
* Después de que se inicie la aplicación, escriba algún texto en el campo de entrada y seleccione el botón. El texto se transmite a los servicios de Voz y se sintetiza en voz, la cual se reproduce en el altavoz.

  [![Captura de pantalla del inicio rápido en ejecución en la ventana de juego de Unity](media/sdk/qs-tts-csharp-unity-output-inline.png)](media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Compruebe en la [ventana de consola](https://docs.unity3d.com/Manual/Console.html) los mensajes de depuración.
* Cuando haya terminado la síntesis de voz, seleccione el botón **Play** (Reproducir) en la barra de herramientas del editor de Unity para detener la aplicación.

## <a name="additional-options-to-run-this-application"></a>Opciones adicionales para ejecutar esta aplicación

También se puede implementar esta aplicación en Android, como una aplicación independiente de Windows o una aplicación de UWP.
Consulte el [repositorio de ejemplo](https://aka.ms/csspeech/samples) en la carpeta quickstart/csharp-unity que describe la configuración para estos destinos adicionales.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalizar las fuentes de voz](how-to-customize-voice-font.md)
- [Grabación de ejemplos de voz](record-custom-voice-samples.md)
