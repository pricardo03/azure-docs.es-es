---
title: 'Inicio rápido: Creación de una aplicación para Unity Android con Azure Spatial Anchors | Microsoft Docs'
description: En este tutorial, aprenderá a compilar una aplicación Android con Unity mediante Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 642abfb99b40d67802b7194ad225ebcd2872a72b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135082"
---
# <a name="quickstart-create-a-unity-android-app-with-azure-spatial-anchors"></a>Inicio rápido: Creación de una aplicación para Unity Android con Azure Spatial Anchors

En este inicio rápido se describe cómo crear una aplicación para Unity Android con [Azure Spatial Anchors](../overview.md). Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación Android de ARCore compilada con Unity que puede guardar y recuperar un delimitador espacial.

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

- Un equipo con Windows o macOS con <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3+</a> y <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3+</a>.
  - Si se ejecuta en Windows, también necesitará <a href="https://git-scm.com/download/win" target="_blank">Git para Windows</a>.
  - Si se ejecuta en macOS, instale Git a través de HomeBrew. Escriba el siguiente comando en una sola línea de Terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. A continuación, ejecute `brew install git`.
- Un dispositivo Android <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">habilitado para el desarrollo</a> y <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">preparado para ARCore</a>.
- La aplicación debe usar la versión **1.7** del SDK de ARCore para Unity.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Descarga y apertura del proyecto de Unity de ejemplo

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

En el panel **Project** (Proyecto), vaya a `Assets/AzureSpatialAnchorsPlugin/Examples` y abra el archivo de escena `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Para guardar la escena, seleccione **File** -> **Save** (Archivo > Guardar).

## <a name="export-the-android-studio-project"></a>Exportación del proyecto de Android Studio

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Asegúrese de que la casilla **Export Project** (Exportar proyecto) no está activada. Haga clic en **Build And Run** (Compilar y ejecutar). Se le pedirá que guarde su archivo `.apk`; puede elegir cualquier nombre para él.

Siga las instrucciones de la aplicación para colocar y recuperar un delimitador.

> [!NOTE]
> Cuando se ejecuta la aplicación, si no ve la cámara como fondo (en cambio ve, por ejemplo, una textura negra, azul o de otro tipo), es probable que deba volver a importar los recursos en Unity. Detenga la aplicación. En el menú superior de Unity, elija **Assets -> Reimport all** (Recursos -> Volver a importar todo). Luego, vuelva a ejecutar la aplicación.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](../tutorials/tutorial-share-anchors-across-devices.md)
