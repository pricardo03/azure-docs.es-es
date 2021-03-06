---
title: 'Inicio rápido: Configuración de la plataforma del SDK de Voz para .NET Framework (Windows): servicio Voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para configurar la plataforma para usar C# en .NET Framework y Windows con el SDK del servicio de voz.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/10/2019
ms.author: erhopf
ms.openlocfilehash: db00398efa638ac945bc188ad0c44f904059e689
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75468496"
---
En esta guía se muestra cómo instalar el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para .NET Framework en Windows.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Prerequisites

Esta guía de inicio rápido requiere:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-visual-studio-project-and-install-the-speech-sdk"></a>Creación de un proyecto de Visual Studio e instalación del SDK de Voz

El siguiente paso consiste en instalar el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) para que pueda hacer referencia a él en el código. Para ello, primero es necesario crear un proyecto **helloworld**. Si ya tiene un proyecto con la carga de trabajo de **desarrollo de escritorio de .NET** disponible, puede usar ese proyecto y pasar directamente a [Uso del Administrador de paquetes NuGet para instalar el SDK de Voz](#use-nuget-package-manager-to-install-the-speech-sdk).

### <a name="create-helloworld-project"></a>Creación del proyecto helloworld

1. Abra Visual Studio 2019.

1. En la ventana Inicio, seleccione **Crear un proyecto**. 

1. En la ventana **Crear un proyecto**, elija **Aplicación de consola (.NET Framework)** y seleccione **Siguiente**.

1. En la ventana **Configure su nuevo proyecto**, escriba *helloworld* en **Nombre del proyecto** , elija o cree la ruta de acceso del directorio en **Ubicación** y seleccione **Crear**.

1. En la barra de menús de Visual Studio, seleccione **Herramientas** > **Obtener herramientas y características**, que abre el Instalador de Visual Studio y muestra el cuadro de diálogo **Modificando**.

1. Compruebe si la carga de trabajo **Desarrollo de escritorio de .NET** está disponible. Si la carga de trabajo aún no se ha instalado, active la casilla que hay al lado y seleccione **Modificar** para iniciar la instalación. La descarga e instalación pueden tardar unos minutos.

   Si la casilla que está junto a **Desarrollo de escritorio de .NET** ya está seleccionada, seleccione **Cerrar** para salir del cuadro de diálogo.

   ![Habilitación del desarrollo de escritorio .NET](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Cierre el Instalador de Visual Studio.

### <a name="use-nuget-package-manager-to-install-the-speech-sdk"></a>Uso del Administrador de paquetes Nuget para instalar el SDK de Voz

1. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **helloworld** y seleccione **Administrar paquetes NuGet** para mostrar el Administrador de paquetes NuGet.

   ![Administrador de paquetes NuGet](~/articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. En la esquina superior derecha, busque el cuadro desplegable **Origen del paquete** y asegúrese de que **`nuget.org`** está seleccionado.

1. En la esquina superior izquierda, seleccione **Examinar**.

1. En el cuadro de búsqueda, escriba *Microsoft.CognitiveServices.Speech* y seleccione **Entrar**.

1. En los resultados de la búsqueda, seleccione el paquete **Microsoft.CognitiveServices.Speech** y, después, seleccione **Instalar** para instalar la versión estable más reciente.

   ![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Acepte todos los contratos y licencias para iniciar la instalación.

   Después de instalar el paquete aparecerá una confirmación en la ventana **Consola del administrador de paquetes**.

Ahora puede continuar con la sección [Pasos siguientes](#next-steps).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [windows](../quickstart-list.md)]
