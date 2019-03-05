---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 5f1e0153b1f919bc9d7e921d2a1b3ae745b2b01f
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753523"
---
## <a name="open-the-sample-project-in-unity"></a>Apertura del proyecto de ejemplo en Unity

[!INCLUDE [Clone Sample Repo](spatial-anchors-clone-sample-repository.md)]

## <a name="to-set-up-for-an-android-device"></a>Para configurar para un dispositivo Android

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

## <a name="to-set-up-for-an-ios-device"></a>Para configurar para un dispositivo iOS

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

En el panel **Project** (Proyecto), vaya a `Assets/AzureSpatialAnchorsPlugin/Examples` y abra el archivo de escena `AzureSpatialAnchorsLocalSharedDemo.unity`.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

Además, en el panel **Inspector**, escriba `Sharing Anchors Service url` (de la implementación en Azure de la aplicación web de ASP.NET) como el valor de `Base Sharing Url`, que reemplaza `index.html` por `api/anchors`. El aspecto será parecido al siguiente: `https://<app_name>.azurewebsites.net/api/anchors`.

Para guardar la escena, seleccione **File** -> **Save** (Archivo > Guardar).

## <a name="to-deploy-to-an-android-device"></a>Para implementar en un dispositivo Android

Encienda el dispositivo Android, inicie sesión y conéctelo al equipo mediante un cable USB.

Seleccione **File** -> **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En **Scenes In Build** (Escenas en compilación), active la escena `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` y desactive todas las demás escenas.

Asegúrese de que la casilla **Export Project** (Exportar proyecto) no está activada. Haga clic en **Build And Run** (Compilar y ejecutar). Se le pedirá que guarde su archivo `.apk`; puede elegir cualquier nombre para él.

Siga las instrucciones de la aplicación. Puede elegir **Create & Share Anchor** (Crear y compartir delimitador) o **Locate Shared Anchor** (Buscar delimitador anclado). La primera opción permite crear un delimitador que se puede encontrar más adelante en el mismo dispositivo o en uno diferente. Si ya ha ejecutado la aplicación, en el mismo dispositivo o en otro, la segunda opción permite buscar delimitadores previamente compartidos.

## <a name="to-deploy-to-an-ios-device"></a>Para implementar en un dispositivo iOS

Seleccione **File** -> **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En **Scenes In Build** (Escenas en compilación), active la escena `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` y desactive todas las demás escenas.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Siga las instrucciones de la aplicación. Puede elegir **Create & Share Anchor** (Crear y compartir delimitador) o **Locate Shared Anchor** (Buscar delimitador anclado). La primera opción permite crear un delimitador que se puede encontrar más adelante en el mismo dispositivo o en uno diferente. Si ya ha ejecutado la aplicación, en el mismo dispositivo o en otro, la segunda opción permite buscar delimitadores previamente compartidos.

En Xcode, presione **Stop** (Detener) para detener la aplicación.
