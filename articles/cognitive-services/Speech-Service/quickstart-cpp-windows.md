---
title: 'Inicio rápido: Reconocimiento de voz, C++ (Windows): servicios de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en C++ para Windows mediante el SDK de Speech Service
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: wolfma
ms.openlocfilehash: 454b84f4ba5e492f61daf67c980b204c82a2090d
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53075194"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en C++ en Windows mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, creará una aplicación de consola de C++ para Windows. El [SDK de Voz](speech-sdk.md) de Cognitive Services se usa para transcribir la conversión de voz en texto en tiempo real desde el micrófono de un equipo. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción del servicio Voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita del servicio Voz](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

1. Inicie Visual Studio 2017.

1. Asegúrese de que la carga de trabajo **Desarrollo para el escritorio con C++** está disponible. Elija **Herramientas** > **Get Tools and Features (Obtener herramientas y características)** desde la barra de menús de Visual Studio para abrir el instalador de Visual Studio. Si ya está habilitada esta carga de trabajo, vaya al paso siguiente.

    ![Captura de pantalla de la pestaña Cargas de trabajo de Visual Studio](media/sdk/vs-enable-cpp-workload.png)

    Si no es así, active la casilla junto a **Desarrollo para el escritorio con C++**.

1. Asegúrese de que el componente **Administrador de paquetes NuGet** está disponible. Cambie a la pestaña **Componentes individuales** del cuadro de diálogo del instalador de Visual Studio y seleccione **Administrador de paquetes NuGet** si todavía no está habilitado.

      ![Captura de pantalla de la pestaña Componentes individuales de Visual Studio](media/sdk/vs-enable-nuget-package-manager.png)

1. Si necesita habilitar NuGet o la carga de trabajo de C++, seleccione **Modificar** (en la esquina inferior derecha del cuadro de diálogo). La instalación de las nuevas características tardará un momento. Si ambas características ya estaban habilitadas, cierre el cuadro de diálogo.

1. Cree una aplicación de consola para escritorio de Windows en Visual C++. En primer lugar, elija **Archivo** > **Nuevo** > **Proyecto** en el menú. En el cuadro de diálogo **Nuevo proyecto**, expanda **Instalado** > **Visual C++** > **Escritorio de Windows** en el panel de la izquierda. Luego seleccione **Aplicación de consola de Windows**. Para el nombre del proyecto, escriba *helloworld*.

    ![Captura de pantalla del cuadro de diálogo Nuevo proyecto](media/sdk/qs-cpp-windows-01-new-console-app.png)

1. Si está ejecutando Windows de 64 bits, puede cambiar la plataforma de compilación `x64` mediante el menú desplegable en la barra de herramientas de Visual Studio. (Las versiones de Windows de 64 bits pueden ejecutar aplicaciones de 32 bits, por lo que esto no es requisito).

    ![Captura de pantalla de la barra de herramientas de Visual Studio, con la opción x64 resaltada](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. En el Explorador de soluciones, haga clic con el botón derecho en la solución y elija **Administrar paquetes de NuGet para la solución**.

    ![Captura de pantalla del Explorador de soluciones, con la opción Administrar paquetes NuGet para la solución resaltada](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. En la esquina superior derecha, en el campo **Origen del paquete**, seleccione **nuget.org**. Busque el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

    ![Captura de pantalla del cuadro de diálogo Administrar paquetes para la solución](media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > La versión actual del SDK de Speech de Cognitive Services es `1.1.0`.

1. Acepte la licencia que aparece para comenzar la instalación del paquete de NuGet.

    ![Captura de pantalla del cuadro de diálogo Aceptación de licencia](media/sdk/qs-cpp-windows-05-nuget-license.png)

Después de instalar el paquete, aparecerá una confirmación en la consola del Administrador de paquetes.

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra el archivo de origen *helloworld.cpp*. Reemplace todo el código que aparece debajo de la instrucción include inicial (`#include "stdafx.h"` o `#include "pch.h"`) por lo siguiente:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

   ![Captura de pantalla de la aplicación de Visual Studio, con la opción Compilar solución resaltada](media/sdk/qs-cpp-windows-06-build.png)

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

   ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Aparece una ventana de consola que le pide decir algo. Diga una oración o frase en inglés. Lo que diga se transmitirá al servicio Voz y se transcribirá en texto, que aparece en la misma ventana.

   ![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/cpp-windows`.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Reconocimiento de intenciones a partir de contenido de voz con el SDK de Voz para C++](how-to-recognize-intents-from-speech-cpp.md)

## <a name="see-also"></a>Otras referencias

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
