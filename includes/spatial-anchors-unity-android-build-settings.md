---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 662aced6df27febdf29f2645725962763e89cfa2
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752903"
---
Abra Unity y abra el proyecto en la carpeta `Unity`.

Seleccione **File** -> **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En la sección **Platform** (Plataforma), seleccione **Android**. A continuación, cambie **Build System** (Sistema de compilación) a **Gradle** y active la opción **Export Project** (Exportar proyecto).

Seleccione **Switch Platform** (Cambiar plataforma) para cambiar la plataforma a **Android**. Unity puede pedirle que instale los componentes de compatibilidad de Android si no están presentes.

![Configuración de compilación de Unity](./media/spatial-anchors-unity/unity-android-build-settings.png)

Cierre la ventana **Build Settings** (Configuración de compilación).

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>Descargar e importar el SDK de ARCore para Unity

Descargue el archivo `unitypackage` de [ARCore SDK for Unity releases](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). De vuelta en el proyecto de Unity, seleccione **Assets** -> **Import Package** -> **Custom Package...** (Recursos > Importar paquete > Paquete personalizado) y seleccione el archivo `unitypackage` descargado anteriormente. En el cuadro de diálogo **Import Unity Package** (Importar paquete de Unity), asegúrese de que todos los archivos están seleccionados y, a continuación, seleccione **Import** (Importar).
