---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 9798e5f76881be38fb27e1f428565caba6e50bf2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135077"
---
Seleccione **File** > **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En la sección **Platform** (Plataforma), seleccione **Android**. Cambie el **Sistema de compilación** a **Gradle** y asegúrese de que la casilla **Exportar proyecto** no tenga ninguna marca de verificación.

Seleccione **Switch Platform** (Cambiar plataforma) para cambiar la plataforma a **Android**. Unity puede solicitarle que instale los componentes de compatibilidad con Android si no están presentes.

![Ventana Build Settings (Configuración de compilación) de Unity](./media/spatial-anchors-unity/unity-android-build-settings.png)

Cierre la ventana **Build Settings** (Configuración de compilación).

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>Descargar e importar el SDK de ARCore para Unity

Descargue el archivo `unitypackage` de las [versiones de Unity 1.7 para ARCore SDK](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.7.0). De vuelta en el proyecto de Unity, seleccione **Assets** > **Import Package** > **Custom Package** (Recursos > Importar paquete > Paquete personalizado) y seleccione el archivo `unitypackage` que descargó anteriormente. En el cuadro de diálogo **Import Unity Package** (Importar paquete de Unity), asegúrese de que todos los archivos están seleccionados y, después, seleccione **Import** (Importar).
