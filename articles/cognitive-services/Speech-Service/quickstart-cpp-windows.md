---
title: 'Guía de inicio rápido: Reconocimiento de voz en C++ en el escritorio de Windows con Speech SDK de Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Aprenda a reconocer voz en C++ en el escritorio de Windows con el SDK de Voz de Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 7c93f9f450cc2d244a0d716401171cde02abf70b
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324578"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-desktop-using-the-speech-sdk"></a>Guía de inicio rápido: Reconocimiento de voz en C++ en el escritorio de Windows mediante el SDK de Voz

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Se describe cómo crear una aplicación de consola basada en C++ para escritorio de Windows que use el SDK de Speech.
La aplicación se basa en el [paquete NuGet del SDK de Voz de Microsoft Cognitive Services](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Un PC Windows con micrófono operativo.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition o superior.
* **Desarrollo de escritorio con la carga de trabajo C++** en Visual Studio y componente del **administrador de paquetes NuGet** en Visual Studio.
  Puede habilitarlo tanto en **Herramientas** \> **Get Tools and Features** (Obtener herramientas y características), en las pestañas **Workloads** (Cargas de trabajo) e **Individual components** (Componentes individuales), respectivamente:

  ![Habilitación del desarrollo de escritorio con la carga de trabajo de C++](media/sdk/vs-enable-cpp-workload.png)

  ![Habilitar el Administrador de paquetes NuGet en Visual Studio ](media/sdk/vs-enable-nuget-package-manager.png)

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

En Visual Studio 2017, cree una nueva aplicación de consola para escritorio de Windows en Visual C++. En el cuadro de diálogo **Nuevo proyecto**, en el panel izquierdo, expanda **Instalado** \> **Visual C++** \> **Escritorio de Windows** y, después, seleccione **Aplicación de consola Windows**. Para el nombre del proyecto, escriba *helloworld*.

![Creación de una aplicación de consola para escritorio de Windows en Visual C++](media/sdk/qs-cpp-windows-01-new-console-app.png)

Si dispone de una instalación de Windows de 64 bits, puede cambiar, opcionalmente, la plataforma de compilación a `x64`:

![Cambio de la plataforma de compilación a x64](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Instalación y referencia del paquete NuGet del SDK de Speech

En el Explorador de soluciones, haga clic con el botón derecho en la solución y luego haga clic en **Administrar paquetes de NuGet para la solución...**.

![Haga clic con el botón derecho en Administrar paquetes NuGet para la solución.](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

En la esquina superior derecha, en el campo **Origen del paquete**, elija "Nuget.org".
En la pestaña **Examinar**, busque el paquete "Microsoft.CognitiveServices.Speech", selecciónelo y marque las casillas **Proyecto** y **helloworld** situadas a la derecha, y seleccione **Instalar** para instalarlo en el proyecto helloworld.

> [!NOTE]
> La versión actual del SDK de Speech de Cognitive Services es `0.5.0`.

![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](media/sdk/qs-cpp-windows-04-nuget-install-0.5.0.png)

En la pantalla de licencia que aparece, acepte la licencia:

![Aceptación de la licencia](media/sdk/qs-cpp-windows-05-nuget-license.png)

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Reemplace el código de inicio predeterminado por el siguiente:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Compile la aplicación. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora:

   ![Compilación correcta](media/sdk/qs-cpp-windows-06-build.png)

1. Inicie la aplicación. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**.

   ![Inicio de la aplicación en depuración](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Aparece una ventana emergente de consola que le pide que diga algo (en inglés).
   El resultado del reconocimiento se mostrará en la pantalla.

   ![Salida de la consola después de un reconocimiento correcto](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/cpp-windows`.

## <a name="next-steps"></a>Pasos siguientes

* [Obtener nuestros ejemplos](speech-sdk.md#get-the-samples)
