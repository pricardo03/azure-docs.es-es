---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 53f480b8858e2bbe7d4699d8637ecaa5ab0c08a3
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305334"
---
En Unity, abra el proyecto en la carpeta `Unity`.

Seleccione **File** > **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En la sección **Platform** (Plataforma), seleccione **Android**. Cambie **Build System** (Sistema de compilación) a **Gradle** y seleccione **Export Project** (Exportar proyecto).

Seleccione **Switch Platform** (Cambiar plataforma) para cambiar la plataforma a **Android**. Unity puede solicitarle que instale los componentes de compatibilidad con Android si no están presentes.

![Ventana Build Settings (Configuración de compilación) de Unity](./media/spatial-anchors-unity/unity-android-build-settings.png)

Cierre la ventana **Build Settings** (Configuración de compilación).

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>Descargar e importar el SDK de ARCore para Unity

Descargue el archivo `unitypackage` de [ARCore SDK for Unity releases](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). De vuelta en el proyecto de Unity, seleccione **Assets** > **Import Package** > **Custom Package** (Recursos > Importar paquete > Paquete personalizado) y seleccione el archivo `unitypackage` que descargó anteriormente. En el cuadro de diálogo **Import Unity Package** (Importar paquete de Unity), asegúrese de que todos los archivos están seleccionados y, después, seleccione **Import** (Importar).
