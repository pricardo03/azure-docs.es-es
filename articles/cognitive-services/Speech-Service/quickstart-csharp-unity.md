---
title: 'Inicio rápido: Reconocimiento de voz con Unity (servicios Voz)'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de conversión de voz a texto con Unity y el SDK de Voz para Unity (Beta). Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: cdde9f0ec69bec48ae0fb747db0cc49e81920817
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57872586"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-unity-beta"></a>Inicio rápido: Reconocimiento de voz con el SDK de Voz para Unity (Beta)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Use esta guía para crear una aplicación de conversión de voz a texto con [Unity](https://unity3d.com/) y el SDK de Voz para Unity (Beta).
Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
Si no está familiarizado con Unity, se recomienda que consulte el [manual del usuario de Unity](https://docs.unity3d.com/Manual/UnityManual.html) antes de iniciar el desarrollo de aplicaciones.

> [!NOTE]
> El SDK de Voz para Unity está actualmente en versión beta.
> Es compatible con Windows x86 y x64 (aplicación de escritorio independiente o plataforma universal de Windows) y Android (ARM32/64, x86).

## <a name="prerequisites"></a>Requisitos previos

Para completar este proyecto necesitará lo siguiente:

* [Unity 2018.3 o posterior](https://store.unity.com/)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción para el servicio Voz. [Obtenga una gratis](get-started.md).
* Acceso al micrófono del equipo.

## <a name="create-a-unity-project"></a>Creación de un proyecto de Unity

* Inicie Unity y, en la pestaña **Projects** (Proyectos), seleccione **New** (Nuevo).
* En **Project name** (Nombre del proyecto) especifique **csharp-unity**, en **Template** (Plantilla) seleccione **3D** y elija una ubicación.
  A continuación, seleccione **Create project** (Crear proyecto).
* Después de un tiempo, aparecerá la ventana del editor de Unity.

## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

* El SDK de Voz para Unity (Beta) se empaqueta como un paquete de recursos de Unity (.unitypackage).
  Puede descargarlo [aquí](https://aka.ms/csspeech/unitypackage).
* Importe el SDK de Voz seleccionando **Assets (Recursos)** > **Import Package (Importar paquete)** > **Custom Package (Paquete personalizado)**.
  Para más información, consulte la [documentación de Unity](https://docs.unity3d.com/Manual/AssetPackages.html).
* En el selector de archivos, seleccione el archivo .unitypackage del SDK de Voz que descargó anteriormente.
* Asegúrese de que se seleccionan todos los archivos y haga clic en **Import** (Importar):

  ![Captura de pantalla del editor de Unity al importar el paquete de recursos de Unity para el SDK de Voz](media/sdk/qs-csharp-unity-01-import.png)

## <a name="add-ui"></a>Incorporación de la interfaz de usuario

Vamos a agregar una pequeña interfaz de usuario a nuestro escenario que consistirá en un botón para desencadenar el reconocimiento de voz y un campo de texto para que aparezca el resultado.

* En la [ventana de jerarquía](https://docs.unity3d.com/Manual/Hierarchy.html) (que se sitúa de forma predeterminada a la izquierda), aparece una escena de ejemplo que Unity ha creado con el nuevo proyecto.
* Haga clic en el **crear** situado en la parte superior de la ventana de jerarquía y seleccione **UI** > **botón**.
* Esto crea tres objetos de juego que se pueden ver en la ventana de jerarquía: un objeto **Button** (Botón) anidado dentro de un objeto **Canvas** (Lienzo) y un objeto **EventSystem**.
* [Vaya a la vista de escena](https://docs.unity3d.com/Manual/SceneViewNavigation.html) para ver una buena vista del lienzo y del botón en la [vista de escena](https://docs.unity3d.com/Manual/UsingTheSceneView.html).
* Haga clic en el objeto **Button** (Botón) en la ventana de jerarquía para mostrar su configuración en la [ventana del inspector](https://docs.unity3d.com/Manual/UsingTheInspector.html) (que se sitúa de forma predeterminada a la derecha).
* Establezca las propiedades **Pos X** y **Pos Y** en **0**, para que el botón se sitúe en el centro del lienzo.
* Haga clic en el botón **Crear** en la parte superior de la ventana de jerarquía de nuevo y seleccione el botón **IU** > **Texto** para crear un campo de texto.
* Haga clic en el objeto **Text** (Texto) en la ventana de jerarquía para mostrar su configuración en la [ventana del inspector](https://docs.unity3d.com/Manual/UsingTheInspector.html) (que se sitúa de forma predeterminada a la derecha).
* Establezca las propiedades **Pos X** y **Pos Y** en **0** y **120**, establezca las propiedades **Width** (Anchura) y **Height** (Altura) en **240** y **120** para asegurarse de que el campo de texto y el botón no se superponen.

Cuando haya terminado, la interfaz de usuario debe ser similar a esta captura de pantalla:

[![Captura de pantalla de la interfaz de usuario de inicio rápido del editor de Unity](media/sdk/qs-csharp-unity-02-ui-inline.png)](media/sdk/qs-csharp-unity-02-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. En la [ventana del proyecto](https://docs.unity3d.com/Manual/ProjectView.html) (que se sitúa de forma predeterminada en la parte inferior izquierda), haga clic en el botón **Create** y, a continuación, seleccione **C# script** (Script de C#). Llame `HelloWorld` al script.

1. Edite el script haciendo doble clic en él.

   > [!NOTE]
   > Puede configurar qué editor de código se va a iniciar en **Edit (Editar)** > **Preferences (Preferencias)**, consulte el [manual del usuario de Unity](https://docs.unity3d.com/Manual/Preferences.html).

1. Reemplace todo el código por lo siguiente:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Localice la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción a los servicios de voz.

1. Localice la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a sus suscripción. Por ejemplo, si usa la evaluación gratuita, la región es `westus`.

1. Guarde los cambios en el script.

1. De vuelta en el editor de Unity, se debe agregar el script como componente a uno de los objetos del juego.

   * Haga clic en el objeto **Canvas** (Lienzo) en la ventana de jerarquía. Esto abre la configuración en la [ventana del inspector](https://docs.unity3d.com/Manual/UsingTheInspector.html) (que se sitúa de forma predeterminada a la derecha).
   * Haga clic en el botón **Add Component** (Agregar componente) en la ventana del inspector y, a continuación, busque el script HelloWorld que creamos anteriormente y agréguelo.
   * Tenga en cuenta que el componente Hola mundo tiene dos propiedades sin inicializar, **Output Text** (Texto de salida) y **Start Reco Button** (Botón Iniciar reconocimiento), que coinciden con las propiedades públicas de la clase `HelloWorld`.
     Para conectarlos, haga clic en el selector de objetos (el icono de círculo pequeño situado a la derecha de la propiedad) y elija los objetos Text y Button que creó anteriormente.

     > [!NOTE]
     > El botón también tiene un objeto Text anidado. Asegúrese de que no lo selecciona accidentalmente para la salida de texto (o cambie el nombre de uno de los objetos de texto mediante el campo Name (Nombre) en la ventana del inspector para evitar esa confusión).

## <a name="run-the-application-in-the-unity-editor"></a>Ejecución de la aplicación en el editor de Unity

* Pulse el botón **Play** (Reproducir) en la barra de herramientas del editor de Unity (debajo de la barra de menús).

* Una vez que se inicie la aplicación, haga clic en el botón y pronuncie una expresión o frase en inglés en el micrófono del equipo. Lo que diga se transmitirá a los servicios de voz y se transcribirá en texto, que aparece en la misma ventana.

  [![Captura de pantalla del inicio rápido en ejecución en la ventana de juego de Unity](media/sdk/qs-csharp-unity-03-output-inline.png)](media/sdk/qs-csharp-unity-03-output-expanded.png#lightbox)

* Compruebe la [ventana de consola](https://docs.unity3d.com/Manual/Console.html) para ver los mensajes de depuración.

* Cuando haya terminado el reconocimiento de voz, haga clic en el botón **Play** (Reproducir) en la barra de herramientas del editor de Unity para detener la aplicación.

## <a name="additional-options-to-run-this-application"></a>Opciones adicionales para ejecutar esta aplicación

También se puede implementar esta aplicación en Android, como una aplicación independiente de Windows o una aplicación de UWP.
Consulte el [repositorio de ejemplo](https://aka.ms/csspeech/samples) en la carpeta quickstart/csharp-unity que describe la configuración para estos destinos adicionales.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
