---
title: 'Guía de inicio rápido: Creación de una aplicación de Unity para iOS con Azure Spatial Anchors | Microsoft Docs'
description: En este inicio rápido, aprenderá a compilar una aplicación iOS con Unity mediante Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 37856c0833ecde1478d4bd588b8e3122e8eac0ca
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135216"
---
# <a name="quickstart-create-a-unity-ios-app-with-azure-spatial-anchors"></a>Inicio rápido: Creación de una aplicación de Unity para iOS con Azure Spatial Anchors

En este inicio rápido se describe cómo crear una aplicación de Unity para iOS con [Azure Spatial Anchors](../overview.md). Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación iOS de ARKit compilada con Unity que puede guardar y recuperar un delimitador espacial.

Aprenderá a:

> [!div class="checklist"]
> * Crear una cuenta de Spatial Anchors
> * Preparar la configuración de compilación de Unity
> * Descargar e importar el complemento ARKit de Unity
> * Configurar la clave y el identificador de la cuenta de Spatial Anchors
> * Exportación del proyecto de Xcode
> * Implementarlo y ejecutarlo en un dispositivo iOS

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

- Una máquina macOS que tenga instalados <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3+</a>, <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode 10</a> y <a href="https://cocoapods.org" target="_blank">CocoaPods</a>.
- GIT instalado mediante HomeBrew. Escriba el siguiente comando en una sola línea de Terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. A continuación, ejecute `brew install git`.
- Un dispositivo iOS <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">compatible con ARKit</a> habilitado para el desarrollo.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Descarga y apertura del proyecto de Unity de ejemplo

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [iOS Unity Build Settings](../../../includes/spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

En el panel **Project** (Proyecto), vaya a `Assets/AzureSpatialAnchorsPlugin/Examples` y abra el archivo de escena `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Para guardar la escena, seleccione **File** -> **Save** (Archivo > Guardar).

## <a name="export-the-xcode-project"></a>Exportación del proyecto de Xcode

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

[!INCLUDE [Configure Xcode](../../../includes/spatial-anchors-unity-ios-xcode.md)]

Siga las instrucciones de la aplicación para colocar y recuperar un delimitador.

> [!NOTE]
> Cuando se ejecuta la aplicación, si no ve la cámara como fondo (en cambio ve, por ejemplo, una textura negra, azul o de otro tipo), es probable que deba volver a importar los recursos en Unity. Detenga la aplicación. En el menú superior de Unity, elija **Assets -> Re-import all** (Recursos -> Volver a importar todo). Luego, vuelva a ejecutar la aplicación.

En Xcode, presione **Stop** (Detener) para detener la aplicación.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](../tutorials/tutorial-share-anchors-across-devices.md)
