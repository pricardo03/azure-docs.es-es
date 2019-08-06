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
ms.openlocfilehash: 24a3424a73fb21530f3cde227aa9f05f16bd6ad0
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562456"
---
# <a name="quickstart-create-a-unity-android-app-with-azure-spatial-anchors"></a>Inicio rápido: Creación de una aplicación para Unity Android con Azure Spatial Anchors

En este inicio rápido se describe cómo crear una aplicación para Unity Android con [Azure Spatial Anchors](../overview.md). Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación Android de ARCore compilada con Unity que puede guardar y recuperar un delimitador espacial.

Aprenderá a:

> [!div class="checklist"]
> * Crear una cuenta de Spatial Anchors
> * Preparar la configuración de compilación de Unity
> * Configurar la clave y el identificador de la cuenta de Spatial Anchors
> * Exportar el proyecto de Android Studio
> * Realizar la implementación y ejecución en un dispositivo Android

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

- Una máquina Windows o MacOS <a href="https://unity3d.com/get-unity/download" target="_blank">con Unity 2019.1 o superior</a>, incluidos los módulos Android Build Support y Android SDK & NDK Tools, así como <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.4 o superior</a>.
  - Si se ejecuta en Windows, también necesitará <a href="https://git-scm.com/download/win" target="_blank">Git para Windows</a>.
  - Si se ejecuta en macOS, instale Git a través de HomeBrew. Escriba el siguiente comando en una sola línea de Terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. A continuación, ejecute `brew install git`.
- Un dispositivo Android <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">habilitado para el desarrollo</a> y <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">preparado para ARCore</a>.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Descarga y apertura del proyecto de Unity de ejemplo

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

En el panel **Project** (Proyecto), vaya a `Assets/AzureSpatialAnchors.Examples/Scenes` y abra el archivo de escena `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Para guardar la escena, seleccione **File** -> **Save** (Archivo > Guardar).

## <a name="export-the-android-studio-project"></a>Exportación del proyecto de Android Studio

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Seleccione el dispositivo en **Run Device** (Dispositivo de ejecución) y haga clic en **Build And Run** (Compilar y ejecutar). Se le pedirá que guarde un archivo `.apk` para el que puede elegir cualquier nombre.

Siga las instrucciones de la aplicación para colocar y recuperar un delimitador.

## <a name="troubleshooting"></a>solución de problemas

Cuando se ejecuta la aplicación, si no ve la cámara como fondo (en cambio ve, por ejemplo, una textura negra, azul o de otro tipo), es probable que deba volver a importar los recursos en Unity. Detenga la aplicación. En el menú superior de Unity, elija **Assets -> Reimport all** (Recursos -> Volver a importar todo). Luego, vuelva a ejecutar la aplicación.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](../tutorials/tutorial-share-anchors-across-devices.md)
