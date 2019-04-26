---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 2/20/2019
ms.author: erhopf
ms.openlocfilehash: b5a24d83faef5a895317f162178f8bd588a1466d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60620153"
---
1. Inicie Visual Studio 2017.

1. Asegúrese de que la carga de trabajo **Desarrollo para el escritorio con C++** está disponible. Elija **Herramientas** > **Get Tools and Features (Obtener herramientas y características)** desde la barra de menús de Visual Studio para abrir el instalador de Visual Studio. Si ya está habilitada esta carga de trabajo, vaya al paso siguiente.

    ![Captura de pantalla de la pestaña Cargas de trabajo de Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-cpp-workload.png)

    Si no es así, active la casilla junto a **Desarrollo para el escritorio con C++**.

1. Asegúrese de que el componente **Administrador de paquetes NuGet** está disponible. Cambie a la pestaña **Componentes individuales** del cuadro de diálogo del instalador de Visual Studio y seleccione **Administrador de paquetes NuGet** si todavía no está habilitado.

      ![Captura de pantalla de la pestaña Componentes individuales de Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-nuget-package-manager.png)

1. Si necesita habilitar NuGet o la carga de trabajo de C++, seleccione **Modificar** (en la esquina inferior derecha del cuadro de diálogo). La instalación de las nuevas características tardará un momento. Si ambas características ya estaban habilitadas, cierre el cuadro de diálogo.

1. Cree una aplicación de consola para escritorio de Windows en Visual C++. En primer lugar, elija **Archivo** > **Nuevo** > **Proyecto** en el menú. En el cuadro de diálogo **Nuevo proyecto**, expanda **Instalado** > **Visual C++** > **Escritorio de Windows** en el panel de la izquierda. Luego seleccione **Aplicación de consola de Windows**. Para el nombre del proyecto, escriba *helloworld*.

    ![Captura de pantalla del cuadro de diálogo Nuevo proyecto](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-01-new-console-app.png)

1. Si está ejecutando Windows de 64 bits, puede cambiar la plataforma de compilación `x64` mediante el menú desplegable en la barra de herramientas de Visual Studio. (Las versiones de Windows de 64 bits pueden ejecutar aplicaciones de 32 bits, por lo que esto no es requisito).

    ![Captura de pantalla de la barra de herramientas de Visual Studio, con la opción x64 resaltada](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. En el Explorador de soluciones, haga clic con el botón derecho en la solución y elija **Administrar paquetes de NuGet para la solución**.

    ![Captura de pantalla del Explorador de soluciones, con la opción Administrar paquetes NuGet para la solución resaltada](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. En la esquina superior derecha, en el campo **Origen del paquete**, seleccione **nuget.org**. Busque el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

    ![Captura de pantalla del cuadro de diálogo Administrar paquetes para la solución](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > La versión actual del SDK de Speech de Cognitive Services es `1.4.0`.

1. Acepte la licencia que aparece para comenzar la instalación del paquete de NuGet.

    ![Captura de pantalla del cuadro de diálogo Aceptación de licencia](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-05-nuget-license.png)

Después de instalar el paquete, aparecerá una confirmación en la consola del Administrador de paquetes.
