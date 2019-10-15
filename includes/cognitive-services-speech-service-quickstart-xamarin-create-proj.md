---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2019
ms.author: erhopf
ms.openlocfilehash: 0e4e67710c98b80dce2b0d55a86869625f3942d2
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71837428"
---
Si quiere crear un proyecto de Visual Studio para el desarrollo de una aplicación móvil multiplataforma para .NET con Xamarin, debe configurar las opciones de desarrollo de Visual Studio, crear el proyecto, seleccionar la arquitectura de destino e instalar el SDK de Voz.

### <a name="set-up-visual-studio-development-options"></a>Configuración de las opciones de desarrollo de Visual Studio

Para empezar, asegúrese de que Visual Studio está configurado correctamente para el desarrollo para dispositivos móviles multiplataforma con .NET:

1. Abra Visual Studio 2019.

1. En la barra de menús de Visual Studio, seleccione **Herramientas** > **Get Tools and Features** (Obtener herramientas y características) para abrir el Instalador de Visual Studio y ver el cuadro de diálogo **Modificar**.

   ![Pestaña Cargas de trabajo, cuadro de diálogo Modificar, Instalador de Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-workload.png)

1. En la pestaña **Cargas de trabajo**, en **Windows**, busque la carga de trabajo **Desarrollo para dispositivos móviles con .NET**. Si la casilla situada junto a esa carga de trabajo ya está seleccionada, cierre el cuadro de diálogo **Modificar** y vaya al paso 5.

1. Active la casilla **Desarrollo para dispositivos móviles con .NET**, seleccione **Modificar** y, a continuación, en el cuadro de diálogo **Antes de comenzar**, seleccione **Continuar** para instalar la carga de trabajo de desarrollo para dispositivos móviles con .NET. La instalación de la nueva característica puede tardar un rato.

1. Cierre el Instalador de Visual Studio.

### <a name="create-the-project"></a>Creación del proyecto

1. En la barra de menús de Visual Studio, elija **Archivo** > **Nuevo** > **Proyecto** para mostrar la ventana **Crear un nuevo proyecto**.

   ![Creación de un proyecto: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-create-new-project.png)

1. Busque y seleccione **Aplicación móvil (Xamarin Forms)** .

1. Seleccione **Siguiente** para abrir la pantalla **Configurar el nuevo proyecto**. 

   ![Configuración de un nuevo proyecto: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-configure-your-new-project.png)

1. En **Nombre del proyecto**, escriba `helloworld`.

1. En **Ubicación**, vaya la carpeta en la que desea guardar el proyecto y selecciónela o créela.

1. Seleccione **Crear** para ir a la ventana de **nuevo proyecto aplicación móvil de Xamarin Forms**.

   ![Cuadro de diálogo Nuevo proyecto de la Plataforma universal de Windows: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-new-xamarin-project.png)

1. Seleccione la plantilla **En blanco**.

1. En **Plataforma**, active las casillas **Android**, **iOS** y **Windows (UWP)** .

1. Seleccione **Aceptar**. Se le devolverá al IDE de Visual Studio, con el nuevo proyecto creado y visible en el panel **Explorador de soluciones**.

   ![Proyecto HelloWorld: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-helloworld.png)

Ahora seleccione la arquitectura de la plataforma de destino y el proyecto de inicio. En la barra de herramientas de Visual Studio, busque el cuadro desplegable **Plataformas de solución**. (Si no lo ve, elija **Ver** > **Barra de herramientas** > **Estándar** para mostrar la barra de herramientas que contiene **Plataformas de solución**). Si está ejecutando Windows de 64 bits, elija **x64** en el cuadro desplegable. Windows de 64 bits puede ejecutar también aplicaciones de 32 bits, por lo que puede elegir **x86**, si lo prefiere. En el cuadro desplegable **Proyectos de inicio**, establezca helloworld.UWP (Windows universal).

### <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

Instale el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y haga referencia al SDK de Voz en el proyecto:

1. En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Manage NuGet Packages for Solution** (Administrar paquetes NuGet para la solución) para ir a la ventana **NuGet: solución**.

1. Seleccione **Examinar**.

   ![Captura de pantalla del cuadro de diálogo Administrar paquetes para la solución](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. En **Origen del paquete**, elija **nuget.org**.

1. En el cuadro de búsqueda **Buscar**, escriba `Microsoft.CognitiveServices.Speech` y, a continuación, elija el paquete cuando aparezca en los resultados de la búsqueda.

   ![Captura de pantalla del cuadro de diálogo Administrar paquetes para la solución](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-nuget-install.png)
   > Nota: la biblioteca de iOS dentro del paquete NuGet Microsoft.CognitiveServices.Speech no tiene bitcode habilitado. En caso de que necesite la biblioteca habilitada para bitcode en la aplicación, use el paquete NuGet Microsoft.CognitiveServices.Speech.Xamarin.iOS específicamente para el proyecto de iOS.

1. En el panel de estado del paquete situado junto a los resultados de la búsqueda, seleccione todos los proyectos. **helloworld**, **helloworld.Android**, **helloworld.iOS** y **helloworld.UWP**.

1. Seleccione **Instalar**.

1. En el cuadro de diálogo **Vista previa de los cambios**, seleccione **Aceptar**.

1. En el cuadro de diálogo **Aceptación de licencia**, consulte la licencia; después, seleccione **Acepto** e instale la referencia del paquete del SDK de Voz en todos los proyectos. Una vez completada correctamente la instalación, es posible que vea la siguiente advertencia para helloworld.iOS. Este es un problema conocido y no debe afectar a la funcionalidad de la aplicación.

> No se pudo resolver la referencia "C:\Users\Default\.nuget\packages\microsoft.cognitiveservices.speech\1.7.0\build\Xamarin.iOS\libMicrosoft.CognitiveServices.Speech.core.a". Si el código requiere esta referencia, pueden aparecer errores de compilación.
