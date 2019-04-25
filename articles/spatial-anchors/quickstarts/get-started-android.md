---
title: 'Inicio rápido: Creación de una aplicación para Android con Azure Spatial Anchors | Microsoft Docs'
description: En este artículo de inicio rápido aprenderá a compilar una aplicación para Android mediante Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 003e9beb3a319802754397efae3b636f51de9e52
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995904"
---
# <a name="quickstart-create-an-android-app-with-azure-spatial-anchors"></a>Inicio rápido: Creación de una aplicación para Android con Azure Spatial Anchors

En esta guía de inicio rápido se describe cómo crear una aplicación para Android con [Azure Spatial Anchors](../overview.md) en Java o en C++/NDK. Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación para Android de ARCore que puede guardar y recuperar un delimitador espacial.

Aprenderá a:

> [!div class="checklist"]
> * Crear una cuenta de Spatial Anchors
> * Configurar la clave y el identificador de la cuenta de Spatial Anchors
> * Realizar la implementación y ejecución en un dispositivo Android

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

- Una máquina con Windows o macOS con <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3</a> como mínimo.
  - Si se ejecuta en Windows, también necesitará <a href="https://git-scm.com/download/win" target="_blank">Git para Windows</a>.
  - Si se ejecuta en macOS, instale Git a través de HomeBrew. Escriba el siguiente comando en una sola línea de Terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. A continuación, ejecute `brew install git`.
  - Para compilar el ejemplo de NDK, también deberá instalar el NDK y CMake 3.6 SDK Tools en Android Studio.
- Un dispositivo Android <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">habilitado para el desarrollo</a> y <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">preparado para ARCore</a>.
- La aplicación debe tener como destino ARCore 1.7.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Apertura del proyecto de ejemplo

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Si va a compilar el ejemplo del NDK de Android, deberá descargar `arcore_c_api.h` desde [aquí](https://raw.githubusercontent.com/google-ar/arcore-android-sdk/v1.7.0/libraries/include/arcore_c_api.h) y colocarlo en `Android\NDK\libraries\include`.

Abra Android Studio.

# <a name="javatabopenproject-java"></a>[Java](#tab/openproject-java)

Seleccione **Open an existing Android Studio project** (Abrir un proyecto existente de Android Studio) y el proyecto que se encuentra en `Android/Java/`.

# <a name="ndktabopenproject-ndk"></a>[NDK](#tab/openproject-ndk)

Seleccione **Open an existing Android Studio project** (Abrir un proyecto existente de Android Studio) y el proyecto que se encuentra en `Android/NDK/`.

***

## <a name="configure-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

El paso siguiente es configurar la aplicación para usar el identificador de cuenta y la clave de cuenta. Los copió en un editor cuando [configuró el recurso de Spatial Anchors](#create-a-spatial-anchors-resource).

# <a name="javatabopenproject-java"></a>[Java](#tab/openproject-java)

Abra `Android/Java/app/src/main/java/com/microsoft/sampleandroid/AzureSpatialAnchorsActivity.java`.

Busque el campo `SpatialAnchorsAccountKey` y reemplace `Set me` por la clave de la cuenta.

Busque el campo `SpatialAnchorsAccountId` y reemplace `Set me` por el identificador de la cuenta.

# <a name="ndktabopenproject-ndk"></a>[NDK](#tab/openproject-ndk)

Abra `Android/NDK/app/src/main/cpp/spatial_services_application.cc`.

Busque el campo `SpatialAnchorsAccountKey` y reemplace `Set me` por la clave de la cuenta.

Busque el campo `SpatialAnchorsAccountId` y reemplace `Set me` por el identificador de la cuenta.

***

## <a name="deploy-the-app-to-your-android-device"></a>Implementación de la aplicación en el dispositivo Android

Encienda el dispositivo Android, inicie sesión y conéctelo al equipo mediante un cable USB.

Seleccione **Run** (Ejecutar) en la barra de herramientas de Android Studio.

![Implementación y ejecución de Android Studio](./media/get-started-android/android-studio-deploy-run.png)

Seleccione el dispositivo Android en el cuadro de diálogo **Select Deployment Target** (Seleccionar destino de implementación) y seleccione **OK** (Aceptar) para ejecutar la aplicación en el dispositivo Android.

Siga las instrucciones de la aplicación para colocar y recuperar un delimitador.

Detenga la aplicación, para lo que debe seleccionar **Stop** (Detener) en la barra de herramientas de Android Studio.

![Stop (Detener) en Android Studio](./media/get-started-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](../tutorials/tutorial-share-anchors-across-devices.md)
