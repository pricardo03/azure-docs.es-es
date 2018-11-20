---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2018
ms.author: wolfma
ms.openlocfilehash: 7a028f65467c517be59be6f0c6d2c45ef9a7685e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285779"
---
1. Inicie Visual Studio 2017.

1. En la barra de menús de Visual Studio, seleccione **Herramientas > Obtener herramientas** y asegúrese de que la carga de trabajo **Desarrollo de escritorio de .NET** está disponible. Si aún no se ha instalado la carga de trabajo, active la casilla y haga clic en **Modificar** para iniciar la instalación. La descarga e instalación pueden tardar unos minutos.

   Si la casilla que está junto a **Desarrollo de escritorio de .NET** está seleccionada, puede cerrar el cuadro de diálogo ahora.

   ![Habilitación del desarrollo de escritorio .NET](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. A continuación, vamos a crear un proyecto. En la barra de menús, seleccione **Archivo > Nuevo > Proyecto**. Cuando aparezca el cuadro de diálogo, en el panel izquierdo expanda las secciones **Instalado > Visual C# > Escritorio de Windows** y seleccione **Aplicación de consola (.NET Framework)**. Asigne el nombre *helloworld* a este proyecto.

    ![Creación de aplicación de consola de Visual C# (.NET Framework)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Creación de aplicación de consola de Visual C# (.NET Framework)")

1. Ahora que el proyecto está configurado, es necesario instalar el [paquete NuGet de Speech SDK](https://aka.ms/csspeech/nuget) y hacer referencia a él en nuestro código. Busque el Explorador de soluciones y haga clic con el botón derecho en helloworld. En el menú, seleccione **Administrar paquetes de NuGet...**.

   ![Hacer clic con el botón derecho en Manage NuGet Packages for Solution (Administrar paquetes NuGet para la solución)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "Manage NuGet Packages for Solution")

1. En la esquina superior derecha del Administrador de paquetes NuGet, busque la lista desplegable **Origen del paquete** y asegúrese de que **nuget.org** está seleccionado. A continuación, seleccione **Examinar**, busque el paquete `Microsoft.CognitiveServices.Speech` e instale la última versión estable.

   ![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png "Instalación del paquete Nuget")

1. Acepte todos los contratos y licencias para iniciar la instalación.

   ![Aceptación de licencia](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Accept the license")

    Después de instalar el paquete, aparecerá una confirmación en la consola del Administrador de paquetes.

1. El siguiente paso es crear una configuración de plataforma que coincida con la arquitectura del equipo que se usa para compilar y ejecutar la aplicación de consola. En la barra de menús, seleccione **Compilar** > **Administrador de configuración...** .

    ![Inicio del Administrador de configuración](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Launch the configuration manager")

1. En el cuadro de diálogo **Administrador de configuración**, busque la lista desplegable **Plataforma de la solución activa** y seleccione **Nuevo**.

    ![Incorporación de una nueva plataforma en la ventana del Administrador de configuración](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Add a new platform under the configuration manager window")

1. Si está ejecutando Windows de 64 bits, cuando se le indique **Escriba o seleccione la nueva plataforma**, seleccione `x64`. Si está ejecutando Windows de 32 bits, seleccione `x86`. Cuando haya terminado, haga clic en **Aceptar**.

    ![En Windows de 64 bits, incorporación de una nueva plataforma denominada "x64"](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "Add x64 platform")
