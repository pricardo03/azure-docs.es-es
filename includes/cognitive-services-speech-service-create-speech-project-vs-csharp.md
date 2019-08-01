---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 308ee2ef121648cb45152948926c5fd7fb934744
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68362616"
---
1. Abra Visual Studio 2019.

1. En la ventana Inicio, seleccione **Crear un proyecto**. 

1. Seleccione **Aplicación de consola (.NET Framework)** y **Siguiente**.

1. En el **nombre del proyecto**, escriba `helloworld` y, a continuación, seleccione **Crear**.

1. En la barra de menús de Visual Studio, seleccione **Herramientas** > **Get Tools and Features** (Obtener herramientas y características) y asegúrese de que la carga de trabajo **Desarrollo de escritorio de .NET** está disponible. Si aún no se ha instalado la carga de trabajo, active la casilla y seleccione **Modificar** para iniciar la instalación. La descarga e instalación pueden tardar unos minutos.

   Si la casilla que está junto a **Desarrollo de escritorio de .NET** está seleccionada, puede cerrar el cuadro de diálogo ahora.

   ![Habilitación del desarrollo de escritorio .NET](../articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

El siguiente paso consiste en instalar el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) para que pueda hacer referencia a él en el código.

1. En el explorador de soluciones, haga clic con el botón derecho en `helloworld` y seleccione **Manage NuGet Packages** (Administrar paquetes NuGet) para mostrar el administrador de paquetes NuGet.

   ![Administrador de paquetes NuGet](../articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. En la esquina superior derecha, busque el cuadro desplegable **Origen del paquete** y asegúrese de que **nuget.org** está seleccionado.

1. En la esquina superior izquierda, seleccione **Examinar**.

1. En el cuadro de búsqueda, escriba `Microsoft.CognitiveServices.Speech` y presione Entrar.

1. Seleccione `Microsoft.CognitiveServices.Speech` e **Instalar** para instalar la versión estable más reciente.

   ![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](../articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Acepte todos los contratos y licencias para iniciar la instalación.

   Después de instalar el paquete aparecerá una confirmación en la ventana **Consola del administrador de paquetes**.

Ahora, para compilar y ejecutar la aplicación de consola, cree una configuración de plataforma que coincida con la arquitectura del equipo.

1. En la barra de menús, seleccione **Compilar** > **Administrador de configuración**. Aparecerá el cuadro de diálogo **Administrador de configuración**.

   ![Cuadro de diálogo Administrador de configuración](../articles/cognitive-services/speech-service/media/sdk/vs-configuration-manager-dialog-box.png)

1. En el cuadro desplegable **Active solution platform** (Plataforma de soluciones activas), seleccione **Nuevo**. Aparecerá el cuadro de diálogo **Nueva plataforma de solución**.

1. En el cuadro desplegable **escriba o seleccione la nueva plataforma**:
   - Si está ejecutando Windows de 64 bits, seleccione **x64**.
   - Si está ejecutando Windows de 32 bits, seleccione **x86**.

1. Seleccione **Aceptar** y, después, **Cerrar**.
