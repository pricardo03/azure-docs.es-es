---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: b802c9dbd0cef65325cb03538b68b49c57b85bb3
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56891008"
---
Seleccione **Compilar** para abrir un cuadro de diálogo. A continuación, seleccione una carpeta para exportar el proyecto de Xcode.

Una vez completada la exportación, se mostrará una carpeta que contiene el proyecto de Xcode exportado.

### <a name="open-the-xcode-project"></a>Apertura del proyecto de Xcode

En la carpeta del proyecto Xcode exportado, ejecute el siguiente comando para instalar las instancias de CocoaPods necesarias para el proyecto:

```bash
pod install --repo-update
```

Ahora puede abrir `Unity-iPhone.xcworkspace` para abrir el proyecto en Xcode:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Si ve un error `library not found for -lPods-Unity-iPhone`, es probable que haya abierto el archivo `.xcodeproj` en lugar de `.xcworkspace`. Abra `.xcworkspace` e inténtelo de nuevo.

Seleccione el nodo raíz **Unity-iPhone** para ver la configuración del proyecto y seleccione la pestaña **General**.

En **Signing** (Firma), seleccione **Automatically manage signing** (Administrar la firma automáticamente). Seleccione **Enable Automatic** (Habilitar automática) en el cuadro de diálogo que aparece para restablecer la configuración de compilación.

En **Deployment Info** (Información de implementación), asegúrese de que **Deployment Target** (Destino de la implementación) esté establecido en `11.0`.

### <a name="deploy-the-app-to-your-ios-device"></a>Implementación de la aplicación en el dispositivo iOS

Conecte el dispositivo iOS al equipo Mac y establezca el **esquema activo** en su dispositivo iOS.

![Selección del dispositivo](./media/spatial-anchors-unity/select-device.png)

Seleccione **Build and then run the current scheme** (Compilar y luego ejecutar el esquema actual).

![Implementación y ejecución](./media/spatial-anchors-unity/deploy-run.png)