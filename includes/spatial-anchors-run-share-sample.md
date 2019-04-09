---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 397a8a9b07b4d7a88d0345399ac4abcc3e738a82
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631184"
---
## <a name="set-up-your-device"></a>Configuración del dispositivo

En Unity, abra el proyecto en la carpeta `Unity`.

![Ventana de Unity](./media/spatial-anchors-unity/unity-window.png)

### <a name="set-up-an-android-device"></a>Configuración de un dispositivo Android

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>Configuración de un dispositivo iOS

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

En el panel **Project** (Proyecto), vaya a `Assets/AzureSpatialAnchorsPlugin/Examples` y abra el archivo de escena `AzureSpatialAnchorsLocalSharedDemo.unity`.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

En el panel **Inspector**, escriba `Sharing Anchors Service url` (de la implementación en Azure de la aplicación web de ASP.NET) como valor de `Base Sharing Url`, que reemplaza `index.html` por `api/anchors`. Debería ser parecido a este: `https://<app_name>.azurewebsites.net/api/anchors`.

Para guardar la escena, seleccione **File** > **Save** (Archivo > Guardar).

## <a name="deploy-to-your-device"></a>Implementación en el dispositivo

### <a name="deploy-to-android-device"></a>Implementación en un dispositivo Android

Inicie sesión en el dispositivo Android y conéctelo a un equipo mediante un cable USB.

Seleccione **File** > **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En **Scenes In Build** (Escenas en compilación), coloque una marca de verificación junto a la escena `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` y borre las marcas de las restantes escenas.

Asegúrese de que **Export Project** (Exportar proyecto) no tiene una marca de verificación. Seleccione **Build And Run** (Compilar y ejecutar). Se le pedirá que guarde el archivo `.apk`. Puede elegir cualquier nombre.

Siga las instrucciones de la aplicación. Puede seleccionar **Create & Share Anchor** (Crear y compartir delimitador) o **Locate Shared Anchor** (Buscar delimitador anclado). El primer escenario permite crear un delimitador que se puede encontrar más adelante tanto en el mismo dispositivo como en otro. Si ya ha ejecutado la aplicación en el mismo dispositivo o en otro, el segundo escenario permite buscar delimitadores compartidos previamente. Después de elegir el escenario, la aplicación le guiará con instrucciones adicionales. Por ejemplo, se le pedirá que mueva el dispositivo para recopilar información del entorno. Más adelante, colocará un delimitador en el mundo, esperará mientras carga y así sucesivamente.

### <a name="deploy-to-an-ios-device"></a>Implementación en un dispositivo iOS

Seleccione **File** > **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En **Scenes In Build** (Escenas en compilación), coloque una marca de verificación junto a la escena `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` y borre las marcas de las restantes escenas.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Siga las instrucciones de la aplicación. Puede seleccionar **Create & Share Anchor** (Crear y compartir delimitador) o **Locate Shared Anchor** (Buscar delimitador anclado). El primer escenario permite crear un delimitador que se puede encontrar más adelante tanto en el mismo dispositivo como en otro. Si ya ha ejecutado la aplicación en el mismo dispositivo o en otro, el segundo escenario permite buscar delimitadores compartidos previamente. Después de elegir el escenario, la aplicación le guiará con instrucciones adicionales. Por ejemplo, se le pedirá que mueva el dispositivo para recopilar información del entorno. Más adelante, colocará un delimitador en el mundo, esperará mientras carga y así sucesivamente.

En Xcode, seleccione **Stop** (Detener) para detener la aplicación.
