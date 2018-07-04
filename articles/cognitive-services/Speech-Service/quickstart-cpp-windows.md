---
title: Guía de inicio rápido de SDK de Speech para C++ y Windows | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar el SDK de Speech con Windows y C# en Cognitive Services rápidamente.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 0bcdc3c4357cb8985fad16c607957bffad4a2b8c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049237"
---
# <a name="quickstart-for-c-and-windows"></a>Guía de inicio rápido para C++ y Windows

La versión actual del SDK de Speech de Cognitive Services es `0.4.0`.

Se describe cómo crear una aplicación de consola basada en C++ para escritorio de Windows que use el SDK de Speech.
La aplicación se basa en el [paquete NuGet del SDK de Microsoft Cognitive Services](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech) y Microsoft Visual Studio 2017.

> [!NOTE]
> Si busca un inicio rápido para C++ y Linux, vaya [aquí](quickstart-cpp-linux.md).<br>
> Si busca un inicio rápido para C# y Windows, vaya [aquí](quickstart-csharp-windows.md).

> [!NOTE]
> Esta guía de inicio rápido requiere un PC con un micrófono operativo.<br>
> Para ver un ejemplo en el que se reconoce voz a partir de un archivo de audio de entrada determinado, consulte este [ejemplo](speech-to-text-sample.md#speech-recognition-from-a-file).

> [!NOTE]
> Asegúrese de que la instalación de Visual Studio incluye la carga de trabajo **"Desarrollo para el escritorio con C++"**.
> Si no está seguro, siga estos pasos para comprobar y corregir: en Visual Studio 2017, seleccione **Herramientas** \> **Obtener herramientas y características** y confirme la solicitud del control de cuentas de usuario eligiendo **Sí**.
> En la pestaña **Cargas de trabajo**, si **Desarrollo para el escritorio con C++** no tiene una casilla activada junto a ella, actívela y haga clic en **Modificar** para guardar los cambios.

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

## <a name="creating-an-empty-console-application-project"></a>Creación de un proyecto de aplicación de consola vacío

En Visual Studio 2017, cree una nueva aplicación de consola para escritorio de Windows en Visual C++ con el nombre "CppHelloSpeech":

![Creación de una aplicación de consola para escritorio de Windows en Visual C++](media/sdk/speechsdk-05-vs-cpp-new-console-app.png)

Si dispone de una instalación de Windows de 64 bits, puede cambiar, opcionalmente, la plataforma de compilación a `x64`:

![Cambio de la plataforma de compilación a x64](media/sdk/speechsdk-07-vs-cpp-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Instalación y referencia del paquete NuGet del SDK de Speech

> [!NOTE]
> Asegúrese de que el administrador de paquetes NuGet está habilitado para la instalación de Visual Studio de 2017.
> En Visual Studio 2017, seleccione **Herramientas** \> **Obtener herramientas y características** y confirme la solicitud del control de cuentas de usuario eligiendo **Sí**. A continuación, seleccione la pestaña **Componentes individuales** y busque **Administrador de paquetes NuGet** en **Herramientas de código**.
> Si la casilla que está a su izquierda no está activada, no olvide activarla y, después, haga clic en **Modificar** para guardar los cambios.
>
> ![Habilitar el Administrador de paquetes NuGet en Visual Studio ](media/sdk/speechsdk-05-vs-enable-nuget-package-manager.png)

En el Explorador de soluciones, haga clic con el botón derecho en la solución y luego haga clic en **Administrar paquetes de NuGet para la solución...**.

![Haga clic con el botón derecho en Administrar paquetes NuGet para la solución.](media/sdk/speechsdk-09-vs-cpp-manage-nuget-packages.png)

En la esquina superior derecha, en el campo **Origen del paquete**, elija "Nuget.org".
En la pestaña **Examinar**, busque el paquete "Microsoft.CognitiveServices.Speech", selecciónelo y marque las casillas **Proyecto** y **CppHelloSpeech** situadas a la derecha, y seleccione **Instalar** para instalarlo en el proyecto CppHelloSpeech.

![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](media/sdk/speechsdk-11-vs-cpp-manage-nuget-install.png)

En la pantalla de licencia que aparece, acepte la licencia:

![Aceptación de la licencia](media/sdk/speechsdk-12-vs-cpp-manage-nuget-license.png)

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

Reemplace el código de inicio predeterminado por el siguiente:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-cpp/CppHelloSpeech.cpp#code)]

> [!IMPORTANT]
> Reemplace la clave de suscripción por una que haya obtenido. <br>
> Reemplace la región por su región en la [API REST de Speech Service](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis), por ejemplo, reemplace por "Oeste de EE. UU".

![Incorporación de la clave de suscripción](media/sdk/sub-key-recognize-speech-cpp.png)

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

El código se debería compilar sin errores ahora:

![Compilación correcta](media/sdk/speechsdk-16-vs-cpp-build.png)

Inicie el programa en el depurador con el botón Inicio o mediante el método abreviado de teclado F5:

![Inicio de la aplicación en depuración](media/sdk/speechsdk-17-vs-cpp-f5.png)

Aparecerá una ventana emergente de consola que le pide que diga algo (en inglés).
El resultado del reconocimiento se mostrará en la pantalla.

![Salida de la consola después de un reconocimiento correcto](media/sdk/speechsdk-18-vs-cpp-console-output-release.png)

## <a name="downloading-the-sample"></a>Descarga del ejemplo

Para obtener el conjunto más reciente de ejemplos, consulte el [repositorio de GitHub de ejemplos del SDK de Speech de Cognitive Services](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Pasos siguientes

* Visite la [página de ejemplos](samples.md) para obtener más ejemplos.
