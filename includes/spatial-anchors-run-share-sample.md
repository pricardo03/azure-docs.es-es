---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: b46a2b18309851bbe2934980137a53d2de6f6efc
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135343"
---
## <a name="set-up-your-device-in-unity"></a>Configuración del dispositivo en Unity

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

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

En **Scenes In Build** (Escenas en compilación), asegúrese de que haya una marca de verificación junto a cada escena.

Asegúrese de que **Export Project** (Exportar proyecto) no tiene una marca de verificación. Seleccione **Build And Run** (Compilar y ejecutar). Se le pedirá que guarde el archivo `.apk`. Puede elegir cualquier nombre.

Una vez que se inicia la aplicación, en el cuadro de diálogo **Choose A Demo** (Elegir una demostración), use las flechas izquierda y derecha para seleccionar la opción **LocalShare** y pulse **Go!** (Ir). Siga las instrucciones de la aplicación. Puede seleccionar **Create & Share Anchor** (Crear y compartir delimitador) o **Locate Shared Anchor** (Buscar delimitador anclado).

El primer escenario permite crear un delimitador que se puede encontrar más adelante tanto en el mismo dispositivo como en otro.
Si ya ha ejecutado la aplicación en el mismo dispositivo o en otro, el segundo escenario permite buscar delimitadores compartidos previamente. Después de elegir el escenario, la aplicación le guiará con instrucciones adicionales. Por ejemplo, se le pedirá mover el dispositivo para recopilar información del entorno. Más adelante, colocará un delimitador en el mundo, esperará para guardarlo y así sucesivamente.

### <a name="deploy-to-an-ios-device"></a>Implementación en un dispositivo iOS

Seleccione **File** > **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En **Scenes In Build** (Escenas en compilación), asegúrese de que haya una marca de verificación junto a cada escena.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Una vez que se inicia la aplicación, en el cuadro de diálogo **Choose A Demo** (Elegir una demostración), use las flechas izquierda y derecha para seleccionar la opción **LocalShare** y pulse **Go!** (Ir). Siga las instrucciones de la aplicación. Puede seleccionar **Create & Share Anchor** (Crear y compartir delimitador) o **Locate Shared Anchor** (Buscar delimitador anclado).

El primer escenario permite crear un delimitador que se puede encontrar más adelante tanto en el mismo dispositivo como en otro.
Si ya ha ejecutado la aplicación en el mismo dispositivo o en otro, el segundo escenario permite buscar delimitadores compartidos previamente. Después de elegir el escenario, la aplicación le guiará con instrucciones adicionales. Por ejemplo, se le pedirá mover el dispositivo para recopilar información del entorno. Más adelante, colocará un delimitador en el mundo, esperará para guardarlo y así sucesivamente.

En Xcode, seleccione **Stop** (Detener) para detener la aplicación.
