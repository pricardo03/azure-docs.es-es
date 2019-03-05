---
title: 'Inicio rápido: Creación de una aplicación de Unity para Android con Azure Spatial Anchors | Microsoft Docs'
description: En este tutorial, aprenderá a compilar una aplicación Android con Unity mediante Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: e92c812ffc8b72fe79248c602e48ff01ef9fefcb
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961021"
---
# <a name="quickstart-create-an-android-unity-app-with-azure-spatial-anchors"></a>Inicio rápido: Creación de una aplicación de Unity para Android con Azure Spatial Anchors

En este inicio rápido se describe cómo crear una aplicación de Unity para Android con [Azure Spatial Anchors](../overview.md). Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación Android de ARCore compilada con Unity que puede guardar y recuperar un delimitador espacial.

Aprenderá a:

> [!div class="checklist"]
> * Crear una cuenta de Spatial Anchors
> * Preparar la configuración de compilación de Unity
> * Descargar e importar el SDK de ARCore para Unity
> * Configurar la clave y el identificador de la cuenta de Spatial Anchors
> * Exportar el proyecto de Android Studio
> * Realizar la implementación y ejecución en un dispositivo Android

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

- Un equipo con Windows o macOS con <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3</a> y <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3</a>, como mínimo, instalados.
- Un dispositivo Android <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">habilitado para el desarrollo</a> y <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">preparado para ARCore</a>.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Apertura del proyecto de ejemplo en Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

En el panel **Project** (Proyecto), vaya a `Assets/AzureSpatialAnchorsPlugin/Examples` y abra el archivo de escena `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Para guardar la escena, seleccione **File** -> **Save** (Archivo > Guardar).

## <a name="export-the-android-studio-project"></a>Exportación del proyecto de Android Studio

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Seleccione **Exportar** para abrir un cuadro de diálogo. A continuación, seleccione una carpeta para exportar el proyecto de Android Studio.

Cuando se haya completado la exportación, se mostrará una carpeta con el proyecto de Android Studio exportado y con una subcarpeta llamada **HelloAR U3D**.

## <a name="deploy-the-android-application"></a>Actualización de la aplicación Android

Abra Android Studio y seleccione **Open an existing Android Studio project** (Abrir un proyecto existente de Android Studio). Luego, seleccione la subcarpeta **HelloAR U3D** desde el proyecto de Android Studio exportado y haga clic en **OK** (Aceptar).

Al abrirse, aparecerá un mensaje en el que se pregunta si se desea usar el contenedor de Gradle. Seleccione **OK** (Aceptar) para usar el contenedor Gradle y para abrir el proyecto.

Encienda el dispositivo Android, inicie sesión y conéctelo al equipo mediante un cable USB.

Seleccione **Run** (Ejecutar) en la barra de herramientas de Android Studio.

![Implementación y ejecución de Android Studio](./media/get-started-unity-android/android-studio-deploy-run.png)

Seleccione el dispositivo Android en el cuadro de diálogo **Select Deployment Target** (Seleccionar destino de implementación) y seleccione **OK** (Aceptar) para ejecutar la aplicación en el dispositivo Android.

Siga las instrucciones de la aplicación para colocar y recuperar un delimitador.

> [!NOTE]
> Cuando se ejecuta la aplicación, si no ve la cámara como fondo (en cambio ve, por ejemplo, una textura negra, azul o de otro tipo), es probable que deba volver a importar los recursos en Unity. Detenga la aplicación. En el menú superior de Unity, elija **Assets -> Reimport all** (Recursos -> Volver a importar todo). Luego, vuelva a ejecutar la aplicación.

Detenga la aplicación, para lo que debe seleccionar **Stop** (Detener) en la barra de herramientas de Android Studio.

![Stop (Detener) en Android Studio](./media/get-started-unity-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](../tutorials/tutorial-share-anchors-across-devices.md)
