---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: e8daaaf5b6b15eb3095f11e94c707a33b4b18e28
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305148"
---
Seleccione **Build** (Compilar). En el cuadro de diálogo que aparece, seleccione la carpeta a la que va a exportar el proyecto Xcode.

Cuando se complete la exportación, aparecerá una carpeta que contiene el proyecto Xcode exportado.

> [!NOTE]
> Si aparece una ventana en la que se le pregunta si desea reemplazar o anexar, es aconsejable que seleccione **Append** (Anexar), porque es más rápido. Solo sería necesario seleccionar **Reemplazar** si va a cambiar los recursos de la escena. (Por ejemplo, si va a agregar, quitar, o cambiar las relaciones de elementos primarios y secundarios, o si va a agregar, quitar o cambiar las propiedades.) Si solo va a realizar cambios en el código fuente, **Anexar** debería ser suficiente.

### <a name="open-the-xcode-project"></a>Apertura del proyecto de Xcode

En la carpeta del proyecto Xcode exportado, ejecute este comando en el Terminal para instalar las instancias de CocoaPods necesarias para el proyecto:

```bash
pod install --repo-update
```

Ya puede abrir `Unity-iPhone.xcworkspace` para abrir el proyecto en Xcode:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Si ve un error `library not found for -lPods-Unity-iPhone`, es probable que haya abierto el archivo `.xcodeproj`, en lugar de `.xcworkspace`. 

Seleccione el nodo raíz **Unity-iPhone** para ver la configuración del proyecto y, después, seleccione la pestaña **General**.

En **Firma**, asegúrese de que **Automatically manage signing** (Administrar automáticamente la firma) está habilitado. Si no lo está, habilítelo y seleccione **Enable Automatic** (Habilitar automático) en el cuadro de diálogo que aparece para restablecer la configuración de compilación.

En **Deployment Info** (Información de implementación), asegúrese de que **Deployment Target** (Destino de la implementación) esté establecido en `11.0`.

### <a name="deploy-the-app-to-your-ios-device"></a>Implementación de la aplicación en el dispositivo iOS

Conecte el dispositivo iOS al equipo Mac y establezca el **esquema activo** en su dispositivo iOS.

![Selección del dispositivo](./media/spatial-anchors-unity/select-device.png)

Seleccione **Build and then run the current scheme** (Compilar y luego ejecutar el esquema actual).

![Implementación y ejecución](./media/spatial-anchors-unity/deploy-run.png)